+++
title = 'OAuth'
weight = '540'
draft = false
+++
-------------

## OAuth et délégation d'accès

Le protocole **OAuth 2.0** est couramment utilisé lorsqu'une application doit accéder à certaines ressources appartenant à un utilisateur, **sans que celui-ci lui fournisse son mot de passe**.

*OAuth* ne sert pas principalement à dire « **qui est cet utilisateur ?** », mais plutôt :

> **« Qu'est-ce que cette application est autorisée à faire en son nom ? »**

###### Exemple : Application qui accède à Google Drive

Supposons qu'une application souhaite permettre à un utilisateur de sélectionner un fichier stocké dans son Google Drive.

Au lieu de demander le mot de passe Google de l'utilisateur, l'application peut utiliser *OAuth* :

![Exemple de flux OAuth avec Google Drive](/images/05-flux-oauth.png)

1. L'application demande l'autorisation d'accéder à certaines ressources.
2. L'utilisateur est redirigé vers le fournisseur de services.
3. L'utilisateur s'authentifie auprès de ce fournisseur.
4. Le fournisseur demande à l'utilisateur s'il autorise l'application à accéder aux ressources demandées.
5. L'utilisateur accepte.
6. L'application reçoit un jeton d'accès (access token).
7. L'application utilise ce jeton pour accéder aux ressources autorisées.

Le jeton peut être limité à certaines permissions. Par exemple, une application pourrait être autorisée à **lire** certains fichiers sans être autorisée à les modifier.

{{%notice style="info" title="OAuth ≠ authentification"%}}

**OAuth est un mécanisme d'autorisation et de délégation d'accès**, pas un protocole d'authentification à proprement parler.

Lorsqu'un service veut utiliser OAuth pour permettre à un utilisateur de se connecter avec son compte Google ou Microsoft, il utilise généralement **OpenID Connect (OIDC)** par-dessus *OAuth*.

On peut donc retenir :

- **OAuth → « Qu'est-ce que cette application peut faire ? »**
- **OIDC → « Qui est l'utilisateur ? »**

{{%/notice%}}

