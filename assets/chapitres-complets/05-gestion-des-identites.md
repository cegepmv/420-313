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



# 5.2. Laboratoire : Cassage de mots de passe

## Objectifs

Dans ce laboratoire, vous allez expérimenter différentes techniques permettant de retrouver un mot de passe à partir de sa représentation hachée.

À la fin du laboratoire, vous serez capable de :

- expliquer la différence entre une attaque par dictionnaire et une attaque par force brute ;
- utiliser **John the Ripper** pour effectuer des attaques sur des mots de passe hachés ;
- observer l'effet de la longueur et de la composition d'un mot de passe sur le temps nécessaire pour le retrouver ;
- expliquer pourquoi certains mots de passe sont beaucoup plus faciles à retrouver que d'autres ;
- distinguer une attaque contre un mot de passe haché hors ligne d'une attaque directement contre un service en ligne ;
- expliquer pourquoi la longueur et l'unicité d'un mot de passe sont importantes.

{{%notice style="warning" title="Important: Environnement de laboratoire"%}}
Toutes les manipulations de ce laboratoire doivent être effectuées uniquement sur les fichiers, comptes et machines fournis pour le laboratoire.
Le but est d'expérimenter avec des mots de passe fictifs dans un environnement contrôlé. **Vous ne devez pas tenter de casser les mots de passe d'un autre utilisateur ou d'un système qui ne vous appartient pas**.
{{%/notice%}}

### Préparation de l'environnement
Vous disposez de la VM **Ubuntu** *jump server* fournie lors des laboratoires du chapitre *4. Cryptographie*. Vous allez d'abord y installer **John the Ripper (version jumbo)**, puis télécharger les fichiers du laboratoire.

##### 1. Installer John the Ripper (jumbo)

La version présente dans les dépôts d'Ubuntu (paquet `john`) est une version de base plus limitée. Nous allons plutôt compiler la version **jumbo**, qui prend notamment en charge l'option `--mask` et la détection automatique de nombreux formats de hachage.

Installez d'abord les dépendances nécessaires à la compilation :

```bash
sudo apt update
sudo apt install -y build-essential git libssl-dev zlib1g-dev yasm pkg-config \
    libgmp-dev libpcap-dev libbz2-dev libomp-dev unzip
```

Récupérez ensuite le code source et compilez-le :

```bash
git clone https://github.com/openwall/john -b bleeding-jumbo john
cd john/src
./configure
make -s clean && make -sj4
```

La compilation peut prendre quelques minutes. Une fois terminée, l'exécutable se trouve dans le dossier `john/run`.

Placez-vous dans ce dossier pour la suite du laboratoire :

```bash
cd ../run
```

Vérifiez que John the Ripper fonctionne :

```bash
./john --version
```

Vous devriez obtenir une information indiquant la version installée (elle doit mentionner **jumbo**).

Vous pouvez également afficher l'aide :

```bash
./john --help
```

{{%notice style="tip" title="Rendre la commande plus pratique"%}}
Dans la suite de l'énoncé, les commandes sont écrites `john ...`. Comme l'exécutable se trouve dans `john/run`, vous devrez soit lancer les commandes depuis ce dossier avec `./john ...`, soit ajouter un alias, par exemple :

```bash
alias john=~/john/run/john
```
{{%/notice%}}

##### 2. Télécharger les fichiers du laboratoire

Les fichiers nécessaires au laboratoire sont fournis dans une archive. Téléchargez-la puis décompressez-la :

```bash
wget https://github.com/cegepmv/420-313/raw/refs/heads/main/assets/files/labo-cassage-mdp.zip -O labo-cassage-mdp.zip
unzip labo-cassage-mdp.zip -d labo-mdp
cd labo-mdp
```

<!-- {{%notice style="info" title="Si unzip n'est pas installé"%}}
```bash
sudo apt install -y unzip
```
{{%/notice%}} -->

L'archive contient les fichiers suivants :

