+++
title = 'HTTPS et TLS'
weight = '436'
draft = false
+++
-------------
## HTTPS et TLS

Les connexions **HTTPS** utilisent le protocole **TLS (Transport Layer Security)**.

TLS combine plusieurs mécanismes cryptographiques :

- la cryptographie à clé publique ;
- les certificats numériques ;
- l'échange de clés ;
- la cryptographie symétrique ;
- les fonctions de hachage et les signatures.

![Étapes de connexion HTTPS/TLS](/images/04-https-tls.png)

Lorsqu'un navigateur se connecte à un serveur HTTPS, il doit notamment :

1. obtenir le certificat du serveur ;
2. vérifier le certificat et sa chaîne de confiance ;
3. vérifier que le certificat correspond au domaine demandé ;
4. établir les paramètres cryptographiques de la connexion ;
5. établir une clé ou des clés de session ;
6. utiliser ensuite une cryptographie symétrique efficace pour protéger les données échangées.

On retrouve donc les différentes notions étudiées dans ce chapitre :

![Résumé de toutes les technologies et notions utilisées par HTTPS/TLS](/images/04-https-tls-resume.png)

{{%notice style="tip" title="À retenir"%}}

Dans une connexion HTTPS moderne, la cryptographie asymétrique et les certificats servent principalement à **établir la confiance et les paramètres de la connexion**, tandis que la cryptographie symétrique est utilisée pour protéger efficacement les données échangées.

{{%/notice%}}
