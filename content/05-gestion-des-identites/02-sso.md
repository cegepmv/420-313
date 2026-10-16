+++
title = 'SSO'
weight = '520'
draft = false
+++
-------------

## Authentification unique (SSO) et fédération d'identité

Le **SSO** (*Single Sign-On*) permet à un utilisateur de s'authentifier **une seule fois**, auprès d'un fournisseur d'identité pour ensuite accéder à plusieurs services sans devoir se réauthentifier auprès de chacun d'eux.

<!-- La **fédération d'identité** étend ce principe entre organisations distinctes. -->



Par exemple, dans un environnement scolaire ou professionnel, un même compte peut permettre d'accéder à :

- la messagerie ;
- une plateforme d'apprentissage ;
- un espace de stockage ;
- des applications administratives ;
- différents services internes.

L'utilisateur ne possède donc pas nécessairement un mot de passe différent pour chaque service.

##### Exemple : connexion avec un compte Microsoft

Un utilisateur se rend sur une application qui utilise son compte Microsoft.

Le processus peut être résumé ainsi :

![Exemple de flux SSO](/images/05-flux-sso.png?width=50rem)


1. L'utilisateur tente d'accéder à l'application.
2. L'application constate qu'il n'est pas authentifié.
3. Elle redirige l'utilisateur vers le fournisseur d'identité (*Identity Provider* ou *IdP*).
4. L'utilisateur s'authentifie auprès du fournisseur d'identité, par exemple avec son mot de passe et une MFA.
5. Le fournisseur d'identité confirme son identité et renvoie une preuve d'authentification à l'application.
6. L'application vérifie cette preuve et crée une session pour l'utilisateur.
7. L'utilisateur peut maintenant utiliser l'application sans fournir à nouveau son mot de passe.

{{%notice style="tip" title="Point important"%}}

Avec le SSO, l'application ne reçoit généralement **pas le mot de passe de l'utilisateur**.

L'authentification est effectuée par le fournisseur d'identité, qui transmet ensuite à l'application une preuve permettant d'établir l'identité de l'utilisateur.

{{%/notice%}}

#### SSO et fédération d'identité

Le **SSO** concerne principalement l'expérience d'authentification : une authentification permet d'accéder à plusieurs services.

La **fédération d'identité** permet quant à elle de faire confiance à l'identité fournie par une autre organisation ou un autre domaine.

Par exemple, une application pourrait permettre à ses utilisateurs d'accéder à leur compte en utilisant leur compte Google (*Se connecter avec Google*). L'application fait alors confiance au fournisseur d'identité (Google) pour effectuer l'authentification.
