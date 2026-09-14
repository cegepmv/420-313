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