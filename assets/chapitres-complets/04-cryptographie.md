# Chapitre 4 - Cryptographie

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


# 4.1 - Cryptographie classique

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

# 4.2 - Cryptographie moderne

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



# 4.3 - Cryptographie à clé publique

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

# 4.3.1 - Applications

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

# 4.3.2 - Fonctions de hachage

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

# 4.3.3 - Signature numérique


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
Signature ──► Vérification  ◄──┘
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



# 4.3.4 - Certificats numériques


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

{{%notice style="info" title="Le rôle d'un certificat"%}}

Un certificat ne signifie pas simplement « ce site est fiable ».

Il permet notamment d'établir une **association vérifiable entre une identité et une clé publique**, selon les règles de validation de l'autorité de certification.

{{%/notice%}}

# 4.3.5 - Infrastructure à clé publique (PKI)

L’ensemble des mécanismes, technologies et organisations permettant de gérer les clés, les certificats et les relations de confiance constitue une **infrastructure à clé publique (PKI — Public Key Infrastructure)**.

Une PKI peut notamment comprendre :

- des **autorités de certification (CA)** ;
- des certificats numériques ;
- des mécanismes de gestion et de validation des certificats ;
- des listes ou mécanismes permettant de déterminer si un certificat est encore valide ou doit être révoqué.

La PKI permet ainsi de construire une **chaîne de confiance** entre une clé publique et une identité.


# 4.3.6 - HTTPS et TLS

Les connexions **HTTPS** utilisent le protocole **TLS (Transport Layer Security)**.

TLS combine plusieurs mécanismes cryptographiques :

- la cryptographie à clé publique ;
- les certificats numériques ;
- l'échange de clés ;
- la cryptographie symétrique ;
- les fonctions de hachage et les signatures.

![Étapes de connexion HTTPS/TLS](/images/04-https-tls.png)

Lorsqu'un navigateur se connecte à un serveur HTTPS, il doit notamment :

1. obtenir le certificat du serveur ;
2. vérifier le certificat et sa chaîne de confiance ;
3. vérifier que le certificat correspond au domaine demandé ;
4. établir les paramètres cryptographiques de la connexion ;
5. établir une clé ou des clés de session ;
6. utiliser ensuite une cryptographie symétrique efficace pour protéger les données échangées.

On retrouve donc les différentes notions étudiées dans ce chapitre :

![Résumé de toutes les technologies et notions utilisées par HTTPS/TLS](/images/04-https-tls-resume.png)

{{%notice style="tip" title="À retenir"%}}
Dans une connexion HTTPS moderne, la cryptographie asymétrique et les certificats servent principalement à **établir la confiance et les paramètres de la connexion**, tandis que la cryptographie symétrique est utilisée pour protéger efficacement les données échangées.
{{%/notice%}}


# 4.4 - Cryptographie post-quantique

## Regard vers l'avenir : l'informatique quantique

Toute la cryptographie à clé publique vue dans ce chapitre (*RSA*, *Diffie-Hellman*) repose sur des problèmes mathématiques réputés difficiles à résoudre pour un ordinateur classique, principalement la *factorisation de grands nombres* et le *calcul de logarithme discret*.

## Algorithme de Shor

En 1994, le mathématicien **Peter Shor** a démontré qu'un **ordinateur quantique** suffisamment puissant pourrait résoudre ces deux problèmes de façon efficace, rendant potentiellement obsolètes *RSA* et *Diffie-Hellman* tels qu'utilisés aujourd'hui.

{{%notice style="info" title="Nuance importante"%}}
Cette menace concerne spécifiquement le chiffrement **asymétrique**. Le chiffrement **symétrique** (AES) et les fonctions de **hachage** demeurent globalement robustes face aux ordinateurs quantiques.
{{%/notice%}}

## Où en est-on réellement ?

Le développement d'un ordinateur quantique capable de casser *RSA* en pratique nécessite un nombre de **qubits logiques** (après correction d'erreurs) largement supérieur à ce qui est démontré publiquement à ce jour. 

{{%notice style="note" title="Distinction importante"%}}
Il est important de distinguer la **recherche théorique**, bien établie, de la **menace opérationnelle** à court terme, qui reste incertaine et dépend de percées technologiques encore à venir.
{{%/notice%}}

