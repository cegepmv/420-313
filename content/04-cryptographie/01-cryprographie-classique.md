
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

En utilisant la même table de substitution montrée ci-dessus Quel est le texte en clair?
{{%/notice%}}

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

## Le masque jetable (*One-Time Pad*)

Inventé par le capitaine Vernam (US Army Signal Corps, 1919), utilisé entre autres pour le téléphone rouge Moscou-Washington durant la guerre froide.

**Conditions de sécurité parfaite (Shannon)** :
1. La clé doit être *aussi longue* que le message.
2. La clé doit être générée de manière *parfaitement aléatoire*.
3. La clé ne doit *jamais être réutilisée*.

Si ces conditions sont respectées, le masque jetable est le **seul système cryptographique dont la sécurité est mathématiquement parfaite** : sans la clé, tous les messages de la même longueur sont également probables, rendant la force brute inutile en pratique. En contrepartie, la distribution et la gestion d'une clé aussi longue que le message (et jamais réutilisée) sont extrêmement contraignantes, ce qui limite son usage à des contextes très spécifiques (diplomatie, renseignement).

### Fonctionnement général
Imaginons qu’on veut chiffrer le mot “ATTAQUE” avec une clé qui représente un décalage alphabétique, comme `12-2-4-5-1-20-1`:

||
|--------------------|-----|----|---|---|---|----|----|
|**Original**        |	A  |	T |	T |	A |	Q |	U  |	E |
|**Clé**             |	12 |	2 |	4 |	5 |	1 |	20 |	1 |
|**Message chiffré** |	M  |	V |	X |	F |	R |	O  |	F |

Si on veut déchiffrer `MVXFROF` sans connaître la clé, la seule manière d’y arriver est de tester toutes les clés possibles. Puisque chaque “lettre” de la clé a 26 valeurs possibles et que la taille de la clé est de 7 (la même que le mot), on a 26⁷ clés possibles à tester soit 803 181 176 possibilités. Et surtout, si on arrive à les énumérer et les utiliser pour tenter de déchiffrer `MVXFROF`, on aura obtenu au passage tous les mots de 7 lettres possibles en français… Par exemple, le mot “Bananes”, si on le chiffre avec la clé 12-21-9-5-4-9-13 donne aussi MVXFROF. 

Comment alors savoir si le message original est “Attaque” ou “Bananes”, ou “Compter”, “Jardins”, etc? Il faut avoir la clé pour en être sûr.


### Utilisation concrète
Dans les systèmes cryptographiques modernes, on ne chiffre pas les messages en utilisant un décalage mais plutôt en utilisant l’opération logique XOR (“ou exclusif”) sur les bits du message en clair. La clé est une séquence de bits (0 et 1) aussi longue que le message. Le chiffrement s'effectue par un OU EXCLUSIF (**XOR**) bit-à-bit entre le message et la clé.

|x|	y|	x ⊕ y|
|-|-|-|---|
|0|	0|	0|
|0|	1|	1|
|1|	0|	1|
|1|	1|	0|

Le chiffrement du mot “POMME” consiste donc à se baser sur la représentation binaire du mot, par exemple l’encodage ASCII, à générer une clé binaire aléatoire de même taille et d’appliquer l’opération XOR:


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

<!-- {{% expand "Réponse" %}}
1. nuage
2. bim
{{% /expand %}} -->
