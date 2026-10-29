+++
pre = '<b>07. </b>'
title = 'Sécurité des applications'
weight = '700'
draft = false
+++
---------

Les chapitres précédents ont porté sur la protection des **données** (chapitre 4), des **identités** (chapitre 5) et des **postes de travail** (chapitre 6). Ce chapitre s'intéresse aux **applications**, en particulier les **applications Web** : comment des erreurs de programmation permettent de lire une base de données, de voler une session ou de faire agir un utilisateur à son insu, et comment s'en protéger.

{{%notice style="warning" title="Cadre éthique et légal"%}}
Les techniques présentées sont étudiées **pour apprendre à s'en défendre**. Tester une attaque contre un système sans l'autorisation de son propriétaire est **illégal** (au Canada, notamment l'article 342.1 du *Code criminel*). Les manipulations se font **uniquement** dans l'environnement de laboratoire. L'exploitation approfondie sera reprise dans le cours *Piratage éthique*.
{{%/notice%}}

## Pourquoi la sécurité applicative est cruciale

Rappel du chapitre 2 : les produits et services informatiques **ne sont pas naturellement sûrs**. Une grande partie des **vulnérabilités** (chapitre 6) corrigées par les mises à jour sont des *erreurs de programmation*.

Une application Web est particulièrement exposée : elle est souvent **accessible publiquement**, elle reçoit des **données fournies par des inconnus** (formulaires, paramètres d'URL, cookies, fichiers) et elle est reliée à des **ressources sensibles** (base de données, comptes, fichiers).

![Surface d'attaque d'une application Web : les entrées contrôlées par l'attaquant (formulaires, paramètres d'URL, cookies, fichiers téléversés) entre le navigateur et le serveur](/images/07-surface-attaque.svg)

{{%notice style="info" title="Règle d'or : ne jamais faire confiance au client"%}}
Tout ce qui se trouve **dans le navigateur** (HTML, JavaScript, champs cachés, cookies, longueur maximale d'un champ...) peut être lu et modifié par l'utilisateur avec les outils de développement (**F12**).

Une validation en JavaScript améliore l'expérience utilisateur, mais **n'apporte aucune sécurité**. Toute vérification de sécurité doit être faite **côté serveur**.
{{%/notice%}}

## Rappels : cookies et sessions

Comme vu au **chapitre 5 (section 5.3)**, pour garder une trace d'authentification et éviter de s'authentifier à chaque requête, l'application crée une **session** dont l'identifiant est conservé dans un **cookie**.

<!-- Le protocole **HTTP** est **sans état** : chaque requête est indépendante.  -->

Le serveur crée le cookie avec l'en-tête `Set-Cookie` :

```YAML
Set-Cookie: SESSIONID=8f3a9c1e7b2d4f60a1c5; Secure; HttpOnly; SameSite=Lax
```

Ensuite, le navigateur le **renvoie automatiquement** à chaque requête vers ce site :

```YAML
Cookie: SESSIONID=8f3a9c1e7b2d4f60a1c5
```

{{%notice style="tip" title="Le point clé"%}}
**Quiconque possède le cookie de session est, aux yeux du serveur, l'utilisateur authentifié.** Et ce cookie est envoyé **automatiquement**, sans action de l'utilisateur. Plusieurs attaques de ce chapitre exploitent ces deux faits.
{{%/notice%}}

## Attributs de sécurité des cookies

| Attribut | Rôle | Protège contre |
|---|---|---|
| `Secure` | Cookie envoyé **uniquement en HTTPS** | Interception sur le réseau |
| `HttpOnly` | Cookie **inaccessible au JavaScript** | Vol par **XSS** |
| `SameSite=Strict` / `Lax` | Cookie non envoyé (ou envoyé seulement lors d'un simple clic de lien) quand la requête provient d'un **autre site** | **CSRF** |
| `Max-Age` / `Expires` | Durée de vie limitée | Réutilisation d'un cookie volé |

## Politique de même origine

Le navigateur empêche le JavaScript d'une page de **lire** les données d'une autre **origine** (schéma + hôte + port). Par exemple, `https://site-pirate.com` ne peut pas lire votre boîte courriel ouverte dans un autre onglet.

Deux attaques contournent cette protection :

- le **XSS**, en exécutant du code **à l'intérieur** du site légitime ;
- le **CSRF**, qui n'a pas besoin de *lire* la réponse : il lui suffit d'*envoyer* une requête.