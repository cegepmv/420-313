+++
title = 'Sessions et jetons'
weight = '530'
draft = false
+++
-------------

Une fois qu'un utilisateur s'est authentifié, l'application doit pouvoir reconnaître cet utilisateur lors de ses requêtes suivantes.

Il serait inefficace et peu sécuritaire de demander le mot de passe à chaque requête. Les applications utilisent donc généralement une **session** ou un **jeton** (*token*) permettant de conserver la preuve de l'authentification.

## Session

Dans une application Web traditionnelle, le serveur peut créer une **session** après l'authentification de l'utilisateur.

Le processus peut être résumé ainsi :

![Flux d'authentification par session](/images/05-flux-session.png)

1. L'utilisateur fournit ses informations d'authentification.
2. Le serveur vérifie son identité.
3. Le serveur crée une session associée à cet utilisateur.
4. Le serveur transmet au navigateur un identifiant de session, généralement conservé dans un cookie.
5. Pour les requêtes suivantes, le navigateur renvoie automatiquement ce cookie.
6. Le serveur utilise l'identifiant pour retrouver la session et déterminer quel utilisateur effectue la requête.

<!-- On peut représenter le processus ainsi :

Utilisateur → authentification → serveur

Serveur → création de session → navigateur

Navigateur → identifiant de session → serveur

Serveur → retrouve la session → utilisateur authentifié -->

L'identifiant de session ne devrait pas contenir directement des informations sensibles comme le mot de passe. Il sert plutôt de **référence vers une session conservée par le serveur**.

{{%notice style="warning" title="Vol de session"%}}

Si un attaquant obtient un identifiant de session valide, il peut parfois l'utiliser pour se faire passer pour l'utilisateur sans connaître son mot de passe.

La protection des cookies de session est donc importante. Des mécanismes comme **HTTPS**, `Secure`, `HttpOnly` et `SameSite` permettent notamment de réduire certains risques.

{{%/notice%}}

## Jetons d'accès

Dans de nombreuses architectures modernes, notamment avec les API et OAuth, on utilise plutôt des **jetons d'accès** (*access tokens*).

Un jeton représente une autorisation accordée à une application ou à un utilisateur. L'application peut présenter ce jeton lorsqu'elle souhaite accéder à une ressource protégée.

Par exemple :

![Flux d'authentification par jeton d'accès](/images/05-flux-token.png)


1. L'utilisateur s'authentifie auprès d'un fournisseur d'identité.
2. Il autorise une application à accéder à certaines ressources.
3. Le fournisseur délivre un jeton d'accès.
4. L'application présente ce jeton lorsqu'elle appelle une API.
5. L'API vérifie le jeton et détermine si l'accès demandé est autorisé.

Le mot de passe de l'utilisateur n'est donc pas transmis à chaque application ou à chaque requête.

{{%notice style="info" title="Session, jeton et mot de passe"%}}

Ces trois éléments ont des rôles différents :

- **Mot de passe :** permet de prouver son identité lors de l'authentification.
- **Session :** permet au serveur de conserver l'état d'une connexion authentifiée.
- **Jeton d'accès :** représente une autorisation permettant d'accéder à certaines ressources.

<!-- Dans la pratique, les architectures peuvent combiner plusieurs de ces mécanismes. -->

{{%/notice%}}

## Expiration et révocation

Les sessions et les jetons ne devraient généralement pas être valides indéfiniment.

Une application peut notamment :

- faire expirer une session après une certaine période d'inactivité ;
- faire expirer un jeton après une durée limitée ;
- révoquer une session lorsqu'un utilisateur se déconnecte ;
- révoquer les accès lorsqu'un compte est compromis ou qu'une permission est retirée.

Cela permet de limiter les conséquences d'un identifiant ou d'un jeton volé.