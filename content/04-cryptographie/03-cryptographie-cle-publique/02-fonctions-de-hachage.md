+++
title = 'Fonctions de hachage'
weight = '432'
draft = false
+++
-------------

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

<!-- ### Structures de données

Les fonctions de hachage sont également utilisées en programmation pour construire des structures de données comme les **tables de hachage** (*hash tables*).

Elles permettent notamment de retrouver efficacement une valeur à partir d'une clé. -->

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