## « *Harvest now, decrypt later* »

Une des raisons pour lesquelles il faut commencer à se préparer dès maintenant est une stratégie connue sous le nom de :

> **Harvest now, decrypt later**
>
> *Collecter maintenant, déchiffrer plus tard*.

Imaginons qu'un attaquant intercepte aujourd'hui une communication chiffrée avec une technologie à clé publique vulnérable à un futur ordinateur quantique. L'attaquant ne peut peut-être pas la déchiffrer aujourd'hui mais il peut cependant :

1. intercepter la communication;
2. conserver les données chiffrées;
3. attendre qu'une technologie suffisamment puissante soit disponible;
4. tenter de déchiffrer les données plusieurs années plus tard.


Cette menace est particulièrement importante pour les informations qui doivent rester confidentielles pendant de nombreuses années.

Par exemple :
+ des dossiers médicaux;
+ des secrets industriels;
+ des données de recherche;
+ des informations gouvernementales;
+ des données personnelles sensibles;
+ des communications diplomatiques ou stratégiques.

{{%notice style="info" title="Recommendations du NIST"%}}
**NIST** identifie explicitement le scénario *harvest now, decrypt later* comme une raison de commencer la transition vers la cryptographie post-quantique avant l'apparition d'un ordinateur quantique capable de casser les systèmes actuels.
{{%/notice%}}

## La cryptographie post-quantique

La **cryptographie post-quantique**, ou **PQC** (*Post-Quantum Cryptography*), désigne des algorithmes cryptographiques conçus pour résister aux attaques provenant aussi bien des ordinateurs classiques que des ordinateurs quantiques.

L'objectif n'est pas de créer une cryptographie qui fonctionne sur un ordinateur quantique.

Au contraire, les algorithmes post-quantiques sont généralement destinés à fonctionner sur les infrastructures informatiques classiques que nous utilisons aujourd'hui.

L'idée est plutôt de **remplacer progressivement** les mécanismes de cryptographie à clé publique vulnérables à l'algorithme de Shor.

### Les algorithmes « *quantum-resistant* »

Les algorithmes post-quantiques s'appuient sur d'autres problèmes mathématiques réputés difficiles même pour un ordinateur quantique, notamment :

- les **réseaux euclidiens** (*lattices*) ;
- les **codes correcteurs d'erreurs** ;
- certaines constructions basées sur le **hachage**.

{{%notice style="note" title="Nuance importante"%}}
Un algorithme est considéré comme **post-quantique** parce que sa sécurité est conçue pour résister aux attaques connues provenant d'ordinateurs quantiques. Cela ne signifie pas qu'il est démontré mathématiquement impossible à casser.
{{%/notice%}}

### Les premiers standards post-quantiques du NIST

Le **NIST** (*National Institute of Standards and Technology*) a organisé pendant plusieurs années un processus international d'évaluation et de sélection d'algorithmes post-quantiques.

En août 2024, trois premiers standards ont été finalisés :

|Standard|	Algorithme|	Fonction principale|	Famille|
|-------|--------|--------|-----|
|**FIPS 203**|	**ML-KEM**|	Établissement de clé	|Réseaux euclidiens|
|**FIPS 204**|	**ML-DSA**|	Signature numérique	|Réseaux euclidiens|
|**FIPS 205**|	**SLH-DSA**|	Signature numérique|	Hachage|

## Approches de transition

Dans l'attente d'une adoption complète des algorithmes post-quantiques, plusieurs organisations déploient déjà des solutions **hybrides**, combinant un algorithme classique (*RSA* ou *ECC*) et un algorithme post-quantique pour un même échange de clé, afin de rester protégées même si l'un des deux algorithmes venait à être cassé.

# 5. Laboratoires

# 5.1 Analyser un certificat

## Objectifs

- identifier les principales informations contenues dans un certificat numérique;
- identifier l'autorité de certification (CA) qui a signé un certificat;
- déterminer la période de validité d'un certificat;
- identifier le nom de domaine associé à un certificat;
- identifier les algorithmes utilisés;
- comprendre le rôle d'un certificat dans une connexion HTTPS.

##### 1. Choisir un site HTTPS

Choisissez un site Web utilisant **HTTPS**.

Par exemple :

```text
https://www.wikipedia.org
```

