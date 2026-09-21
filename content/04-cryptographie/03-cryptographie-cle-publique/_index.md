+++
title = 'Cryptographie à clé publique'
weight = '430'
draft = false
+++

-------------

## Symétrique vs. asymétrique

![Schéma comparatif symétrique (une clé partagée) vs asymétrique (paire de clés).](/images/04-symetrique-vs-asymetrique.png)

### Chiffrement symétrique

Les deux parties utilisent la **même clé** pour chiffrer et déchiffrer. Il faut donc que **tous les participants possèdent la clé**, et qu'elle ait été échangée de façon sécurisée au préalable.

{{%notice style="info" title="Algorithmes courants"%}}
**Principaux algorithmes modernes**
+ **AES (*Advanced Encryption Standard*) :** Le standard mondial actuel, qui traite les données par blocs de 128 bits avec des clés de 128, 192 ou 256 bits.
+ **ChaCha20 / XChaCha20 :** Un algorithme de type chiffrement par flot très rapide et sécurisé, souvent utilisé sur les appareils mobiles et le Web.
+ **Twofish et Serpent :** Des alternatives robustes qui étaient finalistes lors du concours pour devenir l'AES

**Anciens algorithmes obsolètes**
+ **DES (*Data Encryption Standard*) :** Ancien standard américain abandonné car sa clé de 56 bits est trop courte et vulnérable. 
+ **3DES (Triple DES) :** Une évolution de DES qui applique l'algorithme trois fois de suite, aujourd'hui déconseillé car trop lent et remplacé par l'AES.
+ **RC4 :** Un chiffrement par flot autrefois très populaire, mais abandonné en raison de failles de sécurité majeures.
{{%/notice%}}


### Chiffrement asymétrique

#### Échange de clés Diffie-Hellman

Jusqu'en 1976, le problème central de la cryptographie restait le même depuis l'Antiquité : comment s'assurer que l'échange initial d'une clé se fasse en toute confidentialité ? **Whitfield Diffie** et **Martin Hellman** ont décrit un mécanisme permettant à deux interlocuteurs d'obtenir la **même clé** sans jamais l'échanger directement.

**Principe simplifié** :
- A et B s'entendent sur une information commune publique, **x**.
- A choisit un secret **y**, combine **x** et **y**, et transmet le résultat (xy) à **B**.
- B choisit un secret **z**, combine **x** et **z**, et transmet le résultat (xz) à A.
- Chacun combine ce qu'il a reçu avec son propre secret pour obtenir la même clé finale, sans que **y** ni **z** n'aient jamais transité sur le réseau.

**Algorithme (version mathématique) :**
1. A et B échangent publiquement un nombre premier **P** et un générateur **G**.
2. Chacun choisit secrètement un nombre **X** (inférieur à P).
3. Chacun calcule `Y = G^X mod P` et transmet le résultat à l'autre.
4. Chacun calcule la clé `C = Y^X mod P` à partir du Y reçu et de son propre X.

La clé ainsi calculée, appelée **clé de session**, n'est utilisée que pour cette communication. Ce mécanisme est à la base de nombreux protocoles modernes, dont **HTTPS**.

**Exemple :**
![Exemple de l'algorithme Diffie-Hellman](/images/04-diffieh.png)

+ A et B s’entendent sur des valeurs pour **P** et **G** (respectivement `37` et `4`).
+ Pour la valeur de **X**, A choisit `7` et **B** choisit **12**. 
+ La clé calculée par A et B à partir de ces valeurs vaut 10.

{{% notice tip "Exercice" %}}
A et B s'entendent sur `P=57` et `G=14`. A choisit `X=5`, B choisit `X=16`. Quelle est la clé finale calculée par les deux parties ?

Vous pouvez utiliser la calculatrice sur [ce site](https://planetcalc.com/8326/).

+ Y = G^X % P
+ C = Y^X % P
{{% /notice %}}

## Clés publiques
Les travaux de Diffie et Hellman ont pavé la voie à une deuxième manière de régler le problème de l’échange de clé. **La cryptographie à clé publique** a été mise au point en 1978 par trois cryptologues: Ronald Rivest, Adi Shamir et Leonard Adleman. L’algorithme se nomme **RSA**, des noms de ses trois inventeurs.

Dans ce système les deux parties utilisent des **clés différentes** : une **clé publique**, qui sert à chiffrer, et une **clé privée**, que seul son détenteur possède et qui sert à déchiffrer. Ce qui est chiffré avec une clé ne peut être déchiffré qu'avec l'autre. Les deux clés fonctionnent exclusivement l’une avec l’autre, comme un cadenas et sa combinaison.

- La clé publique peut être distribuée sans limite.
- La clé privée doit rester *strictement confidentielle*.

{{%notice style="note" title="Problème 1"%}}
Si la clé publique n'est pas fournie de façon fiable, un attaquant pourrait substituer sa propre clé publique et mener une **attaque de l'homme du milieu** — d'où le rôle des **certificats** émis par une autorité de confiance (voir le chapitre sur les cer).
{{%/notice%}}

{{%notice style="note" title="Problème 2"%}}
Le chiffrement asymétrique est nettement plus lent (souvent ~1000 fois) que le chiffrement symétrique, ce qui explique pourquoi il est généralement utilisé **seulement pour échanger une clé symétrique de session**, plutôt que pour chiffrer l'ensemble d'une communication.
{{%/notice%}}

{{%notice style="info" title="Principaux algorithmes"%}}
+ **RSA :** Algorithme de cryptographie à clé publique reposant sur les propriétés mathématiques de la factorisation des grands nombres. Il peut notamment être utilisé pour certaines opérations de chiffrement et de signature.
+ **DSA (*Digital Signature Algorithm*):** Algorithme conçu pour les signatures numériques.
+ **ECC (Courbes Elliptiques) :** Famille de techniques cryptographiques utilisant les mathématiques des courbes elliptiques. Elles permettent d'obtenir une sécurité comparable à certains systèmes RSA avec des clés de taille plus réduite.
{{%/notice%}}

[Reel explicatif](https://www.youtube.com/shorts/4vUeGKPl3mU)

<!-- ### Chiffrer ou signer ?

- **Chiffrer avec la clé publique** : seule la personne possédant la clé privée correspondante pourra déchiffrer → garantit la **confidentialité**.
- **Chiffrer (signer) avec la clé privée** : n'importe qui possédant la clé publique peut déchiffrer, donc aucune confidentialité — mais le fait que le message se déchiffre correctement avec la clé publique prouve qu'il provient bien du détenteur de la clé privée → garantit l'**authenticité** (signature numérique). [Vidéo Explicative](https://www.youtube.com/watch?v=JR4_RBb8A9Q)
 -->
