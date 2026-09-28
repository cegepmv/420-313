+++
title = 'Cryptographie post-quantique'
weight = '450'
draft = false
+++
-------------

## Regard vers l'avenir : l'informatique quantique

Toute la cryptographie à clé publique vue dans ce chapitre (*RSA*, *Diffie-Hellman*) repose sur des problèmes mathématiques réputés difficiles à résoudre pour un ordinateur classique, principalement la *factorisation de grands nombres* et le *calcul de logarithme discret*.

## Algorithme de Shor

En 1994, le mathématicien **Peter Shor** a démontré qu'un **ordinateur quantique** suffisamment puissant pourrait résoudre ces deux problèmes de façon efficace, rendant potentiellement obsolètes *RSA* et *Diffie-Hellman* tels qu'utilisés aujourd'hui.

{{%notice style="info" title="Nuance importante"%}}
Cette menace concerne spécifiquement le chiffrement **asymétrique**. Le chiffrement **symétrique** (AES) et les fonctions de **hachage** demeurent globalement robustes face aux ordinateurs quantiques.
{{%/notice%}}

## Où en est-on réellement ?

Le développement d'un ordinateur quantique capable de casser *RSA* en pratique nécessite un nombre de **qubits logiques** (après correction d'erreurs) largement supérieur à ce qui est démontré publiquement à ce jour. 

{{%notice style="note" title="Distinction importante"%}}
Il est important de distinguer la **recherche théorique**, bien établie, de la **menace opérationnelle** à court terme, qui reste incertaine et dépend de percées technologiques encore à venir.
{{%/notice%}}

## « *Harvest now, decrypt later* »

Une des raisons pour lesquelles il faut commencer à se préparer dès maintenant est une stratégie connue sous le nom de :

> **Harvest now, decrypt later**
>
> *Collecter maintenant, déchiffrer plus tard*.

Imaginons qu'un attaquant intercepte aujourd'hui une communication chiffrée avec une technologie à clé publique vulnérable à un futur ordinateur quantique. L'attaquant ne peut peut-être pas la déchiffrer aujourd'hui mais il peut cependant :

1. intercepter la communication;
2. conserver les données chiffrées;
3. attendre qu'une technologie suffisamment puissante soit disponible;
4. tenter de déchiffrer les données plusieurs années plus tard.


Cette menace est particulièrement importante pour les informations qui doivent rester confidentielles pendant de nombreuses années.

Par exemple :
+ des dossiers médicaux;
+ des secrets industriels;
+ des données de recherche;
+ des informations gouvernementales;
+ des données personnelles sensibles;
+ des communications diplomatiques ou stratégiques.

{{%notice style="info" title="Recommendations du NIST"%}}
**NIST** identifie explicitement le scénario *harvest now, decrypt later* comme une raison de commencer la transition vers la cryptographie post-quantique avant l'apparition d'un ordinateur quantique capable de casser les systèmes actuels.
{{%/notice%}}

## La cryptographie post-quantique

La **cryptographie post-quantique**, ou **PQC** (*Post-Quantum Cryptography*), désigne des algorithmes cryptographiques conçus pour résister aux attaques provenant aussi bien des ordinateurs classiques que des ordinateurs quantiques.

L'objectif n'est pas de créer une cryptographie qui fonctionne sur un ordinateur quantique.

Au contraire, les algorithmes post-quantiques sont généralement destinés à fonctionner sur les infrastructures informatiques classiques que nous utilisons aujourd'hui.

L'idée est plutôt de **remplacer progressivement** les mécanismes de cryptographie à clé publique vulnérables à l'algorithme de Shor.

### Les algorithmes « *quantum-resistant* »

Les algorithmes post-quantiques s'appuient sur d'autres problèmes mathématiques réputés difficiles même pour un ordinateur quantique, notamment :

- les **réseaux euclidiens** (*lattices*) ;
- les **codes correcteurs d'erreurs** ;
- certaines constructions basées sur le **hachage**.

{{%notice style="note" title="Nuance importante"%}}
Un algorithme est considéré comme **post-quantique** parce que sa sécurité est conçue pour résister aux attaques connues provenant d'ordinateurs quantiques. Cela ne signifie pas qu'il est démontré mathématiquement impossible à casser.
{{%/notice%}}

### Les premiers standards post-quantiques du NIST

Le **NIST** (*National Institute of Standards and Technology*) a organisé pendant plusieurs années un processus international d'évaluation et de sélection d'algorithmes post-quantiques.

En août 2024, trois premiers standards ont été finalisés :

|Standard|	Algorithme|	Fonction principale|	Famille|
|-------|--------|--------|-----|
|**FIPS 203**|	**ML-KEM**|	Établissement de clé	|Réseaux euclidiens|
|**FIPS 204**|	**ML-DSA**|	Signature numérique	|Réseaux euclidiens|
|**FIPS 205**|	**SLH-DSA**|	Signature numérique|	Hachage|

<!-- 
ML-KEM

ML-KEM (Module-Lattice-Based Key-Encapsulation Mechanism) permet à deux parties d'établir un secret partagé à travers un canal public.

Il joue donc un rôle comparable, dans une architecture moderne, à celui que peuvent jouer les mécanismes d'établissement de clés que nous avons étudiés avec Diffie-Hellman.

Il ne s'agit toutefois pas simplement d'une « nouvelle version de Diffie-Hellman » : sa construction mathématique est différente et repose sur des problèmes liés aux réseaux euclidiens.




ML-DSA

ML-DSA (Module-Lattice-Based Digital Signature Algorithm) est un algorithme de signature numérique basé sur les réseaux euclidiens.

Il vise donc à remplacer, dans certains usages, des mécanismes de signature classiques vulnérables aux attaques quantiques.




SLH-DSA

SLH-DSA (Stateless Hash-Based Digital Signature Algorithm) est également un algorithme de signature numérique, mais il repose sur une approche différente : les fonctions de hachage.

Cette diversité est importante : utiliser plusieurs familles mathématiques permet de ne pas dépendre d'une seule hypothèse de sécurité.

SLH-DSA est notamment dérivé de SPHINCS+.
 -->

## Approches de transition

Dans l'attente d'une adoption complète des algorithmes post-quantiques, plusieurs organisations déploient déjà des solutions **hybrides**, combinant un algorithme classique (*RSA* ou *ECC*) et un algorithme post-quantique pour un même échange de clé, afin de rester protégées même si l'un des deux algorithmes venait à être cassé.