##### 2. Examiner le certificat avec le navigateur

Ouvrez le site dans votre navigateur, cliquez sur l'icône indiquant que la connexion est sécurisée, généralement représentée par un **cadenas** à gauche de l'adresse du site.

Accédez ensuite aux informations détaillées du certificat.

##### 3. Relever les informations du certificat

Complétez le tableau suivant à partir du certificat du site choisi.

| Information | Réponse |
|---|---|
| Nom du site | |
| Nom du certificat / sujet (Subject) | |
| Autorité ayant émis le certificat (Issuer) | |
| Date de début de validité | |
| Date d'expiration | |
| Algorithme de signature | |
| Algorithme de clé publique | |
| Taille de la clé publique | |
| Nom(s) de domaine dans le certificat (SAN) | |
| Le certificat est-il autosigné ? | |
| Le certificat est-il actuellement valide ? | |


###### **Questions**
1. Quelle organisation a émis le certificat du serveur ?
2. Pourquoi le navigateur fait-il confiance à cette autorité ?
3. Quelle est la différence entre l'**émetteur (Issuer)** et le **sujet (Subject)** d'un certificat ?
4. À quoi sert la date d'expiration d'un certificat ?
5. Pourquoi un certificat doit-il contenir le nom du domaine auquel il s'applique ?
6. Que se passerait-il si vous consultiez un site dont le certificat était expiré ?
7. Quelle différence y a-t-il entre un certificat signé par une CA reconnue et un certificat autosigné ?

##### 4. Observer le certificat avec OpenSSL

Il est également possible d'obtenir des informations sur un certificat directement à partir d'un terminal.

La commande suivante permet d'établir une connexion TLS avec un serveur :

```bash
openssl s_client -connect www.wikipedia.org:443 -servername www.wikipedia.org
```

La sortie contient beaucoup d'informations.

Pour obtenir uniquement les informations principales du certificat, utilisez :

```bash
openssl s_client -connect www.wikipedia.org:443 -servername www.wikipedia.org </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

Vous devriez obtenir des informations similaires à :

```text
subject=...
issuer=...
notBefore=...
notAfter=...
```

###### **Questions**

8. Quelle information correspond au sujet du certificat ?
9. Quelle information correspond à l'autorité de certification ?
10. Que représentent `notBefore` et `notAfter` ?

# 5.2. Laboratoire OpenSSL

-------------
## Certificat autosigné avec OpenSSL

En règle générale, lorsqu’un serveur doit obtenir un certificat reconnu publiquement, celui-ci est émis par une **autorité de certification**.

Dans certains contextes, notamment un laboratoire, un environnement de développement ou un serveur interne, il peut être utile de générer soi-même un certificat.

Un **certificat autosigné** est un certificat signé par sa propre clé privée plutôt que par une autorité de certification.

OpenSSL fournit les outils nécessaires pour générer et examiner des certificats numériques.

{{%notice style="warning" title="Certificat autosigné"%}}

Un certificat autosigné permet de comprendre le fonctionnement d'un certificat, mais il ne bénéficie pas automatiquement de la confiance des navigateurs et des systèmes.

Il est donc particulièrement adapté aux **laboratoires, tests et environnements internes**.

{{%/notice%}}

## Générer une clé privée

La première étape consiste à générer la clé privée du serveur.
```bash
openssl genrsa -out certif.key 2048
```
La clé privée doit être conservée secrète et protégée contre tout accès non autorisé.

## Générer une demande de certificat

Ensuite, nous générons une demande de certificat :
```bash
openssl req -new -key certif.key -out certif.csr
```
Cette commande crée un fichier contenant notamment les informations nécessaires à la création du certificat ainsi que la clé publique correspondant à la clé privée.

## Vérifier la demande

Pour afficher le contenu de la demande :
```bash
openssl req -text -noout -verify -in certif.csr
```
On peut notamment observer les informations du sujet et la clé publique.

## Générer le certificat autosigné

Enfin, nous générons le certificat :
```bash
openssl x509 -req -days 365 \
    -in certif.csr \
    -signkey certif.key \
    -out certif.crt
