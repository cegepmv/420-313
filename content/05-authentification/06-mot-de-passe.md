+++
title = 'Mots de passe'
weight = '560'
draft = false
+++
-------------

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

{{%notice style="info" title="La longueur reste essentielle"%}}

Ces calculs montrent l'importance de la **longueur** et de l'unicité d'un mot de passe.

En pratique, cependant, un attaquant ne teste pas nécessairement toutes les combinaisons possibles. Les mots de passe courants, les informations personnelles et les modèles prévisibles peuvent être testés en priorité.

Un mot de passe long mais prévisible, comme `MotDePasse123456`, peut donc être beaucoup plus facile à trouver qu'un mot de passe généré aléatoirement.

L'**authentification multifacteur** demeure par conséquent une protection importante, même lorsqu'un utilisateur possède un mot de passe robuste.
{{%/notice%}}