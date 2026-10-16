+++
title = "Outils de surveillance et d'audit"
weight = '630'
draft = false
+++
--------------------

Les mécanismes de protection doivent être accompagnés d'outils permettant d'observer les activités d'un système et de repérer des comportements suspects.

### Journaux d'événements (*logs*)

Un **journal d'événements** enregistre des informations sur les activités d'un système : connexions, erreurs, modifications de comptes, installations de logiciels ou alertes de sécurité.

Ces journaux permettent notamment de répondre aux questions suivantes :

- Que s'est-il passé ?
- À quel moment ?
- Quel compte ou quel processus est concerné ?
- Quelles actions ont précédé ou suivi l'événement ?

#### Exemple : journaux de sécurité Windows

Sous Windows, l'Observateur d'événements permet de consulter les événements enregistrés par le système.

Quelques identifiants utiles sont :

| Identifiant | Événement |
|---|---|
| 4624 | Ouverture de session réussie |
| 4625 | Échec d'ouverture de session |
| 4720 | Création d'un compte utilisateur |
| 4732 | Ajout d'un membre à un groupe local de sécurité |

Par exemple, plusieurs échecs d'ouverture de session successifs peuvent indiquer une tentative d'accès non autorisé.

La disponibilité des événements dépend de la configuration de l'audit sur la machine.

### Surveillance des processus et alertes

Les outils de surveillance peuvent aussi observer les programmes en cours d'exécution et certaines communications réseau.

Des comportements inhabituels peuvent déclencher des alertes, par exemple :

- l'exécution d'un programme suspect ;
- un grand nombre de fichiers modifiés en peu de temps ;
- des connexions réseau inhabituelles ;
- des modifications inattendues des paramètres de sécurité.

Une alerte ne constitue pas nécessairement une preuve d'attaque. Elle doit être analysée en tenant compte du contexte.

### Audit et investigation

Un **audit de sécurité** consiste à examiner l'état d'un système et les informations disponibles pour vérifier sa sécurité ou comprendre un incident.

Par exemple, après une suspicion de compromission, un technicien peut consulter les journaux, examiner les processus et vérifier les connexions réseau pour déterminer ce qui s'est passé.

Les journaux doivent être correctement configurés et protégés. S'ils sont incomplets ou supprimés, il peut devenir difficile de reconstituer un incident.


{{%notice style="info" title="SIEM"%}}
Dans les grandes organisations, des outils comme les **SIEM** (*Security Information and Event Management*) permettent de centraliser les journaux de plusieurs systèmes et de rapprocher des événements pour repérer des activités suspectes.

Exemples de SIEM très utilisés dans l'industrie : 
+ Splunk Enterprise 
+ IBM QRadar
+ Wazuh (alternative *open-source*)

Nous n'allons malheureusement pas étudier le fonctionnement détaillé d'un SIEM dans ce cours (en dehors du scope).
{{%/notice%}}