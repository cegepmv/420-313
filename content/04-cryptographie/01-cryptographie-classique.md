
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

<!-- {{% expand "Solution" %}}
“Le vieux pêcheur contemplait paisiblement le coucher de soleil”
{{% /expand %}} -->