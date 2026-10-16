+++
title = 'Comptes et permissions'
weight = '550'
draft = false
+++
-------------

Une fois l'utilisateur authentifié, le système doit déterminer **quelles ressources et quelles actions lui sont accessibles**.

La gestion des permissions repose notamment sur deux principes importants :

- le **principe du moindre privilège**;
- le **contrôle d'accès basé sur les rôles (RBAC)**.

### Principe du moindre privilège

Le **principe du moindre privilège** (*Principle of Least Privilege*) consiste à donner à chaque utilisateur, programme ou processus **uniquement les droits nécessaires à l'exécution de ses tâches**.

Un utilisateur ne devrait donc pas posséder des privilèges administratifs simplement parce qu'ils pourraient être utiles occasionnellement.

###### Exemple : poste informatique d'un cégep

Un enseignant utilise quotidiennement son ordinateur pour :

- préparer ses cours ;
- consulter ses courriels ;
- utiliser des logiciels pédagogiques ;
- accéder aux ressources du réseau.

Il n'a cependant pas besoin de pouvoir :

- modifier les comptes des autres utilisateurs ;
- installer n'importe quel logiciel sur tous les ordinateurs du cégep ;
- modifier la configuration des serveurs ;
- accéder aux dossiers confidentiels du service des ressources humaines.

Ces permissions devraient être réservées aux personnes qui en ont réellement besoin.

Ainsi, même si le compte de l'enseignant est compromis, les possibilités offertes à l'attaquant sont limitées.

{{%notice style="tip" title="Le moindre privilège s'applique aussi aux programmes"%}}

Le principe du moindre privilège ne concerne pas uniquement les utilisateurs.

Un programme ou un service devrait lui aussi fonctionner avec **le minimum de permissions nécessaires**.

Par exemple, une application Web qui doit seulement lire des données dans une base de données ne devrait pas nécessairement disposer des permissions permettant de supprimer ou de modifier toutes les données.
{{%/notice%}}

### RBAC : contrôle d'accès basé sur les rôles

Le **RBAC** (*Role-Based Access Control*) consiste à attribuer des permissions à des **rôles**, puis à attribuer ces rôles aux utilisateurs.

Plutôt que de gérer individuellement les permissions de chaque utilisateur, on définit des rôles correspondant aux responsabilités de l'organisation.

###### Exemple : système informatique d'un cégep

Imaginons un système permettant de gérer les informations scolaires.

On pourrait définir les rôles suivants :

|Rôle|	Permissions|
|----|-------------|
|**Étudiant**|	Consulter ses cours, remettre des travaux, consulter ses notes|
|**Enseignant**|	Consulter ses groupes, créer des évaluations, saisir les notes|
|**Technicien informatique**|	Gérer les comptes, administrer les postes et certains services|
|**Administrateur système**	|Administrer les serveurs et l'ensemble de l'infrastructure|

Un utilisateur peut ensuite recevoir un ou plusieurs rôles selon ses responsabilités.

Par exemple, lorsqu'un nouvel enseignant est embauché, on lui attribue le rôle **Enseignant**. Il reçoit automatiquement les permissions associées à ce rôle.

Lorsqu'un enseignant quitte l'établissement, son compte ou son rôle peut être désactivé sans devoir rechercher manuellement toutes les permissions qui lui avaient été accordées.

{{%notice style="info" title="RBAC et moindre privilège"%}}

Le **RBAC** et le **principe du moindre privilège** sont complémentaires :

+ RBAC permet de structurer et de gérer les permissions à l'aide de rôles.
+ Le moindre privilège permet de s'assurer que ces rôles ne donnent **pas plus de permissions que nécessaire**.

{{%/notice%}}