```text
hashes.txt
hashes_predictibles.txt
hashes_numeric.txt
hashes_comparaison.txt
wordlist.txt
wordlist_predictible.txt
```

##### 3. Examiner le fichier de travail

Un fichier nommé `hashes.txt` vous est fourni.

Il contient plusieurs représentations hachées de mots de passe fictifs.

Examinez son contenu :

```bash
cat hashes.txt
```

Chaque ligne est de la forme `nom_utilisateur:hachage`. Les mots de passe sont hachés avec **bcrypt** (les hachages commencent par `$2b$`).

**Question :** *Pourquoi un système sécurisé ne devrait-il normalement pas conserver les mots de passe des utilisateurs directement en clair ?*

{{%notice style="tip" title="Recommencer une attaque"%}}
John "se souvient" des mots de passe déjà trouvés dans un fichier appelé *pot* (`~/john/run/john.pot`). Si vous voulez recommencer une attaque à zéro, supprimez ce fichier :

```bash
rm ~/john/run/john.pot
```
{{%/notice%}}



# 5.2.1. Attaque par dictionnaire
-----------

Une attaque par dictionnaire consiste à essayer successivement des mots de passe provenant d'une liste.

L'attaquant ne teste donc pas nécessairement toutes les combinaisons possibles. Il commence plutôt par des valeurs qui ont de fortes chances d'être utilisées.

Le cours donne notamment comme exemples des mots de passe courants comme `12345`, `qwerty` ou d'autres mots présents dans les dictionnaires.

### 1. Préparer un dictionnaire

Un fichier `wordlist.txt` vous a été fourni dans l'archive.

Examinez son contenu :

```bash
cat wordlist.txt
```

Vous pouvez compter le nombre de mots présents dans le fichier :

```bash
wc -l wordlist.txt
```

**Question :** *Pourquoi une attaque par dictionnaire peut-elle être beaucoup plus efficace qu'une force brute lorsque les utilisateurs choisissent des mots de passe prévisibles ?*

### 2. Lancer John the Ripper

Lancez une attaque par dictionnaire contre le fichier de hashes :

```bash
john hashes.txt --wordlist=wordlist.txt
```

Observez les résultats.

Une fois l'attaque terminée, affichez les mots de passe retrouvés :

```bash
john --show hashes.txt
```

Notez les résultats dans le tableau suivant.

| Compte / hash | Mot de passe retrouvé ? | Mot de passe | Temps approximatif |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |

**Question :** *Est-ce que tous les mots de passe ont été retrouvés ? Expliquez pourquoi certains peuvent être retrouvés par dictionnaire et d'autres non.*

### 3. Influence de la prévisibilité

Les mots de passe ne sont pas toujours de simples mots du dictionnaire.

Les utilisateurs ont souvent tendance à modifier légèrement un mot de passe :

```text
password
password1
Password1
Password123
```

ou à ajouter une année, un symbole ou quelques chiffres.

Vous disposez maintenant d'un deuxième fichier :

```text
hashes_predictibles.txt
```

et d'une liste :

```text
wordlist_predictible.txt
```

Lancez une nouvelle attaque :

```bash
john hashes_predictibles.txt --wordlist=wordlist_predictible.txt
```

Puis :

```bash
john --show hashes_predictibles.txt
```

{{%notice style="tip" title="Aller plus loin : les règles"%}}
La version jumbo peut générer automatiquement ce genre de variantes à partir d'une liste de mots de base, grâce aux **règles** (*mangling rules*) :

```bash
john hashes_predictibles.txt --wordlist=wordlist.txt --rules
```

À la fin du laboatoire, essayez cette technique puis comparez le résultat avec l'attaque précédente. Ne l'essayez pas tout de suite, car cette attaque peut prendre beaucoup de temps !
{{%/notice%}}

**Questions :**
1. *Pourquoi un mot de passe comme `Password123` peut-il être beaucoup plus facile à retrouver qu'un mot de passe aléatoire de longueur comparable ?*
2. *Le fait d'ajouter une majuscule, un chiffre ou un symbole rend-il automatiquement un mot de passe robuste ? Justifiez votre réponse.*



