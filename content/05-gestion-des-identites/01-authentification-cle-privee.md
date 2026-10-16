+++
title = 'Authentification par clé'
weight = '510'
draft = false
+++
-------------

Comme vu au chapitre 4, une **clé privée** peut servir à authentifier un utilisateur.

L'idée est de démontrer que l'utilisateur possède la clé privée associée à une clé publique connue, sans avoir à transmettre la clé privée elle-même.

Par exemple, avec **SSH**, un utilisateur peut posséder une paire de clés :

- une **clé publique**, enregistrée sur le serveur ;
- une **clé privée**, conservée par l'utilisateur.

Lors de la connexion, le serveur peut vérifier que l'utilisateur possède bien la clé privée correspondante. **La clé privée n'est jamais envoyée au serveur**.

{{%notice style="tip" title=" "%}}
Comme vu dans le laboratoire sur SSH du chapitre 4, cette approche permet notamment d'éviter les problèmes associés aux mots de passe et peut être particulièrement utile pour l'administration de serveurs.
{{%/notice%}}