```
La clé privée du serveur est utilisée ici pour signer le certificat.

Le fichier `certif.crt` contient notamment :

- les informations du certificat ;
- la clé publique ;
- la période de validité ;
- la signature ;
- les informations relatives à l'émetteur.

## Afficher le certificat

Pour afficher son contenu :
```bash
openssl x509 -in certif.crt -text -noout
```

Vous devriez notamment être capables d'identifier :

- le **sujet** ;
- l'**émetteur** ;
- la **période de validité** ;
- la **clé publique** ;
- l'**algorithme de signature** ;
- la **signature du certificat**.

{{%notice style="tip" title="Lien avec la PKI"%}}

Dans ce laboratoire, le certificat est signé par le serveur lui-même.

Dans une PKI réelle, le certificat d'un serveur est généralement signé par une **autorité de certification**.

Le laboratoire permet donc de comprendre le fonctionnement d'un certificat avant d'introduire la notion de **chaîne de confiance**.

{{%/notice%}}

# 5.3 SSH avec paire de clés

## Objectifs

- générer une paire de clés SSH;
- distinguer une clé privée d'une clé publique;
- utiliser une clé publique pour s'authentifier auprès d'un serveur;
- configurer SSH pour utiliser l'authentification par clé;
- désactiver l'authentification par mot de passe;
- comprendre une application concrète de la cryptographie à clé publique.


## Architecture du laboratoire

![Architecture du laboratoire 2](/images/04-lab2-architecture.png)

+ Deux machines virtuelles vous sont fournies: 
  - **VM1** sera la machine cliente;
  - **VM2** sera le serveur SSH;
+ vous utiliserez VM1 pour vous connecter à VM2;
+ VM2 ne devra plus accepter l'authentification SSH par mot de passe.

{{%notice style="info" title="Important"%}}
Ne désactivez pas l'authentification par mot de passe avant d'avoir vérifié que l'authentification par clé fonctionne correctement.
{{%/notice%}}

## Étapes du laboratoire

##### 1. Identifier les machines

Votre enseignant vous a communiqué les adresses IP de vos deux machines virtuelles :

```text
VM1 : <IP_VM1>
VM2 : <IP_VM2>
```

Vous vous connecterez aux deux machines avec l'utilisateur :

```text
root
```

Dans la suite du laboratoire, remplacez `<IP_VM1>` et `<IP_VM2>` par les adresses IP qui vous ont été fournies.

Vous pouvez confirmer que vous êtes bien sur la bonne machine avec :

```bash
hostname
ip a
```

| Information | VM1 | VM2 |
|---|---|---|
| Nom d'hôte | | |
| Adresse IP | `<IP_VM1>` (fournie) | `<IP_VM2>` (fournie) |
| Utilisateur | root | root |

##### 2. Tester l'authentification actuelle

Depuis VM1, connectez-vous à VM2 avec SSH :

```bash
ssh root@<IP_VM2>
```

Vous devriez actuellement pouvoir vous connecter avec le mot de passe du compte.

Une fois connecté, vérifiez que vous êtes bien sur VM2 :

```bash
hostname
```

Puis quittez la connexion :

```bash
exit
```

##### 3. Générer une paire de clés SSH

Sur **VM1**, générez une paire de clés avec *Ed25519* :

```bash
ssh-keygen -t ed25519
```

Lorsque le programme demande :

```text
Enter file in which to save the key:
```

Appuyez sur **Entrée** pour utiliser l'emplacement proposé.

Vous pouvez ensuite définir une phrase secrète (*passphrase*) pour protéger votre clé privée.


{{%notice style="tip" title="À retenir"%}}

La commande crée généralement deux fichiers :

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

La clé privée :

```text
~/.ssh/id_ed25519
```

La clé publique :

```text
~/.ssh/id_ed25519.pub
```
{{%/notice%}}

##### 4. Observer les deux clés

Affichez les permissions :

```bash
ls -l ~/.ssh/id_ed25519*
```

Affichez votre clé publique :

```bash
cat ~/.ssh/id_ed25519.pub
```

Vous pouvez afficher la clé publique, mais **ne partagez jamais votre clé privée**.

Pour observer les caractéristiques de la clé publique :

```bash
ssh-keygen -lf ~/.ssh/id_ed25519.pub
```

###### **Questions**
1. Quel fichier correspond à la clé privée ?
2. Quel fichier correspond à la clé publique ?
3. Pourquoi la clé privée ne doit-elle jamais être copiée sur le serveur ?
4. Quel algorithme est utilisé par cette paire de clés ?
5. À quoi sert la *passphrase* ?

##### 5. Installer la clé publique sur VM2

Depuis VM1 :

```bash
ssh-copy-id <UTILISATEUR>@<IP_VM2>
```

Le programme vous demandera le mot de passe du compte sur VM2.

Cette étape copie la clé publique sur VM2 dans le fichier :

```text
~/.ssh/authorized_keys
```

##### 6. Vérifier la clé sur VM2

Connectez-vous à VM2 :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Puis vérifiez :

```bash
cat ~/.ssh/authorized_keys
```

Vous devriez retrouver votre clé publique.

##### 7. Tester l'authentification par clé

Depuis VM1 :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Cette fois, SSH devrait utiliser votre clé.

Si vous avez défini une *passphrase*, SSH peut vous demander :

```text
Enter passphrase for key ...
```

Il s'agit de la **phrase secrète protégeant votre clé privée**, et non du mot de passe du compte sur VM2.

##### 8. Vérifier que la clé est utilisée

Utilisez le mode détaillé :

```bash
ssh -v <UTILISATEUR>@<IP_VM2>
```

Repérez dans la sortie les messages indiquant qu'une clé publique est utilisée.

Vous pouvez également utiliser :

```bash
ssh -vv <UTILISATEUR>@<IP_VM2>
```

###### **Questions**

6. Quelle différence y a-t-il entre le mot de passe du compte et la passphrase de la clé privée ?
7. Où la clé publique est-elle enregistrée sur VM2 ?
8. Quelle clé reste sur VM1 ?
9. Quelle clé est copiée sur VM2 ?
<!-- 8. Pourquoi est-il possible de placer la clé publique sur le serveur sans compromettre la clé privée ? -->

##### 9. Désactiver l'authentification par mot de passe

Une fois que vous avez confirmé que l'authentification par clé fonctionne, connectez-vous à VM2 et modifiez :

```bash
sudo nano /etc/ssh/sshd_config
```

Vérifiez que les paramètres suivants sont présents :

```text
PubkeyAuthentication yes
PasswordAuthentication no
```

##### 10. Vérifier la configuration SSH

Avant de redémarrer SSH :

```bash
sudo sshd -t
```

Si aucune erreur n'est affichée, la syntaxe est correcte.

Redémarrez ensuite le service :

```bash
sudo systemctl restart ssh
```

##### 11. Tester l'interdiction du mot de passe

Depuis VM1, votre connexion par clé doit toujours fonctionner :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Pour tester que l'authentification par mot de passe est bien désactivée, vous pouvez forcer temporairement SSH à ne pas utiliser de clé :

```bash
ssh -o PubkeyAuthentication=no <UTILISATEUR>@<IP_VM2>
```

La connexion devrait être refusée.

{{%notice style="warning" title="Attention"%}}
Ne fermez pas votre dernière session SSH fonctionnelle avant d'avoir vérifié que la nouvelle configuration fonctionne.
{{%/notice%}}

### Questions de synthèse

10. Pourquoi peut-on considérer VM1 comme un *jump server* dans cette architecture ?
11. Quelles sont les deux mesures utilisées pour protéger l'accès SSH à VM2 ?
12. Quelle serait la conséquence si quelqu'un obtenait une copie de votre clé privée ?
13. Pourquoi une clé privée protégée par une passphrase est-elle plus sécuritaire qu'une clé privée sans protection ?


# 5.4 Certificat autosigné


## Objectifs

- générer une clé privée avec OpenSSL;
- générer un certificat X.509;
- comprendre le lien entre une clé privée, une clé publique et un certificat;
- créer un certificat autosigné;
- examiner le contenu d'un certificat avec OpenSSL.


##### 1. Vérifier la présence d'OpenSSL

Sur **VM2** :

```bash
openssl version
```

Vous devriez obtenir une version d'OpenSSL.

##### 2. Créer les répertoires nécessaires

Vérifiez que les répertoires existent :

```bash
ls /etc/ssl/private
ls /etc/ssl/certs
```

Nous utiliserons `/etc/ssl/private/` pour la clé privée et `/etc/ssl/certs/` pour le certificat.


##### 3. Générer le certificat autosigné

Dans ce laboratoire, nous allons générer simultanément :

- une clé privée RSA;
- un certificat X.509;
- un certificat autosigné.

Utilisez :

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/nginx-selfsigned.key \
  -out /etc/ssl/certs/nginx-selfsigned.crt
```