# 5.2.2. Attaque par force brute
-----------

Une attaque par force brute consiste à essayer systématiquement différentes combinaisons de caractères.

Contrairement à l'attaque par dictionnaire, elle ne dépend pas de la présence du mot de passe dans une liste de mots connus.

Dans cette partie, nous allons volontairement utiliser de très petits espaces de recherche afin que l'expérience puisse être réalisée dans un temps raisonnable.

### 1. Mots de passe numériques

Vous allez tester des mots de passe composés uniquement de chiffres.

Considérons les trois longueurs suivantes :

```text
2 chiffres
3 chiffres
4 chiffres
```

Le nombre de combinaisons possibles peut être calculé avec :
```math
\[
N^L
\]
```
où :

- `N` est le nombre de caractères possibles ;
- `L` est la longueur du mot de passe.

Pour un mot de passe numérique :
```math
\[
N = 10
\]
```

Complétez :

| Longueur | Calcul | Nombre de possibilités |
|---|------|-------|
| 2 | \(10^2\) | |
| 3 | \(10^3\) | |
| 4 | \(10^4\) | |
| 5 | \(10^5\) | |
| 6 | \(10^6\) | |

**Question :** *Que remarquez-vous lorsque la longueur augmente d'un seul caractère ?*

### 2. Expérimentation

Un fichier `hashes_numeric.txt` vous est fourni.

Il contient plusieurs hashes correspondant à des mots de passe numériques de différentes longueurs (de 2 à 6 chiffres).

Avec **John the Ripper**, vous pouvez effectuer une recherche exhaustive sur un espace uniquement composé de chiffres à l'aide de l'option `--mask`. Le symbole `?d` représente un chiffre :

```bash
john hashes_numeric.txt --mask='?d' --min-len=2 --max-len=6
```

Affichez les résultats :

```bash
john --show hashes_numeric.txt
```

{{%notice style="tip" title="Observer une seule longueur à la fois"%}}
Pour mieux mesurer l'effet de la longueur, vous pouvez lancer l'attaque longueur par longueur (pensez à supprimer `~/.john/john.pot` entre chaque essai) :

```bash
john hashes_numeric.txt --mask='?d' --min-len=4 --max-len=4
```
{{%/notice%}}

Notez le temps nécessaire pour retrouver chaque mot de passe.

| Longueur | Mot de passe retrouvé ? | Temps nécessaire |
|---|---|---|
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| 6 | | |


**Questions :**
1. *Les résultats expérimentaux correspondent-ils à ce que vous aviez prévu à partir du nombre de combinaisons ?*
2. *Pourquoi serait-il imprudent d'extrapoler directement votre temps de laboratoire à un ordinateur moderne ou à un autre type de hash ?*


# 5.2.3. Synthèse
----------

### 1. Longueur contre complexité

Dans cette partie, vous allez comparer plusieurs mots de passe ayant des caractéristiques différentes.

Considérez par exemple :

```text
Password1
Password123
Cégep2026
azT7!pQ2
unephrasebeaucouppluslongue
```

Le but est de déterminer si la longueur seule ou la complexité apparente permet de prédire la difficulté d'une attaque.

Vous disposez d'un fichier :

```text
hashes_comparaison.txt
```

Effectuez les attaques nécessaires pour retrouver les mots de passe. Pensez à combiner les approches : une attaque par dictionnaire (au besoin avec `--rules`), puis une force brute ciblée pour les mots de passe plus courts.
Complétez :

| Mot de passe | Longueur | Type | Dictionnaire | Force brute | Difficulté observée |
|---|---:|---|---|---|---|
|1 | | | | | |
| 2| | | | | |
| 3| | | | | |
|4 | | | | | |
| 5| | | | | |

**Questions :**
1. *Quel mot de passe vous a semblé le plus facile à retrouver ? Pourquoi ?*
2. *Un mot de passe plus long est-il nécessairement plus difficile à retrouver ?*
3.  Comparez :
    ```text
    MotDePasse123456
    ```
    *et un mot de passe généré aléatoirement de longueur comparable.*
    
    *Lequel devrait généralement être le plus difficile à retrouver ? Pourquoi ?*

