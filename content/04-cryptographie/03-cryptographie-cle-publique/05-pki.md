+++
title = 'PKI'
weight = '435'
draft = false
+++
-------------

<!-- ## La chaîne de confiance

Dans la pratique, un navigateur ne possède pas nécessairement directement la clé publique de chaque autorité qui signe les certificats des serveurs.

Les certificats peuvent former une **chaîne de confiance**.
```text

Autorité racine (Root CA)
          │
          ▼
Autorité intermédiaire
          │
          ▼
Certificat du serveur
          │
          ▼
        HTTPS
```

Les systèmes d'exploitation et les navigateurs disposent d'un ensemble d'**autorités de certification racines de confiance**.

Lorsqu'un navigateur reçoit un certificat, il peut vérifier progressivement la chaîne jusqu'à une autorité racine qu'il reconnaît.

{{%notice style="tip" title="À retenir"%}}

La cryptographie à clé publique permet de vérifier une signature ou d'utiliser une clé publique, mais elle ne permet pas à elle seule de savoir **à qui appartient cette clé**.

Les certificats et la PKI ajoutent une infrastructure permettant d'établir cette relation de confiance.

{{%/notice%}} -->

## Infrastructure à clé publique (PKI)

L’ensemble des mécanismes, technologies et organisations permettant de gérer les clés, les certificats et les relations de confiance constitue une **infrastructure à clé publique (PKI — Public Key Infrastructure)**.

Une PKI peut notamment comprendre :

- des **autorités de certification (CA)** ;
- des certificats numériques ;
- des mécanismes de gestion et de validation des certificats ;
- des listes ou mécanismes permettant de déterminer si un certificat est encore valide ou doit être révoqué.

La PKI permet ainsi de construire une **chaîne de confiance** entre une clé publique et une identité.
```text

Autorité racine
       │
       ▼
Certificat
       │
       ▼
Clé publique du serveur
       │
       ▼
Identité / domaine
```