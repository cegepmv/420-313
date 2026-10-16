+++
title = 'Mesures de protection'
weight = '620'
draft = false
+++
--------------------

Plusieurs mécanismes complémentaires permettent de protéger un poste de travail. Aucun ne garantit à lui seul une protection complète.

### Antivirus et protection des terminaux

Un **antivirus** détecte, bloque et supprime des logiciels malveillants.

Il peut notamment reconnaître des menaces connues grâce à leurs signatures ou repérer des comportements suspects.

Les solutions modernes combinent généralement plusieurs méthodes de détection. Elles doivent être maintenues à jour pour rester efficaces.

#### EDR
Un **EDR** (*Endpoint Detection and Response*) est une solution plus complète qui surveille les activités d'un poste et aide à détecter et à investiguer les comportements suspects. Il peut notamment permettre d'isoler un poste compromis du réseau.


{{%notice style="tip" title="Antivirus vs. EDR"%}}
+ **Antivirus :** cherche principalement à détecter et bloquer les menaces
+ **EDR :** offre aussi des capacités de surveillance et d'investigation.
{{%/notice%}}

{{% notice style="warning" title="Les limites des protections antivirus" %}}

Un antivirus peut manquer une menace inconnue ou ne pas détecter immédiatement un comportement malveillant. L'absence d'alerte ne garantit donc pas qu'un poste est sain.

Il faut combiner plusieurs mesures de sécurité.

{{% /notice %}}

### Pare-feu personnel

Un **pare-feu (*firewall*)** contrôle les communications réseau selon des règles définies.

On distingue notamment :

- **Trafic entrant** : communications provenant d'autres systèmes vers le poste.
- **Trafic sortant** : communications initiées par le poste vers d'autres systèmes.

Par exemple, un pare-feu peut bloquer les connexions entrantes non autorisées tout en permettant les communications nécessaires au fonctionnement des applications.

Il peut également limiter certaines communications sortantes suspectes.

Un pare-feu ne garantit cependant pas que toutes les communications autorisées sont légitimes ou inoffensives.

### Mises à jour et correctifs de sécurité

Les systèmes d'exploitation et les applications peuvent contenir des vulnérabilités permettant à un attaquant d'exécuter du code ou d'accéder à des données.

Un **correctif de sécurité (*security patch*)** est une modification qui corrige une vulnérabilité ou une faiblesse de sécurité.

Il est important de maintenir à jour :

- le système d'exploitation ;
- les navigateurs Web ;
- les logiciels bureautiques ;
- les applications installées.

Les mises à jour de sécurité doivent être appliquées régulièrement, en accordant une priorité particulière aux vulnérabilités connues pour être exploitées.

### Sauvegardes et protection contre les rançongiciels

Une **sauvegarde (*backup*)** est une copie de données permettant de les récupérer après une perte, une panne ou une attaque.

Les sauvegardes peuvent protéger contre :

- la suppression accidentelle de fichiers ;
- une panne matérielle ;
- la corruption de données ;
- un rançongiciel ;
- la perte ou le vol d'un ordinateur.

On distingue plusieurs types de sauvegardes :

- **Complète** : copie l'ensemble des données sélectionnées.
- **Incrémentielle** : copie les modifications depuis la dernière sauvegarde.
- **Différentielle** : copie les modifications depuis la dernière sauvegarde complète.

#### Règle 3-2-1

La règle **3-2-1** recommande de conserver :

- **3 copies** des données, en comptant les données originales ;
- sur **2 types de supports** différents ;
- dont **1 copie hors site**, dans un emplacement distinct.

Pour mieux se protéger contre les rançongiciels, il est également utile de conserver une copie hors ligne ou immuable, difficile à supprimer ou à modifier par un attaquant.

Il faut enfin tester régulièrement la restauration des données : une sauvegarde n'est utile que si elle peut effectivement être récupérée.

{{% notice style="tip" title="Attention aux sauvegardes accessibles" %}}

Si un rançongiciel peut accéder aux sauvegardes avec les mêmes permissions que les fichiers originaux, il peut aussi les chiffrer ou les supprimer.

Les sauvegardes doivent donc être protégées par des permissions adaptées et, idéalement, par une copie isolée du système principal.

{{% /notice %}}

### Chiffrement et sécurité physique

Le **chiffrement du disque** protège les données contre certains accès non autorisés, notamment en cas de perte ou de vol d'un ordinateur.

Des technologies comme BitLocker sous Windows et LUKS sous Linux permettent de chiffrer des volumes de stockage.

Le chiffrement ne remplace toutefois pas les autres mesures de protection : un logiciel malveillant peut toujours accéder aux fichiers auxquels l'utilisateur a accès lorsque sa session est ouverte.

La sécurité physique est également importante. Quelques mesures simples permettent de réduire les risques :

- verrouiller sa session lorsqu'on s'éloigne de son poste ;
- protéger physiquement les ordinateurs portables ;
- ne pas laisser des documents confidentiels exposés ;
- éviter de connecter des supports amovibles inconnus.

### Défense en profondeur

La **défense en profondeur** consiste à combiner plusieurs mesures de sécurité afin qu'une défaillance ne suffise pas à compromettre l'ensemble du système.

Par exemple, la protection d'un poste peut reposer sur :

1. un compte utilisateur sans privilèges administratifs ;
2. des logiciels à jour ;
3. un antivirus ;
4. un pare-feu ;
5. des sauvegardes protégées ;
6. la surveillance des événements de sécurité.

Si une mesure échoue, les autres peuvent encore empêcher l'attaque ou en limiter les conséquences.