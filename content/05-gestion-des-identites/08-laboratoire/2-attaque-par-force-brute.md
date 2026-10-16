+++
pre = '2- '
title = 'Attaque par force brute'
weight = '582'
draft = false
+++
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