OpenSSL vous demandera différentes informations.

Entrez notamment :

```text
Country Name (2 letter code) [AU]: CA
State or Province Name (full name) [Some-State]: Quebec
Locality Name (eg, city) []: Montreal
Organization Name (eg, company) [Internet Widgits Pty Ltd]: Cégep Marie-Victorin
Organizational Unit Name (eg, section) []: Département Techniques de l'informatique
Common Name (e.g. server FQDN or YOUR name) []: <nom du serveur>
Email Address []: <votre adresse courriel du cégep>
```


{{%notice style="tip" title="Pourquoi utiliser `-nodes`?"%}}

L'option :

```text
-nodes
```

demande à OpenSSL de ne pas chiffrer la clé privée avec une passphrase.

Cela simplifie l'utilisation de la clé par Nginx, car Nginx doit pouvoir lire automatiquement sa clé privée lorsqu'il démarre.

Dans un environnement réel, la protection de la clé privée doit être étudiée avec soin.
{{%/notice%}}

##### 4. Vérifier les fichiers créés

```bash
sudo ls -l /etc/ssl/private/nginx-selfsigned.key
sudo ls -l /etc/ssl/certs/nginx-selfsigned.crt
```

Vous avez maintenant :

```text
nginx-selfsigned.key
        │
        └── clé privée

nginx-selfsigned.crt
        │
        └── certificat X.509 autosigné
```

