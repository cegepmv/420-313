+++
title = "Attaques liées à l'identité"
weight = '570'
draft = false
+++
-------------

Les systèmes d'authentification et d'autorisation sont des cibles importantes pour les attaquants. Plusieurs attaques cherchent directement à **voler une identité**, à **contourner l'authentification** ou à **obtenir des privilèges supplémentaires**.

## Hameçonnage (phishing)

Le **hameçonnage** consiste à tromper un utilisateur afin qu'il fournisse volontairement des informations sensibles, par exemple son mot de passe ou un code d'authentification.


L'attaquant peut créer une fausse page de connexion ressemblant à celle d'un service légitime.

**Exemple :**
1. Un utilisateur reçoit un courriel lui demandant de se connecter à son compte. Le lien mène vers un faux site qui reproduit l'apparence du véritable service.
2. L'utilisateur saisit alors ses identifiants, qui sont transmis à l'attaquant.


## Bourrage d'identifiants (*credential stuffing*)

Lors d'une violation de données, des listes de noms d'utilisateur et de mots de passe peuvent être divulguées.

Si un utilisateur réutilise le même mot de passe sur plusieurs services, un attaquant peut essayer les identifiants volés sur d'autres plateformes.

Cette technique est appelée **credential stuffing**.

L'utilisation de **mots de passe uniques** pour chaque service et de la **MFA** permet de réduire considérablement ce risque.

## Vol de session

Comme vu précédemment, une session permet à une application de reconnaître un utilisateur déjà authentifié.

Si un attaquant parvient à obtenir un **identifiant de session valide**, il peut potentiellement utiliser cette session pour agir au nom de la victime.

Ce type d'attaque est appelé **vol de session** (*session hijacking*).

La protection des communications avec **HTTPS**, la sécurisation des cookies et l'expiration des sessions sont parmi les mécanismes permettant de réduire ce risque.

## Fatigue MFA

La MFA augmente considérablement la sécurité d'un compte, mais elle ne rend pas l'utilisateur invulnérable.

Dans une attaque de **fatigue MFA** (*MFA fatigue*), l'attaquant dispose déjà du mot de passe de la victime et provoque de nombreuses demandes d'approbation MFA sur son téléphone.

L'objectif est de pousser l'utilisateur à accepter accidentellement une demande, simplement pour faire cesser les notifications.

Des mécanismes comme l'affichage d'un **code de correspondance** (*number matching*) et la sensibilisation des utilisateurs permettent notamment de réduire ce risque.

## Escalade de privilèges

Une fois qu'un attaquant a compromis un compte, il peut chercher à obtenir des permissions supplémentaires.

On parle alors d'**escalade de privilèges**.

Par exemple, un compte compromis possédant uniquement des droits de lecture pourrait être utilisé pour tenter d'obtenir des privilèges permettant de modifier des données ou d'administrer un serveur.

C'est notamment pour cette raison que le **principe du moindre privilège** est important : même lorsqu'un compte est compromis, les permissions disponibles pour l'attaquant restent limitées.

{{%notice style="info" title="Des attaques qui se complètent"%}}

Ces attaques peuvent être combinées.

Par exemple :

**Phishing → vol du mot de passe → connexion au compte → tentative de contournement de la MFA → vol de session → escalade de privilèges**

La sécurité des identités ne repose donc pas sur une seule mesure. Elle repose sur plusieurs couches de protection : mots de passe uniques, MFA, gestion des sessions, moindre privilège, RBAC, surveillance et sensibilisation des utilisateurs.

{{%/notice%}}