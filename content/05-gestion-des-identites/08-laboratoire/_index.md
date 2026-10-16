+++
title = 'Laboratoire'
weight = '580'
draft = false
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
Le but est d'expérimenter avec des mots de passe fictifs dans un environnement contrôlé. **Vous ne devez pas tenter de casser les mots de passe d'un autre utilisateur ou d'un système qui ne vous appartient pas**.
{{%/notice%}}

### Préparation de l'environnement
Vous disposez de la VM **Ubuntu** *jump server* fournie lors des laboratoires du chapitre *4. Cryptographie*. Vous allez d'abord y installer **John the Ripper (version jumbo)**, puis télécharger les fichiers du laboratoire.

##### 1. Installer John the Ripper (jumbo)

La version présente dans les dépôts d'Ubuntu (paquet `john`) est une version de base plus limitée. Nous allons plutôt compiler la version **jumbo**, qui prend notamment en charge l'option `--mask` et la détection automatique de nombreux formats de hachage.

Installez d'abord les dépendances nécessaires à la compilation :

```bash
sudo apt update
sudo apt install -y build-essential git libssl-dev zlib1g-dev yasm pkg-config \
    libgmp-dev libpcap-dev libbz2-dev libomp-dev unzip
```

Récupérez ensuite le code source et compilez-le :

```bash
git clone https://github.com/openwall/john -b bleeding-jumbo john
cd john/src
./configure
make -s clean && make -sj4
```

La compilation peut prendre quelques minutes. Une fois terminée, l'exécutable se trouve dans le dossier `john/run`.

Placez-vous dans ce dossier pour la suite du laboratoire :

```bash
cd ../run
```

Vérifiez que John the Ripper fonctionne :

```bash
./john --version
```

Vous devriez obtenir une information indiquant la version installée (elle doit mentionner **jumbo**).

Vous pouvez également afficher l'aide :

```bash
./john --help
```

{{%notice style="tip" title="Rendre la commande plus pratique"%}}
Dans la suite de l'énoncé, les commandes sont écrites `john ...`. Comme l'exécutable se trouve dans `john/run`, vous devrez soit lancer les commandes depuis ce dossier avec `./john ...`, soit ajouter un alias, par exemple :

```bash
alias john=~/john/run/john
```
{{%/notice%}}

##### 2. Télécharger les fichiers du laboratoire

Les fichiers nécessaires au laboratoire sont fournis dans une archive. Téléchargez-la puis décompressez-la :

```bash
wget https://github.com/cegepmv/420-313/raw/refs/heads/main/assets/files/labo-cassage-mdp.zip -O labo-cassage-mdp.zip
unzip labo-cassage-mdp.zip -d labo-mdp
cd labo-mdp
```

<!-- {{%notice style="info" title="Si unzip n'est pas installé"%}}
```bash
sudo apt install -y unzip
```
{{%/notice%}} -->

L'archive contient les fichiers suivants :

```text
hashes.txt
hashes_predictibles.txt
hashes_numeric.txt
hashes_comparaison.txt
wordlist.txt
wordlist_predictible.txt
```

##### 3. Examiner le fichier de travail

Un fichier nommé `hashes.txt` vous est fourni.

Il contient plusieurs représentations hachées de mots de passe fictifs.

Examinez son contenu :

```bash
cat hashes.txt
```

Chaque ligne est de la forme `nom_utilisateur:hachage`. Les mots de passe sont hachés avec **bcrypt** (les hachages commencent par `$2b$`).

**Question :** *Pourquoi un système sécurisé ne devrait-il normalement pas conserver les mots de passe des utilisateurs directement en clair ?*

{{%notice style="tip" title="Recommencer une attaque"%}}
John "se souvient" des mots de passe déjà trouvés dans un fichier appelé *pot* (`~/john/run/john.pot`). Si vous voulez recommencer une attaque à zéro, supprimez ce fichier :

```bash
rm ~/john/run/john.pot
```
{{%/notice%}}