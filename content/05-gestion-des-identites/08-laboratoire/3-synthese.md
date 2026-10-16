+++
pre = '3- '
title = 'Synthèse'
weight = '583'
draft = true
+++
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

Effectuez les attaques nécessaires pour retrouver les mots de passe.

Complétez :

| Mot de passe | Longueur | Type | Dictionnaire | Force brute | Difficulté observée |
|---|---:|---|---|---|---|
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |

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

Jusqu'à présent, vous avez travaillé directement avec des hashes.

Cela représente une situation où un attaquant a réussi à obtenir une copie des données contenant les mots de passe hachés.

Il peut alors effectuer ses tentatives **hors ligne**, sans avoir à communiquer avec le serveur à chaque tentative.

Comparez cette situation avec une attaque contre une page de connexion :

```text
Attaquant
    │
    │  mot de passe
    ▼
Serveur
    │
    └── accepté / refusé
```

Dans ce deuxième cas, le serveur peut mettre en place des mécanismes tels que :

- limitation du nombre de tentatives ;
- délai entre les tentatives ;
- verrouillage temporaire ;
- détection d'activités inhabituelles ;
- MFA.

**Question :**
1. *Pourquoi le fait de posséder une copie des hashes peut-il être particulièrement dangereux pour un attaquant ?*
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

> « Mon mot de passe contient une majuscule, des minuscules, des chiffres et un symbole. Il est donc très sécurisé. »

Vous devez évaluer cette affirmation.

1. Quels éléments rendent ce mot de passe prévisible ?
2. Quelle attaque pourriez-vous essayer en premier ?
3. Proposez une meilleure stratégie de création de mot de passe. Votre réponse doit expliquer **pourquoi** votre stratégie est plus sécuritaire et non simplement donner un exemple de mot de passe.

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