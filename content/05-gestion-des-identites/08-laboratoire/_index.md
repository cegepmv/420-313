+++
title = 'Laboratoire'
weight = '580'
draft = true
+++
# Cassage de mots de passe
---------

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
Le but est d'expérimenter avec des mots de passe fictifs dans un environnement contrôlé. Vous ne devez pas tenter de casser les mots de passe d'un autre utilisateur ou d'un système qui ne vous appartient pas.
{{%/notice%}}

### Préparation de l'environnement

Vous utiliserez **John the Ripper**, un outil permettant notamment de tester des mots de passe à partir de leur représentation hachée.

Vérifiez que John the Ripper est installé :

```bash
john --version
```

Vous devriez obtenir une information indiquant la version installée.

Vous pouvez également afficher l'aide :

```bash
john --help
```

#### Préparer le fichier de travail

Un fichier nommé `hashes.txt` vous est fourni.

Il contient plusieurs représentations hachées de mots de passe fictifs.

Examinez son contenu :

```bash
cat hashes.txt
```

Vous devriez obtenir plusieurs lignes ressemblant à :

```text
...
...
...
```


**Question :** Pourquoi un système sécurisé ne devrait-il normalement pas conserver les mots de passe des utilisateurs directement en clair ?