##### 5. Examiner le certificat

Utilisez :

```bash
sudo openssl x509 \
  -in /etc/ssl/certs/nginx-selfsigned.crt \
  -text \
  -noout
```

Repérez notamment :

- `Subject`
- `Issuer`
- `Validity`
- `Not Before`
- `Not After`
- `Public Key Algorithm`
- `Public-Key`
- `Signature Algorithm`

###### **Questions**

1. Qui est le sujet (`Subject`) du certificat ?
2. Qui est l'émetteur (`Issuer`) ?
3. Pourquoi le sujet et l'émetteur sont-ils identiques dans ce certificat ?
4. Quelle est la période de validité du certificat ?
5. Quel algorithme est utilisé pour la clé publique ?
6. Quelle est la taille de la clé ?
7. Quel algorithme est utilisé pour signer le certificat ?
8. Quelle différence y a-t-il entre le certificat et la clé privée ?

##### 6. Vérifier la clé privée

Vous pouvez examiner les informations de la clé avec :

```bash
sudo openssl rsa \
  -in /etc/ssl/private/nginx-selfsigned.key \
  -text \
  -noout
```

{{%notice style="warning" title="Attention"%}}
**Ne transmettez jamais le contenu complet de cette clé à une autre personne.**
{{%/notice%}}

##### 7. Ajouter un nom de domaine au certificat

Dans les certificats modernes, le nom du serveur doit normalement être présent dans l'extension **Subject Alternative Name (SAN)**.

Si votre environnement utilise un nom comme :

```text
benachourgha-pokedex.lan
```

vous pouvez générer un certificat avec :

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/nginx-selfsigned.key \
  -out /etc/ssl/certs/nginx-selfsigned.crt \
  -subj "/C=CA/ST=Quebec/L=Montreal/O=Cegep Marie-Victorin/OU=Departement Techniques de l'informatique/CN=webghazi" \
  -addext "subjectAltName=DNS:benachourgha-pokedex.lan,IP:10.10.2.251"
```

Si vous utilisez directement l'adresse IP du serveur, vous pouvez plutôt ajouter :

```text
-addext "subjectAltName=IP:<IP_VM2>"
```

Vous pouvez aussi avoir les deux :

```bash
-addext "subjectAltName=DNS:<nom-de-domaine-du-serveur>,IP:<IP_VM2>"
```

##### 8. Vérifier le SAN

```bash
sudo openssl x509 \
  -in /etc/ssl/certs/nginx-selfsigned.crt \
  -text \
  -noout
