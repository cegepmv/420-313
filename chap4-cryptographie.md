+++
pre = '<b>04. </b>'
title = 'Cryptographie'
weight = '400'
draft = false
+++

---------
La **cryptographie** est un élément fondamental de la sécurité informatique. Elle permet notamment de protéger la **confidentialité**, l'**intégrité** et l'**authenticité** des informations échangées.

Dans ce chapitre, nous découvrirons l'évolution de la cryptographie, des **chiffres classiques** aux mécanismes modernes comme le **chiffrement symétrique et asymétrique**, l'**échange de clés**, les **fonctions de hachage**, les **signatures numériques** et les **certificats**. Enfin, nous aborderons les enjeux liés à l'**informatique quantique* et à la **cryptographie post-quantique**.*

## Terminologie et histoire

Dans tout système cryptographique, on retrouve les éléments suivants :

- Le **message en clair** (*cleartext*) : ce qu'on veut chiffrer.
- Le **message chiffré** (*ciphertext*) : le résultat du chiffrement.
- Le **chiffre** (*cipher*) : la méthode utilisée pour chiffrer.
- La **clé** : l'information secrète utilisée pour chiffrer et déchiffrer les messages.

Le but d'un système cryptographique est de rendre très difficile (idéalement impossible) de dériver le message en clair à partir du message chiffré sans posséder la clé. Pour bien des systèmes, il est moins difficile de se procurer la clé que de déchiffrer directement le message — d'où l'importance centrale, à travers l'histoire, de la **confidentialité de la clé**.

### Les grandes ères de la cryptographie

