+++
pre = '1- '
title = 'Attaque par dictionnaire'
weight = '581'
draft = true
+++
-----------

Une attaque par dictionnaire consiste à essayer successivement des mots de passe provenant d'une liste.

L'attaquant ne teste donc pas nécessairement toutes les combinaisons possibles. Il commence plutôt par des valeurs qui ont de fortes chances d'être utilisées.

Le cours donne notamment comme exemples des mots de passe courants comme `12345`, `qwerty` ou d'autres mots présents dans les dictionnaires.

### 1. Préparer un dictionnaire

Un fichier `wordlist.txt` vous est fourni. Vous pouvez le télécharger avec la commande suivante : 
```bash
wget ....
```

Examinez son contenu :

```bash
cat wordlist.txt
```

Vous pouvez compter le nombre de mots présents dans le fichier :

```bash
wc -l wordlist.txt
```

**Question :** Pourquoi une attaque par dictionnaire peut-elle être beaucoup plus efficace qu'une force brute lorsque les utilisateurs choisissent des mots de passe prévisibles ?

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

**Question :** Est-ce que tous les mots de passe ont été retrouvés ? Expliquez pourquoi certains peuvent être retrouvés par dictionnaire et d'autres non.

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

**Questions :**
1. *Pourquoi un mot de passe comme `Password123` peut-il être beaucoup plus facile à retrouver qu'un mot de passe aléatoire de longueur comparable ?*
2. *Le fait d'ajouter une majuscule, un chiffre ou un symbole rend-il automatiquement un mot de passe robuste ? Justifiez votre réponse.*