```

Cherchez :

```text
X509v3 Subject Alternative Name:
```

Vous devriez voir le nom et/ou l'adresse IP utilisés.

# 5.5 HTTPS

## Objectifs

- configurer un serveur web (Nginx) pour utiliser un certificat TLS;
- activer HTTPS;
- associer une clé privée à un certificat;
- vérifier une configuration Nginx;
- tester une connexion HTTPS;
- comprendre le fonctionnement d'un certificat autosigné dans un serveur Web.

Vous utiliserez la même VM2 que dans les laboratoires précédents.


Avant le laboratoire, le site fonctionne en `HTTP`. Votre objectif est de permettre également `HTTPS`

##### 1. Vérifier le fonctionnement actuel

Depuis VM1, ouvrez :

```text
http://<IP_VM2>
```

Vous devriez voir le site Web **Pokédex**.

##### 2. Vérifier la configuration actuelle de Nginx

Sur VM2 :

```bash
sudo nginx -t
```

Puis :

```bash
sudo systemctl status nginx
```

Identifiez le fichier de configuration utilisé par le serveur Web.

Sur Ubuntu, vous pouvez notamment examiner :

<!-- ```bash
ls -l /etc/nginx/sites-enabled/
``` 

et :
-->
```bash
ls -l /etc/nginx/sites-available/
```

---

##### 3. Ajouter HTTPS

Ouvrez le fichier de configuration du site :

```bash
sudo nano /etc/nginx/sites-available/default
```

Dans le bloc `server`, ajoutez une configuration HTTPS.

Par exemple :

<!-- ```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name <nom-du-serveur>;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    root <répertoire-du-site>;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
``` -->

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

{{%notice style="note" title="Attention"%}}
Ne remplacez pas aveuglément la configuration existante du site. Ajoutez ou adaptez le bloc HTTPS en conservant les paramètres nécessaires au fonctionnement du Pokédex.
{{%/notice%}}

##### 4. Vérifier la configuration

Avant de redémarrer Nginx :

```bash
sudo nginx -t
```

Vous devriez obtenir :

```text
syntax is ok
test is successful
```

Si une erreur apparaît, **ne redémarrez pas Nginx**. Corrigez d'abord la configuration.

##### 5. Redémarrer Nginx

```bash
sudo systemctl restart nginx
```

Vérifiez ensuite :

```bash
sudo systemctl status nginx
```

<!-- ##### 6. Tester HTTPS

Depuis VM1 :

```bash
curl -k https://<IP_VM2>
```

L'option :

```text
-k
```

demande à `curl` d'accepter un certificat qui n'est pas reconnu par une autorité de certification de confiance.

Vous devriez recevoir le contenu du site Pokédex. -->


##### 6. Tester avec un navigateur

Dans un navigateur, ouvrez :

```text
https://<IP_VM2>
```

Vous devriez voir le site.

Le navigateur devrait toutefois afficher un avertissement concernant le certificat.

C'est **normal** dans ce laboratoire.

Pourquoi ?

Parce que le certificat a été créé par vous-même :

```text
VM2
 │
 └── certificat autosigné
```

Il n'a pas été signé par une autorité de certification reconnue par le navigateur.

##### 8. Examiner le certificat depuis le navigateur

Affichez les informations du certificat.

Vérifiez notamment :

- le sujet;
- l'émetteur;
- la période de validité;
- la clé publique;
- le nom du serveur;
- le fait que le certificat soit autosigné.

Comparez ces informations avec celles obtenues au laboratoire 3.

###### **Questions**
1. Pourquoi le navigateur affiche-t-il un avertissement ?
2. Le certificat est-il nécessairement faux ou mal formé parce qu'il est autosigné ?
3. Quelle différence y a-t-il entre un certificat autosigné et un certificat signé par une CA reconnue ?
4. Pourquoi le certificat contient-il une clé publique ?
5. Où se trouve la clé privée utilisée par Nginx ?
6. Pourquoi cette clé doit-elle rester secrète ?
7. Quel est le rôle du certificat dans une connexion HTTPS ?
8. Quel est le rôle de la clé privée dans le serveur Nginx ?
9. Pourquoi votre navigateur ne considère-t-il pas automatiquement votre certificat autosigné comme digne de confiance ?
10. Dans un environnement réel, comment pourrait-on remplacer le certificat autosigné utilisé dans ce laboratoire ?