![frise chronologique des grandes ères de la cryptographie (classique → moderne → âge d'or → post-quantique](/images/04-histoire-crypto.png)

- **Ère classique** : jusqu'à l'avènement des machines à chiffrer (chiffrement manuel, faible) — premiers exemples dans l'Égypte ancienne et l'Empire romain.
- **Ère moderne** : début du XXe siècle, avec l'avènement des machines électromécaniques (*Enigma* pendant la Seconde Guerre mondiale), puis électroniques (*DES*).
- **L'âge d'or** : 1976-1978, avec l'invention de la **cryptographie à clé publique** (Diffie-Hellman, puis RSA).
- **Ère post-quantique (émergente)** : recherche actuelle sur des algorithmes résistants aux ordinateurs quantiques.



+++
title = 'Cryptographie classique'
weight = '410'
draft = false
+++
-------------

## Algorithme de César

Utilisé par Jules César pour communiquer avec ses armées, ce chiffre consiste à **décaler** les lettres de l'alphabet d'une valeur fixe (traditionnellement 3) : A devient D, B devient E, etc. La **clé** est la valeur du décalage. Le chiffre de César n'a qu'une seule clé possible par décalage, ce qui le rend *trivial à briser* une fois la méthode connue.

![Algorithme de César](/images/04-algorithme-cesar.png)


## Algorithme de décalage et de substitution

- **Algorithme de décalage** : généralisation de César où la clé *k* peut prendre n'importe quelle valeur entre 1 et 26.
- **Algorithme de substitution** : chaque lettre du texte en clair est remplacée par une lettre différente selon une table de substitution complète, qui constitue la clé.

Exemple :
||
|-----------| -|- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |- |
|**Texte clair**| A| B| C| D| E| F| G| H| I| J| K| L| M| N| O| P| Q| R| S| T| U| V| W| X| Y| Z|
|**Texte codé** | W| X| E| H| Y| Z| T| K| C| P| J| I| U| A| D| G| L| Q| M| N| R| S| F| V| B| O|

Le texte que nous souhaitons crypter est le suivant : 

`UN PETIT ROSEAU M'A SUFFI POUR FAIRE FREMIR L'HERBE HAUTE ET TOUT LE PRE ET LES DOUX SAULES ET LE RUISSEAU QUI CHANTE AUSSI`.

Le texte codé est alors : 

`RA GYNCN QDMYWR U'W MRZZCN GDRQ ZWCQY ZQYUCQ I'KYQXY KWRNY YN NDRN IY GQY YN IYM HDRV MWRIYM LRC EKWANYAN WRMMC`.


{{%notice style="tip" title="Exercice"%}}
1. Chiffrez la phrase suivante en utilisant l’algorithme de décalage et la clé **-5**: 

“Le chat attrape la souris”.

2. Déchiffrez le texte suivant sachant que la clé vaut **13**: 

“Yr irag fbhssyr qbhprzrag pr fbve”.

3. Quelle est le texte en clair pour le message suivant chiffré avec l'algorithme de décalage  et sachant que la clef est **9**? “Dwn ounda nluxc nw brunwln”

4. Vous recevez le message suivant chiffré : 

`IY EKWN WNNQWGGY IW MDRQCM`. 

En utilisant la même table de substitution montrée ci-dessus, quel est le texte en clair?
{{%/notice%}}

{{% expand "Solution" %}}
1. Oh fkdw dwwudsh od vrxulv
2. Le vent souffle doucement ce soir
3. Une fleur éclot en silence
4. Le chat attrape la souris
{{% /expand %}}

## Algorithme de Vigenère

Amélioration du chiffre de décalage : plutôt qu'un décalage fixe, on utilise une **clé** (un mot) dont chaque lettre indique un décalage différent, répétée sur toute la longueur du message. Cela complique significativement la cryptanalyse par rapport à César, bien que le chiffre reste vulnérable à des techniques d'**analyse de fréquence** adaptées.

### Méthode
La clé est un mot dont la position de chacune des lettres est une valeur de décalage. Par exemple, si la clé est le mot `ABRI`, cela correspond aux valeurs `0-1-17-8`.

Pour chiffrer un message, par exemple “Le ciel est bleu”, on applique la clé sur ce message (en la répétant au besoin):

||
|-----------| -|- |- |- |- |- |- |- |- |- |- |- |- |
|**Original**|	L|	E|	C|	I|	E|	L|	E|	S|	T|	B|	L|	E|	U|
|**Clé**|	A|	B|	R|	I|	A|	B|	R|	I|	A|	B|	R|	I|	A|
|**Décalage**|	0|	1|	17|	8|	0|	1|	17|	8|	0|	1|	17|	8|	0|
|**Message chiffré**|	L|	F|	T|	Q|	E|	M|	V|	A|	T|	C|	C|	M|	U|

Pour déchiffrer le message, il suffit d’appliquer le clé sur le message chiffré et décaler les lettres dans le sens inverse. Une manière un peu plus rapide de chiffrer et de déchiffrer les messages ainsi chiffrés consiste à utiliser une table comme la suivante:

![table d'aide de déchiffrement de l'algorithme de Vigenère](/images/04-table-dechiffrer-vigenere.png)

{{% notice tip "Exercice" %}}
Vous recevez le message suivant chiffré par la méthode de Vigenère:
+ **Texte chiffré:** `yshjvykdqdyihfopexrabmrmgdmjjmozqnvrgzqdfypvqsuifcxfzp`.
+ **Clé:** `nombre`

Quel est le texte en clair?
{{% /notice %}}

{{% expand "Solution" %}}
“Le vieux pêcheur contemplait paisiblement le coucher de soleil”
{{% /expand %}}

+++
title = 'Cryptographie moderne'
weight = '420'
draft = false
+++
-------------

## Le masque jetable (*One-Time Pad*)

Inventé par le capitaine Vernam (US Army Signal Corps, 1919), utilisé entre autres pour le téléphone rouge Moscou-Washington durant la guerre froide.

**Conditions de sécurité parfaite (Shannon)** :
1. La clé doit être *aussi longue* que le message.
2. La clé doit être générée de manière *parfaitement aléatoire*.
3. La clé ne doit *jamais être réutilisée*.

Si ces conditions sont respectées, le masque jetable est le **seul système cryptographique dont la sécurité est mathématiquement parfaite** : sans la clé, tous les messages de la même longueur sont également probables, rendant la force brute inutile en pratique. En contrepartie, la distribution et la gestion d'une clé aussi longue que le message (et jamais réutilisée) sont extrêmement contraignantes, ce qui limite son usage à des contextes très spécifiques (diplomatie, renseignement).

### Fonctionnement général
Imaginons qu’on veut chiffrer le mot `ATTAQUE` avec une clé qui représente un décalage alphabétique, comme `12-2-4-5-1-20-1`:

||
|--------------------|-----|----|---|---|---|----|----|
|**Original**        |	A  |	T |	T |	A |	Q |	U  |	E |
|**Clé**             |	12 |	2 |	4 |	5 |	1 |	20 |	1 |
|**Message chiffré** |	M  |	V |	X |	F |	R |	O  |	F |

Si on veut déchiffrer `MVXFROF` sans connaître la clé, la seule manière d’y arriver est de tester toutes les clés possibles. Puisque chaque “lettre” de la clé a 26 valeurs possibles et que la taille de la clé est de 7 (la même que le mot), on a 26⁷ clés possibles à tester soit **803 181 176 possibilités**. Et surtout, si on arrive à les énumérer et les utiliser pour tenter de déchiffrer `MVXFROF`, on aura obtenu au passage tous les mots de 7 lettres possibles en français… Par exemple, le mot “Bananes”, si on le chiffre avec la clé 12-21-9-5-4-9-13 donne aussi MVXFROF. 

Comment alors savoir si le message original est “Attaque” ou “Bananes”, ou “Compter”, “Jardins”, etc? Il faut avoir la clé pour en être sûr.


### Utilisation concrète
Dans les systèmes cryptographiques modernes, on ne chiffre pas les messages en utilisant un décalage mais plutôt en utilisant l’**opération logique XOR** (“ou exclusif”) sur les bits du message en clair. La clé est une séquence de bits (0 et 1) aussi longue que le message. Le chiffrement s'effectue par un OU EXCLUSIF (**XOR**) bit-à-bit entre le message et la clé.

|x|	y|	x ⊕ y|
|-|-|-|---|
|0|	0|	0|
|0|	1|	1|
|1|	0|	1|
|1|	1|	0|

Le chiffrement du mot `POMME` consiste donc à se baser sur la représentation binaire du mot, par exemple l’encodage ASCII, à générer une clé binaire aléatoire de même taille et d’appliquer l’opération XOR:


||
|-|-|-|-|-|-|
|Message en clair|	P|	O|	M|	M|	E|
|**ASCII**|	`01010000`|	`01001111`|	`01001101`|	`01001101`|	`01000101`|
|**Clé**|	`10000001`|	`10101010`|	`00011000`|	`00000000`|	`11111111`|
|**Message chiffré**	|`11010001`	|`11100101`	|`01010101`	|`01001101`	|`10111010`|



{{% notice tip "Exercice" %}}
Les messages suivants sont chiffrés avec l’opération XOR et les [valeurs ASCII](http://sticksandstones.kstrom.com/appen.html) des caratcères. Si la clé n’a pas la même taille que le message d’origine, répétez-la comme pour le chiffre de Vigenère.

1. Si le message chiffré est :

`00001111 00010111 00000010 00000110 00000111` 

et que la clé est `abc`, quel est le message en clair?

2. Si le message en clair est `machine` et que le message chiffré est 

`00001111 00001000 00001110 00001010 00000000 00000011 00000111`

Quelle est la clé (en caractères)?
{{% /notice %}}

{{% expand "Réponse" %}}
1. nuage
2. bim
{{% /expand %}}



## La machine Enigma

![Machine Enigma](/images/04-enigma.png)

Utilisée par l'armée allemande durant la Seconde Guerre mondiale, **Enigma** chiffrait un message lettre par lettre à l'aide d'un système de *rotors électromécaniques*. Pour chiffrer, on entrait une clé (3 ou 4 caractères selon le modèle) définissant la position initiale des rotors, puis on tapait chaque lettre du message ; la lettre chiffrée correspondante s'allumait. La clé changeait *chaque jour*, rendant le déchiffrement difficile sans la connaître — jusqu'à ce que les Alliés parviennent à casser le système, un tournant majeur de la guerre. 

{{%notice style="info" title="Colossus"%}}
La machine servant à effectuer ce décryptage, *Colossus*, peut être considérée comme un des premiers ordinateurs.
{{%/notice%}}

+ **Vidéo :** [Fonctionnement de la machine Enigma](https://www.youtube.com/watch?v=ybkkiGtJmkM)

+++
title = 'Cryptographie à clé publique'
weight = '430'
draft = false
+++

-------------

## Symétrique vs. asymétrique

![Schéma comparatif symétrique (une clé partagée) vs asymétrique (paire de clés).](/images/04-symetrique-vs-asymetrique.png)

### Chiffrement symétrique

Les deux parties utilisent la **même clé** pour chiffrer et déchiffrer. Il faut donc que **tous les participants possèdent la clé**, et qu'elle ait été échangée de façon sécurisée au préalable.

{{%notice style="info" title="Algorithmes courants"%}}
**Principaux algorithmes modernes**
+ **AES (*Advanced Encryption Standard*) :** Le standard mondial actuel, qui traite les données par blocs de 128 bits avec des clés de 128, 192 ou 256 bits.
+ **ChaCha20 / XChaCha20 :** Un algorithme de type chiffrement par flot très rapide et sécurisé, souvent utilisé sur les appareils mobiles et le Web.
+ **Twofish et Serpent :** Des alternatives robustes qui étaient finalistes lors du concours pour devenir l'AES

**Anciens algorithmes obsolètes**
+ **DES (*Data Encryption Standard*) :** Ancien standard américain abandonné car sa clé de 56 bits est trop courte et vulnérable. 
+ **3DES (Triple DES) :** Une évolution de DES qui applique l'algorithme trois fois de suite, aujourd'hui déconseillé car trop lent et remplacé par l'AES.
+ **RC4 :** Un chiffrement par flot autrefois très populaire, mais abandonné en raison de failles de sécurité majeures.
{{%/notice%}}


### Chiffrement asymétrique

#### Échange de clés Diffie-Hellman

Jusqu'en 1976, le problème central de la cryptographie restait le même depuis l'Antiquité : comment s'assurer que l'échange initial d'une clé se fasse en toute confidentialité ? **Whitfield Diffie** et **Martin Hellman** ont décrit un mécanisme permettant à deux interlocuteurs d'obtenir la **même clé** sans jamais l'échanger directement.

**Principe simplifié** :
- A et B s'entendent sur une information commune publique, **x**.
- A choisit un secret **y**, combine **x** et **y**, et transmet le résultat (xy) à **B**.
- B choisit un secret **z**, combine **x** et **z**, et transmet le résultat (xz) à A.
- Chacun combine ce qu'il a reçu avec son propre secret pour obtenir la même clé finale, sans que **y** ni **z** n'aient jamais transité sur le réseau.

**Algorithme (version mathématique) :**
1. A et B échangent publiquement un nombre premier **P** et un générateur **G**.
2. Chacun choisit secrètement un nombre **X** (inférieur à P).
3. Chacun calcule `Y = G^X mod P` et transmet le résultat à l'autre.
4. Chacun calcule la clé `C = Y^X mod P` à partir du Y reçu et de son propre X.

La clé ainsi calculée, appelée **clé de session**, n'est utilisée que pour cette communication. Ce mécanisme est à la base de nombreux protocoles modernes, dont **HTTPS**.

**Exemple :**
![Exemple de l'algorithme Diffie-Hellman](/images/04-diffieh.png)

+ A et B s’entendent sur des valeurs pour **P** et **G** (respectivement `37` et `4`).
+ Pour la valeur de **X**, A choisit `7` et **B** choisit **12**. 
+ La clé calculée par A et B à partir de ces valeurs vaut 10.

{{% notice tip "Exercice" %}}
A et B s'entendent sur `P=57` et `G=14`. A choisit `X=5`, B choisit `X=16`. Quelle est la clé finale calculée par les deux parties ?

Vous pouvez utiliser la calculatrice sur [ce site](https://planetcalc.com/8326/).

+ Y = G^X % P
+ C = Y^X % P
{{% /notice %}}

## Clés publiques
Les travaux de Diffie et Hellman ont pavé la voie à une deuxième manière de régler le problème de l’échange de clé. **La cryptographie à clé publique** a été mise au point en 1978 par trois cryptologues: Ronald Rivest, Adi Shamir et Leonard Adleman. L’algorithme se nomme **RSA**, des noms de ses trois inventeurs.

Dans ce système les deux parties utilisent des **clés différentes** : une **clé publique**, qui sert à chiffrer, et une **clé privée**, que seul son détenteur possède et qui sert à déchiffrer. Ce qui est chiffré avec une clé ne peut être déchiffré qu'avec l'autre. Les deux clés fonctionnent exclusivement l’une avec l’autre, comme un cadenas et sa combinaison.

- La clé publique peut être distribuée sans limite.
- La clé privée doit rester *strictement confidentielle*.

{{%notice style="note" title="Problème 1"%}}
Si la clé publique n'est pas fournie de façon fiable, un attaquant pourrait substituer sa propre clé publique et mener une **attaque de l'homme du milieu** — d'où le rôle des **certificats** émis par une autorité de confiance (voir le chapitre sur les cer).
{{%/notice%}}

{{%notice style="note" title="Problème 2"%}}
Le chiffrement asymétrique est nettement plus lent (souvent ~1000 fois) que le chiffrement symétrique, ce qui explique pourquoi il est généralement utilisé **seulement pour échanger une clé symétrique de session**, plutôt que pour chiffrer l'ensemble d'une communication.
{{%/notice%}}

{{%notice style="info" title="Principaux algorithmes"%}}
+ **RSA :** Algorithme de cryptographie à clé publique reposant sur les propriétés mathématiques de la factorisation des grands nombres. Il peut notamment être utilisé pour certaines opérations de chiffrement et de signature.
+ **DSA (*Digital Signature Algorithm*):** Algorithme conçu pour les signatures numériques.
+ **ECC (Courbes Elliptiques) :** Famille de techniques cryptographiques utilisant les mathématiques des courbes elliptiques. Elles permettent d'obtenir une sécurité comparable à certains systèmes RSA avec des clés de taille plus réduite.
{{%/notice%}}

[Reel explicatif](https://www.youtube.com/shorts/4vUeGKPl3mU)

<!-- ### Chiffrer ou signer ?

- **Chiffrer avec la clé publique** : seule la personne possédant la clé privée correspondante pourra déchiffrer → garantit la **confidentialité**.
- **Chiffrer (signer) avec la clé privée** : n'importe qui possédant la clé publique peut déchiffrer, donc aucune confidentialité — mais le fait que le message se déchiffre correctement avec la clé publique prouve qu'il provient bien du détenteur de la clé privée → garantit l'**authenticité** (signature numérique). [Vidéo Explicative](https://www.youtube.com/watch?v=JR4_RBb8A9Q)
 -->



+++
title = 'Applications'
weight = '431'
draft = false
+++
-------------

La cryptographie à clé publique est très utile et possède de nombreuses applications : **échange de clés, signatures numériques, authentification, certificats**, etc.

Les mécanismes asymétriques sont généralement plus coûteux que les mécanismes symétriques. Les deux types de cryptographie sont donc utilisés de manière **complémentaire** : la cryptographie asymétrique permet notamment d'établir une relation de confiance, de réaliser certaines opérations d'authentification ou de participer à l'établissement d'une clé, tandis que la cryptographie symétrique est ensuite utilisée pour chiffrer efficacement les données.

## Échange de clés

L’une des applications de la cryptographie à clé publique est de permettre l’**échange sécurisé d’une clé symétrique** entre deux interlocuteurs.

Par exemple, RSA peut être utilisé pour chiffrer une clé symétrique et la transmettre de manière confidentielle entre deux participants. Dans ce contexte, RSA constitue une autre approche que l’échange de clés Diffie-Hellman présenté précédemment.

Le principe est le suivant :

![Échange d'une clé de session avec RSA](/images/04-rsa-echange-cle.png)

A et B s’échangent d’abord leurs clés publiques. Cette étape peut être réalisée une seule fois.

Ensuite, chaque fois que A et B souhaitent communiquer, l’un des deux participants (ici, A) génère une **clé de session** aléatoire pour cette communication :

1. A génère une clé de session.
2. A chiffre cette clé avec la clé publique de B.
3. B reçoit la clé chiffrée et utilise sa clé privée pour la déchiffrer.
4. A et B possèdent maintenant la même clé de session.
5. Ils peuvent utiliser cette clé avec un algorithme symétrique comme **AES** ou **ChaCha20** pour chiffrer leurs communications.

La cryptographie asymétrique n’est donc pas nécessairement utilisée pour chiffrer l’ensemble de la communication. Elle sert plutôt à **établir ou transmettre de manière sécurisée les éléments nécessaires à une communication symétrique**.

<!-- {{%notice style="info" title="Dans les systèmes modernes"%}}

Les protocoles modernes, notamment TLS, utilisent généralement des mécanismes d'**échange de clés éphémères** tels que Diffie-Hellman sur courbes elliptiques (ECDHE), plutôt que de transmettre directement une clé de session chiffrée avec RSA.

{{%/notice%}} -->

{{%notice style="tip" title="À retenir"%}}
La cryptographie asymétrique permet notamment de résoudre le problème de l'établissement initial d'une communication sécurisée, tandis que la cryptographie symétrique permet de chiffrer efficacement les données.
{{%/notice%}}

## Authentification

La cryptographie à clé publique peut également être utilisée à des fins d’**authentification**.

Sur de nombreux sites, l’authentification repose sur un mot de passe. Celui-ci constitue une preuve d’identité parce qu’il est supposé être **secret et connu uniquement de son propriétaire**. 

Cependant, les mots de passe peuvent être faibles, réutilisés ou compromis et sont donc vulnérables à différentes attaques.

Une autre approche consiste à utiliser une **paire de clés publique et privée**.

Comme un mot de passe, la clé privée est un secret que son propriétaire doit conserver et ne jamais divulguer. Elle possède toutefois des caractéristiques qui la rendent particulièrement difficile à deviner :

+ Elle est **générée aléatoirement**.
+ Elle possède généralement une **taille importante**.
+ Elle n’a pas besoin d’être mémorisée par l’utilisateur.

La clé privée peut ainsi servir à **prouver la possession d’une identité cryptographique** sans avoir à transmettre le secret lui-même.

Le principe repose sur la **signature numérique** :

![Authentification avec une paire de clés](/images/04-authentification_A_B.png)

Supposons que A souhaite s’authentifier auprès de B et que B possède déjà la **clé publique de A**.

1. B génère un **défi aléatoire**.
2. B transmet ce défi à A.
3. A signe le défi avec sa **clé privée**.
4. A transmet la signature à B.
5. B vérifie la signature à l’aide de la **clé publique de A**.
6. Si la signature est valide, B peut vérifier que le message a bien été signé avec la clé privée correspondante.

A n’a donc jamais besoin de transmettre sa clé privée à B.

{{%notice style="tip" title="Pourquoi utiliser un défi aléatoire ?"%}}
Le message utilisé pour l’authentification doit être différent à chaque tentative. On utilise généralement un **nombre aléatoire appelé nonce** afin d’éviter qu’un attaquant puisse simplement enregistrer une ancienne réponse valide et la réutiliser plus tard.
{{%/notice%}}

Dans cet exemple, A signe un défi relativement court. Pour signer des données plus volumineuses, on utilise généralement une **fonction de hachage** afin de produire une empreinte du message. Cette empreinte est ensuite utilisée dans le processus de signature.




<!-- 

## Signature numérique

L’authentification n’est qu’une des applications des **signatures numériques**.

Une signature numérique permet notamment de vérifier :

+ **l’authenticité :** le message a été signé par le détenteur de la clé privée ;
+ **l’intégrité :** le message n’a pas été modifié depuis sa signature ;
+ **la non-répudiation**, dans certains contextes : le signataire ne peut pas facilement nier avoir produit la signature, sous réserve des conditions juridiques et techniques applicables.

Le fonctionnement est différent du chiffrement utilisé pour assurer la confidentialité :

+ Pour assurer la **confidentialité**, on chiffre avec la **clé publique du destinataire** et seul le détenteur de la clé privée correspondante peut déchiffrer.
+ Pour produire une **signature**, le détenteur utilise sa **clé privée** et les autres participants utilisent sa **clé publique** pour vérifier la signature.

Dans la pratique, on ne signe généralement pas directement l’ensemble du message avec l’algorithme asymétrique. On calcule plutôt une **empreinte cryptographique** du message, puis cette empreinte est signée avec la clé privée.

![Principe général d'une signature numérique](/images/04-signature-numerique.png)

Ainsi, si le message est modifié après sa signature, son empreinte ne correspondra plus à celle utilisée lors de la signature et la vérification échouera.

{{%notice style="tip" title="À retenir"%}}
+ **Clé publique du destinataire → chiffrement → confidentialité**

+ **Clé privée du signataire → signature → authenticité et intégrité**
{{%/notice%}}


## Certificats et infrastructure à clé publique (PKI)

Un dernier problème demeure : **comment savoir à qui appartient réellement une clé publique ?**

Une clé publique peut être distribuée librement, mais un attaquant pourrait tenter de remplacer la clé publique d’un utilisateur par la sienne afin de se faire passer pour lui.

C’est précisément le problème auquel répondent les **certificats numériques**.

Un certificat permet notamment d’associer une **identité** à une **clé publique**. Cette association est validée par une **autorité de certification (CA)**, qui signe elle-même le certificat.

L’ensemble des mécanismes, technologies et organisations permettant de gérer ces clés et certificats constitue une **infrastructure à clé publique (PKI — Public Key Infrastructure)**.

Les certificats et la PKI jouent notamment un rôle essentiel dans **HTTPS**, où ils permettent au navigateur de vérifier l’identité du serveur avant d’établir une communication sécurisée.

{{%notice style="info" title="Le rôle de la PKI"%}}
La cryptographie à clé publique permet de résoudre plusieurs problèmes, mais elle ne permet pas à elle seule de savoir **à qui appartient une clé publique**.

La **PKI** ajoute une infrastructure de confiance permettant d’associer une clé publique à une identité grâce aux **certificats numériques** et **aux autorités de certification**.
{{%/notice%}} -->

+++
title = 'Fonctions de hachage'
weight = '432'
draft = false
+++
-------------

## Définition
Une **fonction de hachage cryptographique** transforme une donnée de taille quelconque en une valeur de taille fixe appelée **empreinte**, **condensat** ou *hash*.

Par exemple, une même fonction peut produire une empreinte de 256 bits pour un message de quelques caractères comme pour un fichier de plusieurs gigaoctets.

![Hachache : Principe général](/images/04-hachage-principe-general.drawio.png)

Une fonction de hachage cryptographique possède notamment les propriétés suivantes :

- **Taille fixe :** la sortie possède toujours la même taille pour une fonction donnée.
- **Déterminisme :** une même entrée produit toujours la même empreinte.
- **Effet avalanche :** une petite modification de l'entrée entraîne généralement une modification importante de l'empreinte.
- **Résistance à la préimage :** à partir d'une empreinte, il doit être extrêmement difficile de retrouver une entrée produisant cette empreinte.
- **Résistance aux collisions :** il doit être extrêmement difficile de trouver deux entrées différentes produisant la même empreinte.

{{%notice style="warning" title="Une empreinte n'est pas unique"%}}

Il est important de ne pas dire qu'une fonction de hachage produit une valeur "unique".

Une fonction de hachage possède un nombre fini de sorties, alors que le nombre de messages possibles est pratiquement illimité. **Les collisions sont donc mathématiquement inévitables**.

Une bonne fonction de hachage cryptographique doit plutôt rendre la recherche volontaire d'une collision extrêmement difficile.

{{%/notice%}}

## Hachage ≠ chiffrement

Une fonction de hachage ne sert pas à chiffrer un message.

+ Le **chiffrement** permet de transformer des données afin qu'elles puissent être récupérées à l'aide d'une clé.
+ Le **hachage** produit une empreinte qui sert notamment à vérifier ou représenter une donnée.

![Hachage vs. Chiffrement](/images/04-hachage-vs-chiffrement.png)

Une fonction de hachage cryptographique ne nécessite donc pas de clé.

## Algorithmes de hachage courants

Plusieurs algorithmes de hachage ont été utilisés au fil du temps.

|Algorithme|	Taille de l'empreinte|	Situation|
|-------|----------------------|----------|
|MD5|	128 bits|	Obsolète|
|SHA-1|	160 bits|	Obsolète|
|SHA-256	|256 bits|	Couramment utilisé|
|SHA-384	|384 bits|	Couramment utilisé|
|SHA-512	|512 bits|	Couramment utilisé|

**MD5** et **SHA-1** ne doivent plus être utilisés lorsqu'une résistance aux collisions est nécessaire.

La famille **SHA-2**, notamment **SHA-256** et **SHA-512**, est encore largement utilisée.

Sous Linux, il est possible de calculer différentes empreintes directement depuis le terminal :
```bash
echo "allo" | md5sum
echo "allo" | sha1sum
echo "allo" | sha256sum
```

On peut également calculer l'empreinte d'un fichier :

```bash
sha256sum fichier.iso
```

## Applications

Les fonctions de hachage sont utilisées dans de nombreux contextes.

### Vérification de l'intégrité

Une empreinte peut être utilisée pour vérifier qu'un fichier n'a pas été modifié.

![Intégrité du fichier original](/images/04-integrite-fichier-original.png)

Après téléchargement :

![Intégrité du fichier original](/images/04-integrite-fichier-telecharge.png)

Si les empreintes sont différentes, le fichier téléchargé n'est pas identique au fichier original.

{{%notice style="info" title="Attention"%}}

Une simple empreinte publiée sur le même canal que le fichier ne permet pas nécessairement de détecter une attaque.

Si un attaquant peut modifier le fichier, il pourrait également modifier l'empreinte publiée.

Pour obtenir une véritable garantie d'authenticité, on utilise notamment une **signature numérique**.

{{%/notice%}}

<!-- ### Structures de données

Les fonctions de hachage sont également utilisées en programmation pour construire des structures de données comme les **tables de hachage** (*hash tables*).

Elles permettent notamment de retrouver efficacement une valeur à partir d'une clé. -->

### Mots de passe

Les fonctions de hachage sont également utilisées dans les systèmes d'authentification.

Cependant, un mot de passe ne devrait pas être stocké simplement avec SHA-256 :
```text
Mot de passe ──► SHA-256 ──► Empreinte
```
Les attaquants peuvent tester très rapidement un grand nombre de mots de passe contre une telle empreinte.

Les systèmes modernes utilisent plutôt des fonctions spécialement conçues pour le stockage des mots de passe, par exemple :

- **Argon2id**
- **scrypt**
- **bcrypt**
- **PBKDF2**

Ces mécanismes utilisent notamment un **sel (*salt*)** et sont volontairement plus coûteux à calculer qu'une fonction de hachage classique.

{{%notice style="tip" title="À retenir"%}}

Une fonction de hachage permet notamment de produire une empreinte d'une donnée.

**Hachage ≠ chiffrement**

Le hachage est particulièrement utile pour l'intégrité, les structures de données et le stockage sécurisé des mots de passe.

{{%/notice%}}


+++
title = 'Signature numérique'
weight = '433'
draft = false
+++
-------------


L’authentification n’est qu’une des applications des **signatures numériques**.

Une signature numérique permet notamment de vérifier :

+ **l'authenticité :** la signature a été produite à l'aide de la clé privée associée au signataire ;

+ **l'intégrité :** les données n'ont pas été modifiées depuis leur signature ;

+ **la non-répudiation**, dans certains contextes, lorsque les conditions techniques et juridiques appropriées sont réunies.

Une signature numérique utilise une paire de clés :
```text
              Clé privée
                  │
                  ▼
Message ──► Hachage ──► Signature
                         │
                         ▼
              Message + signature
```
Le destinataire utilise ensuite la clé publique correspondante pour vérifier la signature :
```text

Message ──► Hachage ───────────┐
                              │
Signature ──► Vérification ◄──┘
                              │
                         Valide / invalide
```

Le principe général est donc :

1. Le signataire calcule une empreinte du message.
2. Il utilise sa clé privée dans l'algorithme de signature pour produire une signature.
3. Il transmet le message accompagné de la signature.
4. Le destinataire utilise la clé publique du signataire pour vérifier la signature.
5. Le destinataire recalcule l'empreinte du message et vérifie que la signature correspond.

Si le message est modifié après sa signature, la vérification échouera.


{{%notice style="warning" title="Signature ≠ chiffrement"%}}

Une signature numérique ne sert pas à cacher le contenu du message.

Le message peut rester parfaitement lisible :
```text
Message en clair + signature
```
La signature permet plutôt de vérifier **qui a signé le message** et **si celui-ci a été modifié**.

Pour assurer la confidentialité, on utilise un mécanisme de chiffrement.

{{%/notice%}}

## Exemples d'algorithmes de signature

Parmi les algorithmes utilisés pour les signatures numériques, on retrouve notamment :

- **RSA-PSS**
- **ECDSA**
- **Ed25519**

Le fonctionnement précis varie selon l'algorithme, mais le principe général reste le même : une signature est produite avec la clé privée et vérifiée avec la clé publique correspondante.

{{%notice style="tip" title="À retenir"%}}

+ **Clé publique du destinataire → chiffrement → confidentialité**

+ **Clé privée du signataire → signature → authenticité et intégrité**
{{%/notice%}}

+++
title = 'Certificats numériques'
weight = '434'
draft = false
+++
-------------


Un problème demeure : **comment savoir à qui appartient réellement une clé publique ?**

Une clé publique peut être distribuée librement, mais un attaquant pourrait tenter de remplacer la clé publique d'un utilisateur par la sienne afin de se faire passer pour lui.

Un certificat numérique permet notamment de créer une association vérifiable entre une **identité** et une **clé publique**.

Un certificat contient notamment des informations comme :

- le nom du domaine ou l'identité concernée ;
- la clé publique ;
- la période de validité ;
- l'identité de l'autorité ayant émis le certificat ;
- la signature de l'autorité de certification.

```text

Identité
   │
   ├── Nom / domaine
   ├── Période de validité
   └── Clé publique

```

Le certificat est signé par une **autorité de certification (CA — Certificate Authority)**.

La signature de la CA permet au client de vérifier que le certificat n'a pas été modifié et qu'il a été émis par une autorité reconnue.

## Exemple simplifié

Une autorité de certification possède elle-même une paire de clés :

```text

CA
│
├── Clé privée
└── Clé publique

```

Lorsqu'un serveur obtient un certificat, celui-ci contient notamment :
```text


        ┌────────────────────────────────┐
        │       Certificat du serveur    │
        │                                │
        │ Domaine                        │
        │ Clé publique                   │
        │ Validité                       │
        │ Émetteur                       │
        │ Signature de la CA             │
        └────────────────────────────────┘
```

La CA utilise sa clé privée pour signer le certificat.

Le navigateur utilise ensuite la clé publique de la CA pour vérifier cette signature.

```text

                 Autorité de certification
                          │
                    clé privée
                          │
                          ▼
                  Signature du certificat
                          │
                          ▼
                Certificat du serveur
                          │
                          ▼
                      Navigateur
                          │
                  vérifie la signature
                          │
                          ▼
                    Certificat valide
                    
```

{{%notice style="info" title="Le rôle d'un certificat"%}}

Un certificat ne signifie pas simplement « ce site est fiable ».

Il permet notamment d'établir une **association vérifiable entre une identité et une clé publique**, selon les règles de validation de l'autorité de certification.

{{%/notice%}}

+++
title = 'HTTPS et TLS'
weight = '436'
draft = false
+++
-------------
## HTTPS et TLS

Les connexions **HTTPS** utilisent le protocole **TLS (Transport Layer Security)**.

TLS combine plusieurs mécanismes cryptographiques :

- la cryptographie à clé publique ;
- les certificats numériques ;
- l'échange de clés ;
- la cryptographie symétrique ;
- les fonctions de hachage et les signatures.

Lorsqu'un navigateur se connecte à un serveur HTTPS, il doit notamment :

1. obtenir le certificat du serveur ;
2. vérifier le certificat et sa chaîne de confiance ;
3. vérifier que le certificat correspond au domaine demandé ;
4. établir les paramètres cryptographiques de la connexion ;
5. établir une clé ou des clés de session ;
6. utiliser ensuite une cryptographie symétrique efficace pour protéger les données échangées.

On retrouve donc les différentes notions étudiées dans ce chapitre :
```text

                  HTTPS / TLS
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   Certificat     Échange de clés   Signature
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                Clé de session
                       │
                       ▼
              Chiffrement symétrique
                       │
                       ▼
                 Données HTTPS
```

{{%notice style="tip" title="À retenir"%}}

Dans une connexion HTTPS moderne, la cryptographie asymétrique et les certificats servent principalement à **établir la confiance et les paramètres de la connexion**, tandis que la cryptographie symétrique est utilisée pour protéger efficacement les données échangées.

{{%/notice%}}

+++
title = 'PKI'
weight = '435'
draft = false
+++
-------------

<!-- ## La chaîne de confiance

Dans la pratique, un navigateur ne possède pas nécessairement directement la clé publique de chaque autorité qui signe les certificats des serveurs.

Les certificats peuvent former une **chaîne de confiance**.
```text

Autorité racine (Root CA)
          │
          ▼
Autorité intermédiaire
          │
          ▼
Certificat du serveur
          │
          ▼
        HTTPS
```

Les systèmes d'exploitation et les navigateurs disposent d'un ensemble d'**autorités de certification racines de confiance**.

Lorsqu'un navigateur reçoit un certificat, il peut vérifier progressivement la chaîne jusqu'à une autorité racine qu'il reconnaît.

{{%notice style="tip" title="À retenir"%}}

La cryptographie à clé publique permet de vérifier une signature ou d'utiliser une clé publique, mais elle ne permet pas à elle seule de savoir **à qui appartient cette clé**.

Les certificats et la PKI ajoutent une infrastructure permettant d'établir cette relation de confiance.

{{%/notice%}} -->

## Infrastructure à clé publique (PKI)

L’ensemble des mécanismes, technologies et organisations permettant de gérer les clés, les certificats et les relations de confiance constitue une **infrastructure à clé publique (PKI — Public Key Infrastructure)**.

Une PKI peut notamment comprendre :

- des **autorités de certification (CA)** ;
- des certificats numériques ;
- des mécanismes de gestion et de validation des certificats ;

- des listes ou mécanismes permettant de déterminer si un certificat est encore valide ou doit être révoqué.

La PKI permet ainsi de construire une **chaîne de confiance** entre une clé publique et une identité.
```text

Autorité racine
       │
       ▼
Certificat
       │
       ▼
Clé publique du serveur
       │
       ▼
Identité / domaine
```
+++
title = 'HTTPS et TLS'
weight = '436'
draft = false
+++
-------------
## HTTPS et TLS

Les connexions **HTTPS** utilisent le protocole **TLS (Transport Layer Security)**.

TLS combine plusieurs mécanismes cryptographiques :

- la cryptographie à clé publique ;
- les certificats numériques ;
- l'échange de clés ;
- la cryptographie symétrique ;
- les fonctions de hachage et les signatures.

Lorsqu'un navigateur se connecte à un serveur HTTPS, il doit notamment :

1. obtenir le certificat du serveur ;
2. vérifier le certificat et sa chaîne de confiance ;
3. vérifier que le certificat correspond au domaine demandé ;
4. établir les paramètres cryptographiques de la connexion ;
5. établir une clé ou des clés de session ;
6. utiliser ensuite une cryptographie symétrique efficace pour protéger les données échangées.

On retrouve donc les différentes notions étudiées dans ce chapitre :
```text

                  HTTPS / TLS
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   Certificat     Échange de clés   Signature
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                Clé de session
                       │
                       ▼
              Chiffrement symétrique
                       │
                       ▼
                 Données HTTPS
```

{{%notice style="tip" title="À retenir"%}}

Dans une connexion HTTPS moderne, la cryptographie asymétrique et les certificats servent principalement à **établir la confiance et les paramètres de la connexion**, tandis que la cryptographie symétrique est utilisée pour protéger efficacement les données échangées.

{{%/notice%}}
