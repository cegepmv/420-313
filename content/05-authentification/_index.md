+++
pre = '<b>05. </b>'
title = 'Gestion des identités'
weight = '500'
draft = false
+++
-------------

## Authentification et autorisation

- **Authentification** : confirmer qu'un utilisateur est bien celui qu'il prétend être.
- **Autorisation** : déterminer ce qu'un utilisateur authentifié a le droit de faire ou de consulter.

{{%notice style="info" title="Deux notions distinctes"%}}
Ces deux notions, bien que souvent confondues, sont *distinctes* : on peut être authentifié sans être autorisé à effectuer une action donnée.

**Exemple :** un étudiant peut s'authentifier avec son compte du cégep, mais ne pas être autorisé à modifier les notes d'un cours. Un enseignant, avec le même système d'authentification, pourrait avoir cette permission.
{{%/notice%}}

## Facteurs d'authentification

![Les trois facteurs d'authentification côte à côte](/images/05-MFA.png?width=48rem)

L'authentification peut reposer sur différents **facteurs** :

- **Ce que je sais** : mot de passe, NIP, question secrète.
- **Ce que je possède** : téléphone, jeton matériel, carte à puce.
- **Ce que je suis** : biométrie (empreinte digitale, reconnaissance faciale).

L'**authentification multifacteur (MFA/2FA)** combine au moins deux facteurs d'authentification différents..

Par exemple, un utilisateur peut devoir fournir :

1. son mot de passe (**ce que je sais**) ;
2. puis un code généré par une application d'authentification ou une clé de sécurité (**ce que je possède**).

Cela réduit considérablement le risque qu'un attaquant compromette un compte avec un seul élément volé, par exemple un mot de passe divulgué lors d'une violation de données.