### 2. Dictionnaire ou force brute ?

Vous allez maintenant comparer directement les deux approches.

Pour chacun des mots de passe fournis, déterminez quelle stratégie semble la plus efficace :

| Mot de passe fictif | Dictionnaire | Force brute | Stratégie la plus efficace |
|---|---|---|---|
| `123456` | | | |
| `bonjour` | | | |
| `azerty123` | | | |
| `483729` | | | |
| mot de passe aléatoire | | | |

**Questions :**
1. *Pourquoi un attaquant ne commencerait-il pas nécessairement par une force brute complète ?*
2. *Dans quelles situations une attaque par dictionnaire est-elle particulièrement efficace ?*

### 3. Attaque hors ligne vs. attaque en ligne

Jusqu'à présent, vous avez travaillé directement avec des *hashes*.

Cela représente une situation où un attaquant a réussi à obtenir une copie des données contenant les mots de passe hachés.

Il peut alors effectuer ses tentatives **hors ligne**, sans avoir à communiquer avec le serveur à chaque tentative.

Dans le cas où l'attaque se fait en ligne, directement contre une page de connexion, le serveur peut mettre en place des mécanismes tels que :

- limitation du nombre de tentatives ;
- délai entre les tentatives ;
- verrouillage temporaire ;
- détection d'activités inhabituelles ;
- MFA.

**Question :**
1. *Pourquoi le fait de posséder une copie des hashes peut-il être particulièrement dangereux ?*
2. *Pourquoi une attaque de force brute contre un formulaire de connexion Web n'est-elle généralement pas équivalente à une attaque de force brute hors ligne ?*

### 4. Synthèse

À partir des expériences réalisées, complétez les phrases suivantes :

1. *Une attaque par dictionnaire consiste à  :* ______________________________________________
2. *Une attaque par force brute consiste à  :* ______________________________________________
3. *Plus un mot de passe est long, plus      :* ______________________________________________
4. *Un mot de passe prévisible peut être vulnérable même s'il :* ______________________________________________
5. *Une attaque hors ligne est particulièrement dangereuse parce que :* _________________________________________

### 5. Mise en situation

Un utilisateur choisit le mot de passe suivant :

```text
Montreal2026!
```

Il affirme :

> "Mon mot de passe contient une majuscule, des minuscules, des chiffres et un symbole. Il est donc très sécurisé."

Vous devez évaluer cette affirmation.

1. *Quels éléments rendent ce mot de passe prévisible ?*
2. *Quelle attaque pourriez-vous essayer en premier ?*
3. *Proposez une meilleure stratégie de création de mot de passe. Votre réponse doit expliquer **pourquoi** votre stratégie est plus sécuritaire et non simplement donner un exemple de mot de passe*.

<!-- 
# 10 — Conclusion

Dans ce laboratoire, vous avez expérimenté deux grandes stratégies de recherche de mots de passe :

- **l'attaque par dictionnaire**, qui exploite les mots de passe courants et les choix prévisibles ;
- **l'attaque par force brute**, qui explore systématiquement un espace de possibilités.

Vous avez également observé que le nombre de combinaisons augmente rapidement avec la longueur du mot de passe :

\[
N^L
\]

Cependant, le nombre théorique de combinaisons n'est pas le seul facteur important. La prévisibilité du mot de passe et la stratégie d'attaque jouent également un rôle important.

Enfin, vous avez constaté qu'une attaque contre des mots de passe hachés peut être très différente d'une attaque directement contre un service en ligne.

### À retenir

> **Un bon mot de passe n'est pas simplement un mot de passe « compliqué ». Il doit surtout être suffisamment long, difficile à deviner et unique.**

L'utilisation de mots de passe uniques et de l'authentification multifacteur permet également de réduire considérablement les conséquences d'une compromission. -->