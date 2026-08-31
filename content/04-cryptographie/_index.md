+++
pre = '<b>04. </b>'
title = 'Cryptographie'
weight = '400'
draft = false
+++

---------
La **cryptographie** est un élément fondamental de la sécurité informatique. Elle permet notamment de protéger la **confidentialité**, l'**intégrité** et l'**authenticité** des informations échangées.

Dans ce chapitre, nous découvrirons l'évolution de la cryptographie, des **chiffres classiques** aux mécanismes modernes comme le **chiffrement symétrique et asymétrique**, l'**échange de clés**, les **fonctions de hachage**, les **signatures numériques** et les **certificats**. Enfin, nous aborderons les enjeux liés à l'**informatique quantique* et à la **cryptographie post-quantique**.*

## Terminologie et histoire

Dans tout système cryptographique, on retrouve les éléments suivants :

- Le **message en clair** (*cleartext*) : ce qu'on veut chiffrer.
- Le **message chiffré** (*ciphertext*) : le résultat du chiffrement.
- Le **chiffre** (*cipher*) : la méthode utilisée pour chiffrer.
- La **clé** : l'information secrète utilisée pour chiffrer et déchiffrer les messages.

Le but d'un système cryptographique est de rendre très difficile (idéalement impossible) de dériver le message en clair à partir du message chiffré sans posséder la clé. Pour bien des systèmes, il est moins difficile de se procurer la clé que de déchiffrer directement le message — d'où l'importance centrale, à travers l'histoire, de la **confidentialité de la clé**.

### Les grandes ères de la cryptographie

![frise chronologique des grandes ères de la cryptographie (classique → moderne → âge d'or → post-quantique](/images/04-histoire-crypto.png)

- **Ère classique** : jusqu'à l'avènement des machines à chiffrer (chiffrement manuel, faible) — premiers exemples dans l'Égypte ancienne et l'Empire romain.
- **Ère moderne** : début du XXe siècle, avec l'avènement des machines électromécaniques (*Enigma* pendant la Seconde Guerre mondiale), puis électroniques (*DES*).
- **L'âge d'or** : 1976-1978, avec l'invention de la **cryptographie à clé publique** (Diffie-Hellman, puis RSA).
- **Ère post-quantique (émergente)** : recherche actuelle sur des algorithmes résistants aux ordinateurs quantiques.
