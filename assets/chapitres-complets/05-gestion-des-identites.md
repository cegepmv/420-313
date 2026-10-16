# 5 - Gestion des identités
## Authentification et autorisation

- **Authentification** : confirmer qu'un utilisateur est bien celui qu'il prétend être.
- **Autorisation** : déterminer ce qu'un utilisateur authentifié a le droit de faire ou de consulter.

{{%notice style="info" title="Deux notions distinctes"%}}
Ces deux notions, bien que souvent confondues, sont *distinctes* : on peut être authentifié sans être autorisé à effectuer une action donnée.

**Exemple :** un étudiant peut s'authentifier avec son compte du cégep, mais ne pas être autorisé à modifier les notes d'un cours. Un enseignant, avec le même système d'authentification, pourrait avoir cette permission.
{{%/notice%}}

## Facteurs d'authentification

![Les trois facteurs d'authentification côte à côte](/images/05-MFA.png?width=48rem)

L'authentification peut reposer sur différents **facteurs** :

- **Ce que je sais** : mot de passe, NIP, question secrète.
- **Ce que je possède** : téléphone, jeton matériel, carte à puce.
- **Ce que je suis** : biométrie (empreinte digitale, reconnaissance faciale).

L'**authentification multifacteur (MFA/2FA)** combine au moins deux facteurs d'authentification différents..

Par exemple, un utilisateur peut devoir fournir :

1. son mot de passe (**ce que je sais**) ;
2. puis un code généré par une application d'authentification ou une clé de sécurité (**ce que je possède**).

Cela réduit considérablement le risque qu'un attaquant compromette un compte avec un seul élément volé, par exemple un mot de passe divulgué lors d'une violation de données.

# 5.1 - Authentification par clé

Comme vu au chapitre 4, une **clé privée** peut servir à authentifier un utilisateur.

L'idée est de démontrer que l'utilisateur possède la clé privée associée à une clé publique connue, sans avoir à transmettre la clé privée elle-même.

Par exemple, avec **SSH**, un utilisateur peut posséder une paire de clés :

- une **clé publique**, enregistrée sur le serveur ;
- une **clé privée**, conservée par l'utilisateur.

Lors de la connexion, le serveur peut vérifier que l'utilisateur possède bien la clé privée correspondante. **La clé privée n'est jamais envoyée au serveur**.

{{%notice style="tip" title=" "%}}
Comme vu dans le laboratoire sur SSH du chapitre 4, cette approche permet notamment d'éviter les problèmes associés aux mots de passe et peut être particulièrement utile pour l'administration de serveurs.
{{%/notice%}}

# 5.2 - SSO

## Authentification unique (SSO) et fédération d'identité

Le **SSO** (*Single Sign-On*) permet à un utilisateur de s'authentifier **une seule fois**, auprès d'un fournisseur d'identité pour ensuite accéder à plusieurs services sans devoir se réauthentifier auprès de chacun d'eux.

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

# 5.3 - Sessions et jetons

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


## Expiration et révocation

Les sessions et les jetons ne devraient généralement pas être valides indéfiniment.

Une application peut notamment :

- faire expirer une session après une certaine période d'inactivité ;
- faire expirer un jeton après une durée limitée ;
- révoquer une session lorsqu'un utilisateur se déconnecte ;
- révoquer les accès lorsqu'un compte est compromis ou qu'une permission est retirée.

Cela permet de limiter les conséquences d'un identifiant ou d'un jeton volé.

# 5.4 - OAuth

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


# 5.5 - Comptes et permissions

Une fois l'utilisateur authentifié, le système doit déterminer **quelles ressources et quelles actions lui sont accessibles**.

La gestion des permissions repose notamment sur deux principes importants :

- le **principe du moindre privilège**;
- le **contrôle d'accès basé sur les rôles (RBAC)**.

### Principe du moindre privilège

Le **principe du moindre privilège** (*Principle of Least Privilege*) consiste à donner à chaque utilisateur, programme ou processus **uniquement les droits nécessaires à l'exécution de ses tâches**.

Un utilisateur ne devrait donc pas posséder des privilèges administratifs simplement parce qu'ils pourraient être utiles occasionnellement.

###### Exemple : poste informatique d'un cégep

Un enseignant utilise quotidiennement son ordinateur pour :

- préparer ses cours ;
- consulter ses courriels ;
- utiliser des logiciels pédagogiques ;
- accéder aux ressources du réseau.

Il n'a cependant pas besoin de pouvoir :

- modifier les comptes des autres utilisateurs ;
- installer n'importe quel logiciel sur tous les ordinateurs du cégep ;
- modifier la configuration des serveurs ;
- accéder aux dossiers confidentiels du service des ressources humaines.

Ces permissions devraient être réservées aux personnes qui en ont réellement besoin.

Ainsi, même si le compte de l'enseignant est compromis, les possibilités offertes à l'attaquant sont limitées.

{{%notice style="tip" title="Le moindre privilège s'applique aussi aux programmes"%}}

Le principe du moindre privilège ne concerne pas uniquement les utilisateurs.

Un programme ou un service devrait lui aussi fonctionner avec **le minimum de permissions nécessaires**.

Par exemple, une application Web qui doit seulement lire des données dans une base de données ne devrait pas nécessairement disposer des permissions permettant de supprimer ou de modifier toutes les données.
{{%/notice%}}

### RBAC : contrôle d'accès basé sur les rôles

Le **RBAC** (*Role-Based Access Control*) consiste à attribuer des permissions à des **rôles**, puis à attribuer ces rôles aux utilisateurs.

Plutôt que de gérer individuellement les permissions de chaque utilisateur, on définit des rôles correspondant aux responsabilités de l'organisation.

###### Exemple : système informatique d'un cégep

Imaginons un système permettant de gérer les informations scolaires.

On pourrait définir les rôles suivants :

|Rôle|	Permissions|
|----|-------------|
|**Étudiant**|	Consulter ses cours, remettre des travaux, consulter ses notes|
|**Enseignant**|	Consulter ses groupes, créer des évaluations, saisir les notes|
|**Technicien informatique**|	Gérer les comptes, administrer les postes et certains services|
|**Administrateur système**	|Administrer les serveurs et l'ensemble de l'infrastructure|

Un utilisateur peut ensuite recevoir un ou plusieurs rôles selon ses responsabilités.

Par exemple, lorsqu'un nouvel enseignant est embauché, on lui attribue le rôle **Enseignant**. Il reçoit automatiquement les permissions associées à ce rôle.

Lorsqu'un enseignant quitte l'établissement, son compte ou son rôle peut être désactivé sans devoir rechercher manuellement toutes les permissions qui lui avaient été accordées.

{{%notice style="info" title="RBAC et moindre privilège"%}}

Le **RBAC** et le **principe du moindre privilège** sont complémentaires :

+ RBAC permet de structurer et de gérer les permissions à l'aide de rôles.
+ Le moindre privilège permet de s'assurer que ces rôles ne donnent **pas plus de permissions que nécessaire**.

{{%/notice%}}

# 5.6 - Mots de passe

### Choix d'un bon mot de passe

Trois règles principales :

1. Éviter les mots présents dans un dictionnaire et les mots de passe courants comme « 12345 » ou « qwerty ».
2. Privilégier des mots de passe **longs**.
3. Utiliser un mot de passe difficile à deviner et éviter les informations personnelles facilement accessibles.

{{%notice style="info" title="Complexe ≠ compliqué"%}}
Un mot de passe long et aléatoire est généralement beaucoup plus difficile à attaquer qu'un mot de passe court auquel on a simplement ajouté quelques chiffres et symboles.
{{%/notice%}}


Pour les comptes importants, l'utilisation d'un **gestionnaire de mots de passe** permet également de générer et de conserver des mots de passe longs et uniques.

### Ordres de grandeur (force brute)

![Table comparant le temps de force brute selon la longueur et le jeu de caractères utilisé.](/images/05-hive-systems-password-table-2026.png?width=35rem)

Le nombre de combinaisons possibles pour un mot de passe dépend du nombre de caractères disponibles et de sa longueur.

Si le jeu de caractères contient **N** caractères et que le mot de passe possède une longueur **L**, le nombre de combinaisons possibles est : **Nᴸ**

Par exemple :
- un mot de passe numérique de 8 chiffres possède **10⁸ = 100 millions** de combinaisons ;
- avec 62 caractères possibles (majuscules, minuscules et chiffres), un mot de passe de 8 caractères possède environ **218 000 milliards** de combinaisons ;
- avec 94 caractères possibles (majuscules, minuscules, chiffres et symboles), un mot de passe de 8 caractères possède environ **6 000 milliards de milliards** de combinaisons.

Cependant, le temps nécessaire pour tester ces combinaisons dépend fortement du **type d'attaque**, du matériel utilisé et surtout de la manière dont le mot de passe est protégé.

Une attaque contre un service en ligne est généralement limitée par des mécanismes comme la limitation du nombre de tentatives.

À l'inverse, lorsqu'un attaquant obtient une copie de mots de passe **hachés**, il peut parfois effectuer beaucoup plus de tentatives hors ligne.

Ces calculs montrent l'importance de la **longueur** et de l'unicité d'un mot de passe.

{{%notice style="info" title="En pratique"%}}
En pratique, cependant, un attaquant ne teste pas nécessairement toutes les combinaisons possibles. Les mots de passe courants, les informations personnelles et les modèles prévisibles peuvent être testés en priorité (attaque par dictionnaire).

Un mot de passe long mais prévisible, comme `MotDePasse123456`, peut donc être beaucoup plus facile à trouver qu'un mot de passe généré aléatoirement.
{{%/notice%}}

{{%notice style="tip" title=" "%}}
L'**authentification multifacteur** demeure par conséquent une protection importante, même lorsqu'un utilisateur possède un mot de passe robuste.
{{%/notice%}}

# 5.7 - Attaques liées à l'identité

Les systèmes d'authentification et d'autorisation sont des cibles importantes pour les attaquants. Plusieurs attaques cherchent directement à **voler une identité**, à **contourner l'authentification** ou à **obtenir des privilèges supplémentaires**.

## Hameçonnage (phishing)

Le **hameçonnage** consiste à tromper un utilisateur afin qu'il fournisse volontairement des informations sensibles, par exemple son mot de passe ou un code d'authentification.


L'attaquant peut créer une fausse page de connexion ressemblant à celle d'un service légitime.

**Exemple :**
1. Un utilisateur reçoit un courriel lui demandant de se connecter à son compte. Le lien mène vers un faux site qui reproduit l'apparence du véritable service.
2. L'utilisateur saisit alors ses identifiants, qui sont transmis à l'attaquant.


## Bourrage d'identifiants (*credential stuffing*)

Lors d'une violation de données, des listes de noms d'utilisateur et de mots de passe peuvent être divulguées.

Si un utilisateur réutilise le même mot de passe sur plusieurs services, un attaquant peut essayer les identifiants volés sur d'autres plateformes.

Cette technique est appelée **credential stuffing**.

{{%notice style="tip" title=" "%}}
L'utilisation de **mots de passe uniques** pour chaque service et de la **MFA** permet de réduire considérablement ce risque.
{{%/notice%}}

## Vol de session

Comme vu précédemment, une session permet à une application de reconnaître un utilisateur déjà authentifié.

Si un attaquant parvient à obtenir un **identifiant de session valide**, il peut potentiellement utiliser cette session pour agir au nom de la victime.

Ce type d'attaque est appelé **vol de session** (*session hijacking*).

{{%notice style="tip" title=" "%}}
La protection des communications avec **HTTPS**, la sécurisation des cookies et l'expiration des sessions sont parmi les mécanismes permettant de réduire ce risque.
{{%/notice%}}

## Fatigue MFA

La MFA augmente considérablement la sécurité d'un compte, mais elle ne rend pas l'utilisateur invulnérable.

Dans une attaque de **fatigue MFA** (*MFA fatigue*), l'attaquant dispose déjà du mot de passe de la victime et provoque de nombreuses demandes d'approbation MFA sur son téléphone.

L'objectif est de pousser l'utilisateur à accepter accidentellement une demande, simplement pour faire cesser les notifications.

{{%notice style="tip" title=" "%}}
Des mécanismes comme l'affichage d'un **code de correspondance** (*number matching*) et la sensibilisation des utilisateurs permettent notamment de réduire ce risque.
{{%/notice%}}


## Escalade de privilèges

Une fois qu'un attaquant a compromis un compte, il peut chercher à obtenir des permissions supplémentaires.

On parle alors d'**escalade de privilèges**.

Par exemple, un compte compromis possédant uniquement des droits de lecture pourrait être utilisé pour tenter d'obtenir des privilèges permettant de modifier des données ou d'administrer un serveur.

C'est notamment pour cette raison que le **principe du moindre privilège** est important : même lorsqu'un compte est compromis, les permissions disponibles pour l'attaquant restent limitées.


{{%notice style="info" title="Des attaques qui se complètent"%}}
Ces attaques peuvent être combinées.

Par exemple :

**Phishing → vol du mot de passe → connexion au compte → tentative de contournement de la MFA → vol de session → escalade de privilèges**

La sécurité des identités ne repose donc pas sur une seule mesure. Elle repose sur plusieurs couches de protection : mots de passe uniques, MFA, gestion des sessions, moindre privilège, RBAC, surveillance et sensibilisation des utilisateurs.

{{%/notice%}}