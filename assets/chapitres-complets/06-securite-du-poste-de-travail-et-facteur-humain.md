# 6. Sécurité du poste de travail et facteur humain
----------------

# 6.1. Comptes, permissions et menaces courantes
----------------

## Comptes et permissions

La gestion des comptes et des permissions a été abordée au chapitre 5. On retiendra notamment les principes suivants :

- **Principe du moindre privilège** : chaque utilisateur ne devrait disposer que des droits nécessaires à ses tâches.
- **Séparation des rôles (RBAC)** : les opérations administratives devraient être réalisées avec un compte adapté, distinct du compte utilisé pour les activités courantes.
- **Verrouillage de session** : une session inutilisée devrait être verrouillée afin d'empêcher une personne non autorisée d'accéder au poste.

Ces mesures limitent les conséquences d'une erreur humaine ou de la compromission d'un compte.

Par exemple, si un utilisateur travaille avec un compte standard plutôt qu'un compte administrateur, un logiciel malveillant exécuté sous ce compte dispose généralement de moins de permissions pour modifier le système.

## Menaces et vulnérabilités

Une **menace** est un élément susceptible de causer un dommage à un système informatique. Une **vulnérabilité** est une faiblesse qui peut être exploitée pour compromettre ce système.

Par exemple, un logiciel qui n'a pas reçu un correctif de sécurité peut présenter une vulnérabilité exploitable par un attaquant.

Les principales menaces visant les postes de travail sont les suivantes :

### Logiciels malveillants (*malware*)

Un logiciel malveillant est un programme conçu pour nuire à un système, voler des informations ou effectuer des actions non autorisées.

Les principales catégories sont :

- **Virus** : infecte des fichiers ou des programmes et peut se propager lorsqu'ils sont utilisés ou partagés.
- **Ver informatique (*worm*)** : se propage automatiquement entre des systèmes, notamment par le réseau.
- **Cheval de Troie (*Trojan horse*)** : se présente comme un programme légitime, mais dissimule des fonctionnalités malveillantes.
- **Rançongiciel (*ransomware*)** : chiffre des fichiers ou bloque l'accès à un système afin d'exiger une rançon.
- **Logiciel espion (*spyware*)** : collecte des informations sur l'utilisateur à son insu.

Un même logiciel malveillant peut combiner plusieurs de ces comportements. Par exemple, un rançongiciel peut voler des données avant de chiffrer les fichiers de sa victime.

{{% notice style="info" title="Comment un poste peut-il être infecté ?" %}}

Un logiciel malveillant peut exploiter une vulnérabilité technique, mais il peut également être exécuté à la suite d'une action de l'utilisateur.

Par exemple, un utilisateur peut télécharger un faux logiciel ou ouvrir une pièce jointe malveillante reçue par courriel.

{{% /notice %}}

### Keyloggers et vol d'informations

Un **keylogger** est un logiciel ou un dispositif matériel qui enregistre les frappes effectuées au clavier à l'insu de l'utilisateur.

Un attaquant peut ainsi chercher à récupérer des mots de passe, des messages confidentiels ou d'autres informations sensibles.

D'autres logiciels malveillants peuvent également capturer des écrans, surveiller le presse-papiers ou récupérer des fichiers.

### Autres vecteurs de compromission

Un poste peut également être compromis par :

- **Pièces jointes malveillantes** : documents ou programmes contenant du code dangereux.
- **Téléchargements douteux** : logiciels provenant de sources non fiables.
- **Supports amovibles inconnus** : clés USB ou disques externes contenant éventuellement des fichiers malveillants.
- **Vulnérabilités non corrigées** : failles dans un système d'exploitation ou une application.
- **Intrusions physiques** : accès à un poste laissé sans surveillance ou à des équipements insuffisamment protégés.

La sécurité d'un poste dépend donc de sa configuration, des logiciels utilisés et des comportements de ses utilisateurs.





# 6.2. Mesures de protection
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


# 6.3. Outils de surveillance et d'audit
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


## 6.4 - Facteur humain et ingénierie sociale

### Le facteur humain

Le **facteur humain** désigne l'influence des comportements, des décisions et des erreurs humaines sur la sécurité informatique.

Un utilisateur peut involontairement compromettre un système en ouvrant un fichier malveillant, en communiquant ses identifiants ou en laissant sa session ouverte.

Les attaquants cherchent donc parfois à manipuler les utilisateurs plutôt qu'à exploiter directement une vulnérabilité technique.

### Ingénierie sociale

L'**ingénierie sociale** consiste à exploiter la confiance, la politesse, la curiosité, la peur ou l'urgence perçue pour amener une personne à divulguer des informations sensibles ou à effectuer une action compromettante.

L'attaquant peut se faire passer pour un collègue, un gestionnaire, un technicien informatique ou un fournisseur.

Par exemple, une personne prétendant travailler au service informatique peut demander à un employé son mot de passe sous prétexte de résoudre un problème urgent.

### Principales techniques d'ingénierie sociale

- **Hameçonnage (*phishing*)** : message frauduleux incitant la victime à cliquer sur un lien, ouvrir une pièce jointe ou communiquer des informations.
- **Harponnage (*spear phishing*)** : hameçonnage ciblant une personne ou une organisation précise, à l'aide d'un message personnalisé.
- **Usurpation d'identité et prétexte (*pretexting*)** : attaquant qui invente un scénario pour se faire passer pour une personne de confiance et obtenir des informations ou un accès.
- **Appât (*baiting*)** : utilisation d'un élément attirant la curiosité de la victime, comme une clé USB inconnue.
- **Vishing et smishing** : hameçonnage par téléphone et par SMS.

Ces techniques exploitent souvent des réactions humaines normales. Un message peut être crédible et bien rédigé tout en étant frauduleux.

### Reconnaître une tentative d'ingénierie sociale

Certains indices doivent éveiller les soupçons :

- une demande inhabituelle ou urgente ;
- une demande de mot de passe ou de code MFA ;
- une adresse d'expéditeur ou un lien suspect ;
- une pièce jointe inattendue ;
- une demande de contourner une procédure habituelle.

Il ne faut pas se fier uniquement à l'apparence d'un message. L'identité de l'expéditeur et la légitimité de la demande doivent être vérifiées.

### Bonnes pratiques

Pour réduire les risques d'ingénierie sociale, les utilisateurs devraient :

1. Vérifier les demandes inhabituelles par un canal indépendant.
2. Ne jamais communiquer leur mot de passe ou un code MFA à une personne qui le demande.
3. Éviter d'ouvrir les liens et les pièces jointes suspects.
4. Ne pas connecter de supports amovibles inconnus.
5. Verrouiller leur poste lorsqu'ils s'en éloignent.
6. Signaler rapidement les messages suspects au service informatique.

### Que faire en cas de compromission suspectée ?

Si un utilisateur ouvre une pièce jointe suspecte ou communique ses identifiants par erreur, il doit rapidement prévenir le service informatique et suivre les procédures prévues.

Selon la situation, il peut être nécessaire d'isoler le poste du réseau, de modifier les identifiants compromis depuis un appareil fiable et de vérifier si d'autres comptes ont été affectés.

Il faut éviter de supprimer les fichiers ou les journaux qui pourraient aider à comprendre ce qui s'est passé.

### Sensibilisation et responsabilité partagée

La sensibilisation des utilisateurs constitue une mesure de sécurité à part entière. Elle permet de reconnaître les menaces, d'adopter de bonnes pratiques et de signaler rapidement les incidents.

Dans un **système de management de la sécurité de l'information (SMSI)**, la formation, les procédures et l'amélioration continue contribuent à intégrer la sécurité dans les activités quotidiennes.

{{% notice style="info" title="La sécurité repose sur plusieurs couches" %}}

Un incident peut résulter de plusieurs défaillances combinées : un message frauduleux, une erreur humaine, un poste insuffisamment protégé et une absence de surveillance.

La sécurité efficace repose sur une combinaison de protections techniques, de procédures organisationnelles et de comportements adaptés.

{{% /notice %}}


# 6.5 Laboratoires

# 6.5.1. Détecter l'ingénierie sociale

### Contexte

L'ingénierie sociale consiste à manipuler une personne afin de l'amener à divulguer des informations, à effectuer une action ou à contourner une procédure de sécurité.

Les attaques ne reposent pas toujours sur une vulnérabilité technique. Elles exploitent souvent la confiance, l'urgence, la peur, la curiosité ou l'autorité.

Vous devez analyser plusieurs situations et proposer des réactions appropriées.

### 1. Le courriel urgent

Vous recevez un courriel semblant provenir du service informatique de votre établissement. Le message indique que votre compte sera désactivé dans les 30 minutes si vous ne confirmez pas immédiatement vos identifiants.

Le message contient un lien vers une page de connexion dont l'adresse semble légèrement différente de celle du véritable établissement.

**Questions :**

1. Quels éléments du message devraient éveiller vos soupçons?
2. Quelle technique d'ingénierie sociale pourrait être utilisée?
3. Quels leviers psychologiques sont exploités?
4. Quelles actions devriez-vous entreprendre avant de répondre ou de cliquer sur le lien?

### 2. Le faux technicien

Une personne vous téléphone en affirmant qu'elle travaille au soutien informatique. Elle explique qu'une panne urgente affecte plusieurs employés et qu'elle a besoin de votre mot de passe pour rétablir votre accès.

Elle insiste sur le fait que la procédure habituelle peut être contournée dans cette situation exceptionnelle.

**Questions :**

1. Quels sont les indices suspects dans cette situation?
2. Pourquoi la prétendue fonction de la personne peut-elle influencer votre décision?
3. Comment pourriez-vous vérifier son identité et la légitimité de sa demande?
4. Quelle devrait être votre réaction?

### 3. La clé USB trouvée

En arrivant dans une salle informatique, vous trouvez une clé USB sans étiquette. Elle se trouve près d'un poste de travail et pourrait appartenir à un étudiant ou à un membre du personnel.

Vous êtes curieux de découvrir son contenu et envisagez de la brancher à votre ordinateur.

**Questions :**

1. Quels risques cette action pourrait-elle entraîner?
2. Pourquoi la curiosité constitue-t-elle un levier d'ingénierie sociale?
3. Quelle procédure serait la plus appropriée pour gérer cette clé USB?
4. Quelles mesures de prévention pourraient limiter les risques associés aux périphériques inconnus?

### 4. Le message du gestionnaire

Vous recevez un courriel semblant provenir de votre gestionnaire. Il vous demande de lui transmettre rapidement un document interne contenant des informations confidentielles. Il précise qu'il est en réunion et qu'il ne pourra pas répondre à vos questions.

Le nom affiché correspond à celui de votre gestionnaire, mais l'adresse d'expédition est inhabituelle.

**Questions :**

1. Quels éléments devraient vous inciter à vérifier la demande?
2. Quelles techniques de manipulation pourraient être utilisées?
3. Comment vérifier la demande sans utiliser les coordonnées fournies dans le message suspect?
4. Quelles conséquences pourraient découler de l'envoi du document sans vérification?

### Synthèse

Pour chacune des quatre situations, complétez le tableau suivant :

| Situation | Technique ou attaque possible | Indices suspects | Risque principal | Réaction recommandée |
|---|---|---|---|---|
| 1. Courriel urgent | | | | |
| 2. Faux technicien | | | | |
| 3. Clé USB trouvée | | | | |
| 4. Demande du gestionnaire | | | | |



# 6.5.2 Investigation
--------------

### Contexte
 
Le mardi 20 octobre 2026, vers 10 h, **Julie Tremblay**, technicienne au Service des finances du Cégep de Val-Laurent, appelle le soutien informatique. Elle explique que :
 
- elle a reçu ce matin un courriel inattendu du « service informatique » ;
- son ordinateur est devenu **très lent** depuis 9 h 45 environ ;
- son poste établit des **connexions réseau** qu'elle ne reconnaît pas.
Le technicien de garde a recueilli plusieurs éléments de preuve **sans éteindre le poste ni supprimer de fichiers**. Vous devez analyser ce dossier comme si vous participiez à la première intervention.
 
### Informations sur l'environnement
 
| Élément | Valeur |
|---|---|
| Utilisatrice | Julie Tremblay, compte `jtremblay` |
| Poste | `PT-ADM-0142`, Windows 11, adresse IP `10.20.3.42` |
| Droits de l'utilisatrice | Membre du groupe **Administrateurs** de son poste |
| Domaine officiel du cégep | `cegepvl.qc.ca` |
| Serveur de fichiers | `10.20.0.10` |
| Serveur d'inventaire | `10.20.0.15`. Le service informatique installe sur tous les postes un agent d'inventaire (`AgentInventaire.exe`) qui communique avec ce serveur. |
 
Toutes les heures du dossier sont en **heure locale**.
 
{{% notice style="note" title="Données fictives" %}}
Le cégep, les personnes, les noms de domaine et les adresses IP publiques de ce dossier sont fictifs.
{{% /notice %}}

### Matériel fourni
 
Téléchargez [le dossier d'investigation](https://github.com/cegepmv/420-313/raw/refs/heads/main/assets/files/06-kit-labo2.zip). Il contient :
 
| Fichier | Contenu |
|---|---|
| `00_LISEZMOI.txt` | Résumé du signalement et de l'environnement|
| `01_Courriel_suspect.txt` | Le courriel reçu et le témoignage de l'utilisatrice |
| `02_Extrait_journal_securite.csv` | Événements du journal Sécurité de Windows |
| `03_Liste_processus.txt` | Programmes en cours d'exécution (capture de 9 h 57) |
| `04_Connexions_reseau.csv` | Connexions réseau actives (capture de 9 h 58) |
| `05_Rapport_antivirus.txt` | Historique de l'antivirus |
 

### 1. Examiner les indices
 
Pour chaque fichier, répondez aux questions suivantes.
 
###### Le courriel
 
1. Comparez l'adresse de l'expéditeur avec le domaine officiel du cégep. Que remarquez-vous?
2. Le lien affiché et le lien réel mènent-ils au même endroit?
3. Quel est le vrai type de la pièce jointe? Pourquoi l'utilisatrice ne l'a-t-elle pas remarqué?
4. Quels leviers psychologiques le message utilise-t-il (urgence, peur, autorité, etc.)? Citez un passage pour chacun.
5. De quelle technique d'ingénierie sociale s'agit-il?

###### Le journal Windows
 
1. Les deux échecs de connexion de 8 h 44 sont-ils inquiétants? Justifiez.
2. Les connexions du compte `svc_inventaire` sont-elles normales? Appuyez-vous sur les informations de l'environnement.
3. Quels événements vous semblent anormaux? Que s'est-il passé exactement?
4. Le journal indique le compte `jtremblay` pour ces événements. Cela prouve-t-il que Julie les a faits elle-même?

###### Les processus
 
1. Quel processus vous semble suspect?
2. Comparez-le aux autres processus du même nom selon quatre critères : **emplacement**, **utilisateur**, **signature** et **utilisation du processeur**.
3. `Spotify.exe` est lui aussi installé dans le dossier `AppData`. Est-il suspect? Justifiez.
{{% notice style="warning" title="Attention" %}}
Un nom de processus connu ne garantit pas qu'il est légitime, et un programme inconnu n'est pas forcément malveillant. Basez-vous sur des critères vérifiables.
{{% /notice %}}
 
###### Les connexions réseau
 
1. Pour chaque connexion, indiquez le programme responsable et dites si elle vous semble normale.
2. Quelle connexion mérite une vérification? Quel lien faites-vous avec la partie 1.3?

###### Le rapport antivirus
 
1. Qu'est-ce que l'antivirus a détecté, à quelle heure et quelle action a-t-il effectuée?
2. L'analyse de 9 h 50 n'a rien trouvé. Le poste est-il sain pour autant? Justifiez avec les autres fichiers.
3. Relevez un élément de la configuration de l'antivirus qui pourrait expliquer ce manque.


### 2. Reconstituer la chronologie
 
À l'aide de **tous** les fichiers, placez les événements dans l'ordre chronologique.
 
| Heure | Ce qui s'est passé | Source (fichier) | Fait ou hypothèse? |
|---|---|---|---|
| 8 h 45 | | | |
| 9 h 12 | | | |
| ... | | | |
 
- Un **fait** est directement observé dans un fichier.
- Une **hypothèse** est une interprétation probable, mais non prouvée.
Répondez ensuite aux questions suivantes :
 
1. Quel événement est à l'origine de l'incident?
2. Quels événements se suivent de très près? Que suggère cet enchaînement?
3. Quelle information manque pour savoir si le mot de passe de Julie a été volé?


### 3. Évaluer l'incident
 
1. Le poste est-il compromis? Justifiez avec au moins **trois indices provenant de fichiers différents**.
2. Résumez en quelques phrases le scénario le plus probable.
3. Quels sont les principaux risques pour Julie et pour le cégep?
4. Quelles **faiblesses** du poste ou des habitudes de l'utilisatrice ont facilité l'attaque? Nommez-en au moins trois.

 
### 4. Proposer un plan d'intervention
 
Vous êtes responsable de la première intervention. Complétez le tableau en justifiant chaque action.
 
| Étape | Actions proposées | Justification |
|---|---|---|
| **1. Contenir** : empêcher l'attaque de continuer | | |
| **2. Préserver les preuves** : conserver ce qui aidera à comprendre | | |
| **3. Investiguer** : vérifier ce qu'on ne sait pas encore | | |
| **4. Rétablir** : retrouver un poste fiable | | |
| **5. Prévenir** : éviter que cela se reproduise | | |

# 6.5.3 Solution de l'investigation
--------------

Ce solutionnaire présente une réponse très détaillée. D'autres sont acceptables si elles s'appuient sur des **indices vérifiables** du dossier et distinguent bien les **faits** des **hypothèses**.

##### 1. Examiner les indices

**Le courriel (`01_Courriel_suspect.txt`)**

1. **Adresse de l'expéditeur :** 
    + Le courriel provient de `securite@cegepvl-soutien.com`, alors que le domaine officiel est `cegepvl.qc.ca`. 
    + Le domaine est **différent** : il reprend le nom du cégep pour paraître légitime, mais il appartient à n'importe qui (`.com` au lieu de `.qc.ca`). 
    + Le nom affiché « Service informatique » ne prouve rien : n'importe qui peut choisir ce nom.

2. **Lien :** Non. Le texte affiché est `https://portail.cegepvl.qc.ca/securite`, mais le lien réel mène à `https://cegepvl-soutien.com/verification`, soit le **même domaine frauduleux** que l'expéditeur. Il s'agit très probablement d'une fausse page de connexion conçue pour voler le mot de passe.

3. **Pièce jointe :** Le fichier `Avis_mise_a_jour.pdf.exe` est une **application (exécutable)** et non un document PDF. Il utilise une **double extension**. Comme Windows **masque les extensions connues** sur ce poste, l'utilisatrice voyait seulement `Avis_mise_a_jour.pdf`, accompagné d'une icône de PDF (un exécutable peut afficher l'icône de son choix). Le fichier est aussi placé dans une archive `.zip`, ce qui aide souvent à contourner les filtres de courriel.

4. **Leviers psychologiques.**

   | Levier | Passage du message |
   |---|---|
   | Urgence | *"URGENT – Mise à jour de sécurité obligatoire avant 10 h"* |
   | Peur | *"votre compte sera DÉSACTIVÉ et vous perdrez l'accès à vos courriels et à vos fichiers"* |
   | Autorité | Signature *"Le Service informatique"* », mention d'une *"faille de sécurité critique"* |
   | Empêcher la vérification | *"Inutile d'appeler le centre d'assistance, il est débordé ce matin"* |

   Le dernier passage est particulièrement révélateur : il décourage la victime de **vérifier la demande par un canal indépendant**, ce qui est justement le bon réflexe.
5. **Technique.** Il s'agit d'**hameçonnage (*phishing*)** avec **usurpation d'identité** du service informatique. Le message combine deux vecteurs : une **pièce jointe malveillante** et un **lien de vol d'identifiants**. On peut aussi parler de **harponnage (*spear phishing*)** visant l'organisation, puisque le message imite précisément le cégep (nom de domaine ressemblant, service interne), même s'il n'est pas personnalisé pour Julie.

**Le journal Windows (`02_Extrait_journal_securite.csv`)**

1. **Échecs de 8 h 44 :** Non, ils ne sont pas inquiétants. Il y a seulement **deux** échecs, à 11 secondes d'intervalle, suivis d'une **ouverture de session réussie au clavier du poste** 16 secondes plus tard, à l'heure d'arrivée au travail. C'est typique d'une faute de frappe dans le mot de passe. Une attaque par force brute produirait de nombreux échecs rapprochés, souvent par le réseau. De plus, ces événements ont lieu **avant** la réception du courriel.
2. **Compte `svc_inventaire`:** Oui, ces connexions sont normales. Ce compte de service correspond à l'agent d'inventaire installé par le service informatique. Les connexions proviennent du **serveur d'inventaire `10.20.0.15`**, comme prévu, et reviennent à intervalle régulier (9 h 02 et 9 h 31), **avant et après** l'incident.
3. **Événements anormaux :**
   - 9 h 23 min 05 s (ID **4720**) : création d'un compte local nommé `support_it` ;
   - 9 h 23 min 07 s (ID **4732**) : ajout de `support_it` au groupe **Administrateurs**.

   Un compte administrateur a été créé, puis élevé, en **2 secondes**, moins de 2 minutes après l'exécution de la pièce jointe. Le nom `support_it` est choisi pour passer inaperçu : même si le logiciel malveillant est supprimé ou si Julie change son mot de passe, l'attaquant conserve un accès administrateur au poste.
4. **Le compte `jtremblay` est indiqué :** Non, cela ne prouve pas que Julie a fait ces actions elle-même. Le journal indique le **compte** sous lequel l'action s'est exécutée, pas la personne devant l'écran. Un programme lancé par Julie s'exécute avec ses droits ; comme elle est **administratrice** et qu'elle a accepté la demande d'autorisation, le logiciel malveillant disposait des droits nécessaires pour créer un compte. 

**Les processus (`03_Liste_processus.txt`)**

1. **Processus suspect :** `svchost.exe`, **PID 6612**.
2. **Comparaison avec les autres `svchost.exe` :**

   | Critère | Vrais `svchost.exe` (PID 892, 1012, 1488) | `svchost.exe` (PID 6612) |
   |---|---|---|
   | Emplacement | `C:\Windows\System32\` | `C:\Users\jtremblay\AppData\Roaming\Microsoft\` |
   | Utilisateur | Comptes système (SYSTEM, SERVICE RÉSEAU, SERVICE LOCAL) | `jtremblay` (compte d'une personne) |
   | Signature | Microsoft Windows | **Non signé** |
   | Processeur | 0,1 % à 0,3 % | **62,3 %** |

   S'ajoute un cinquième indice : il a démarré à **9 h 21 min 39 s**, juste après l'ouverture de la pièce jointe. Le programme **usurpe le nom** d'un processus système légitime pour se fondre dans la liste. Son utilisation élevée du processeur explique probablement la lenteur signalée.
3. **`Spotify.exe`.** Non, il n'est pas suspect. Spotify s'installe **normalement** dans `AppData\Roaming\Spotify` (installation par utilisateur). Son nom correspond à son emplacement, il est **signé par Spotify AB** et il utilise peu le processeur (0,4 %). L'emplacement `AppData` n'est donc pas, à lui seul, un critère suffisant. 
  {{%notice style="note" title="À se demander"%}}
  On pourrait se demander si ce logiciel est **autorisé** par la politique du cégep sur un poste des finances, mais c'est une question de conformité, pas un indice de compromission.
  {{%/notice%}}

**Les connexions réseau (`04_Connexions_reseau.csv`)**

1. **Analyse de chaque connexion.**

   | Programme | Destination | Normale? | Justification |
   |---|---|---|---|
   | `AgentInventaire.exe` | `10.20.0.15:9525` | Oui | Serveur d'inventaire interne prévu dans l'environnement |
   | `OUTLOOK.EXE` | `198.51.100.20:443` | Oui | Messagerie en HTTPS |
   | `msedge.exe` | `198.51.100.35:443` | Probablement | Navigation HTTPS ordinaire, mais l'adresse seule ne dit pas **quel site** est consulté (à vérifier, voir partie 2, question 3) |
   | `Spotify.exe` | `198.51.100.120:4070` | Oui | Programme légitime et signé ; le port 4070 est utilisé par Spotify |
   | `MsMpEng.exe` | `198.51.100.140:443` | Oui | Antivirus Defender |
   | `svchost.exe (6612)` | `203.0.113.45:8443` | **Non** | Faux processus, adresse publique inconnue, port inhabituel |
   | `System (4)` | `10.20.0.10:445` | Oui | Partage de fichiers (SMB) vers le serveur de fichiers interne |

2. **Connexion à vérifier :** La connexion du **faux `svchost.exe` (PID 6612)** vers `203.0.113.45:8443`. C'est **le même processus** que celui repéré à la partie 1.3 : un programme non signé, lancé depuis `AppData`, qui communique avec un serveur externe inconnu. Il s'agit probablement d'un **serveur de commande et de contrôle (C&C)** permettant à l'attaquant de piloter le poste, ou d'un canal d'**exfiltration de données**.
  {{%notice title="À noter" style="info"%}}
  La connexion vers le serveur de fichiers est normale en soi, mais elle montre que le poste (et donc le logiciel malveillant, qui a les droits de Julie) **a accès aux fichiers partagés** du cégep.
  {{%/notice%}}

**Le rapport antivirus (`05_Rapport_antivirus.txt`)**

1. **Détection.** À **9 h 21 min 41 s**, Defender a détecté `Trojan:Win32/Dropper` (comportement suspect) dans `C:\Users\jtremblay\AppData\Local\Temp\Avis_mise_a_jour.pdf.exe`, soit la pièce jointe extraite de l'archive. Il l'a **mise en quarantaine** avec succès.
2. **Analyse de 9 h 50.** Non, le poste n'est pas sain. Le faux `svchost.exe` a démarré à **9 h 21 min 39 s**, soit **2 secondes avant** la mise en quarantaine : Le logiciel malveillant était déjà exécuté et actif. La preuve en est qu'à 9 h 57 et 9 h 58, le faux `svchost.exe` est toujours en cours d'exécution et connecté à `203.0.113.45`, et que le compte `support_it` a été créé à 9 h 23. L'absence d'alerte ne prouve pas l'absence de menace.
3. **Configuration.** Les **signatures n'ont pas été mises à jour depuis 4 jours** (16 octobre). Une charge récente ou inconnue peut échapper à la détection par signature. On peut aussi mentionner qu'une **analyse rapide** ne vérifie que certains emplacements et ne remplace pas une analyse complète.

---

##### 2. Reconstituer la chronologie

| Heure | Ce qui s'est passé | Source (fichier) | Fait ou hypothèse? |
|---|---|---|---|
| 8 h 44 – 8 h 45 | Deux échecs d'ouverture de session, puis ouverture réussie au clavier (faute de frappe probable) | 02 | Fait (l'explication « faute de frappe » est une hypothèse) |
| 9 h 02 | Connexion réseau du compte `svc_inventaire` depuis le serveur d'inventaire (normal) | 02 | Fait |
| 9 h 12 | Réception du courriel frauduleux avec `Avis_mise_a_jour.zip` | 01 | Fait |
| ≈ 9 h 20 | Julie ouvre le « PDF » ; rien ne s'affiche ; elle accepte la demande d'autorisation Windows (UAC) | 01 (témoignage) | Fait déclaré (heure approximative) |
| 9 h 21 min 39 s | Démarrage du faux `svchost.exe` (PID 6612) dans `AppData\Roaming\Microsoft` | 03 | Fait |
| 9 h 21 min 41 s | Defender détecte `Avis_mise_a_jour.pdf.exe` et le met en quarantaine | 05 | Fait |
| — | `Avis_mise_a_jour.pdf.exe` a installé et lancé le faux `svchost.exe` avant d'être bloqué | 03 + 05 | Hypothèse |
| 9 h 23 min 05 s | Création du compte local `support_it` sous le compte `jtremblay` | 02 | Fait |
| 9 h 23 min 07 s | Ajout de `support_it` au groupe Administrateurs | 02 | Fait |
| — | Ces deux actions ont été effectuées automatiquement par le logiciel malveillant (porte dérobée) | 02 + 03 | Hypothèse |
| 9 h 31 | Nouvelle connexion de `svc_inventaire` (normal) | 02 | Fait |
| ≈ 9 h 45 | Le poste devient très lent | 01 (témoignage) | Fait déclaré |
| — | La lenteur est causée par le faux `svchost.exe` (62,3 % du processeur) | 01 + 03 | Hypothèse |
| 9 h 50 | Analyse rapide lancée par Julie : aucune menace détectée | 05 | Fait |
| 9 h 57 | Le faux `svchost.exe` est toujours actif (62,3 % du processeur) | 03 | Fait |
| 9 h 58 | Connexion établie du faux `svchost.exe` vers `203.0.113.45:8443` | 04 | Fait |
| — | `203.0.113.45` est un serveur contrôlé par l'attaquant (C2 ou exfiltration) | 04 | Hypothèse |
| ≈ 10 h | Julie appelle le soutien informatique (témoignage noté à 10 h 02) | 00, 01 | Fait |
| ? | Julie a-t-elle cliqué sur le lien et saisi son mot de passe? | 01 | **Inconnu** |

Le témoignage de Julie est un **fait déclaré** : on sait qu'elle l'a dit, mais ses heures sont approximatives et ses souvenirs peuvent être incomplets. Il est donc préférable de le confirmer par les autres fichiers (ce qui est le cas : 9 h 20 concorde avec 9 h 21 min 39 s).

**Questions**

1. **Origine :** L'**exécution de la pièce jointe** `Avis_mise_a_jour.pdf.exe` vers 9 h 20, avec **acceptation de la demande d'autorisation** (UAC), ce qui a donné au programme les droits administrateur. La réception du courriel (9 h 12) en est le point de départ, mais c'est l'exécution qui déclenche la compromission.
2. **Enchaînement rapproché :** 9 h 21 min 39 s (faux `svchost.exe` démarre) → 9 h 21 min 41 s (mise en quarantaine) → 9 h 23 min 05 s et 07 s (création de `support_it` et ajout aux Administrateurs). Tout s'est fait en moins de 2 minutes. Cet enchaînement suggère une **attaque automatisée**, et montre que l'antivirus a réagi **trop tard** : il a mis en quarantaine, mais le processus était déjà actif.
3. **Information manquante :** On ne sait pas si Julie a **cliqué sur le lien** et **saisi son mot de passe** sur `cegepvl-soutien.com`. Il faudrait consulter l'**historique du navigateur**, les **journaux DNS** (visite du domaine frauduleux?) et les **journaux de connexion du compte de Julie** (par exemple Microsoft 365) pour repérer des connexions depuis des adresses inhabituelles. Même si elle n'a rien saisi, son mot de passe doit être considéré comme **potentiellement compromis**, puisqu'un logiciel malveillant s'exécute dans sa session (il pourrait par exemple enregistrer ses frappes au clavier).

---

##### 3. Évaluer l'incident

1. **Le poste est-il compromis?** **Oui.** Indices provenant de fichiers différents :
   - **01** : un exécutable déguisé en PDF a été ouvert et l'autorisation administrateur a été accordée ;
   - **02** : un compte administrateur `support_it` a été créé moins de 2 minutes plus tard ;
   - **03** : un faux `svchost.exe` non signé, lancé depuis `AppData`, utilise 62 % du processeur et a démarré à 9 h 21 ;
   - **04** : ce même processus maintient une connexion vers une adresse externe inconnue (`203.0.113.45:8443`) ;
   - **05** : Defender a détecté l'exécution dangereuse de `Avis_mise_a_jour.pdf.exe` et l'a mis en quarantaine.
2. **Scénario le plus probable.** Julie a reçu un courriel d'hameçonnage usurpant le service informatique. Croyant ouvrir un PDF, elle a lancé un exécutable et lui a accordé les droits administrateur. L'exécutable a installé un logiciel malveillant déguisé en `svchost.exe` dans son profil, juste avant que l'antivirus ne le mette en quarantaine. Le logiciel malveillant a ensuite créé un compte administrateur caché (`support_it`) pour conserver l'accès, puis établi une connexion vers un serveur externe contrôlé par l'attaquant. Il était toujours actif au moment de l'intervention.
3. **Principaux risques.**
   - **Pour Julie** : vol de ses identifiants (et de tout autre compte utilisant le même mot de passe) ; usurpation de son compte de courriel pour tromper ses collègues ou des fournisseurs ; atteinte à ses renseignements personnels.
   - **Pour le cégep** :
     - **Confidentialité** : exfiltration de données financières et de renseignements personnels (employés, étudiants, fournisseurs), accessibles depuis le poste ou le serveur de fichiers. 
     {{%notice style="tip" title="À prendre en compte"%}}
     Un tel incident pourrait constituer un **incident de confidentialité** au sens de la **Loi 25**, avec l'obligation de l'évaluer, de l'inscrire au registre et, en cas de risque de préjudice sérieux, d'aviser la Commission d'accès à l'information et les personnes concernées.
     {{%/notice%}}
     - **Intégrité et fraude** : le Service des finances est une cible de choix pour la fraude (fausses factures, détournement de paiements depuis un compte légitime).
     - **Propagation** : l'attaquant dispose d'un accès administrateur persistant et pourrait se déplacer vers d'autres postes ou vers le serveur de fichiers.
     - **Disponibilité** : risque de rançongiciel sur les partages réseau ; lenteur actuelle du poste.
     - **Réputation** et coûts de la remise en état.
4. **Faiblesses ayant facilité l'attaque.**
   - **Droits administrateur** pour l'utilisatrice (non-respect du moindre privilège) : un simple clic sur « Oui » a suffi pour créer un compte administrateur.
   - **Extensions de fichiers masquées** : la double extension `.pdf.exe` est passée inaperçue.
   - **Signatures antivirus non à jour** depuis 4 jours.
   - **Filtrage de courriel insuffisant** : une archive contenant un `.exe`, envoyée depuis un domaine imitant celui du cégep, a atteint la boîte de réception.
   - **Manque de sensibilisation** : l'utilisatrice a suivi les instructions sans vérifier la demande par un canal indépendant, malgré plusieurs signaux d'alerte.
   - **Absence de surveillance centralisée** : la détection de 9 h 21 et la création d'un compte administrateur à 9 h 23 n'ont déclenché **aucune réaction** du service informatique ; c'est l'utilisatrice qui a signalé le problème 40 minutes plus tard.

---

##### 4. Proposer un plan d'intervention

| Étape | Actions proposées | Justification |
|---|---|---|
| **1. Contenir** | Isoler le poste du réseau (débrancher le câble, couper le Wi-Fi ou isoler le poste avec l'EDR) **sans l'éteindre**. Bloquer `203.0.113.45` et le domaine `cegepvl-soutien.com` au pare-feu. **Désactiver** (sans supprimer) le compte `support_it`. Réinitialiser le mot de passe de Julie **depuis un autre appareil fiable** et fermer ses sessions actives. Retirer le courriel des autres boîtes de réception et avertir le personnel. | Couper la communication avec l'attaquant et empêcher l'exfiltration ou la propagation vers le serveur de fichiers. Éteindre le poste effacerait la mémoire vive (processus, connexions). Changer le mot de passe depuis le poste compromis pourrait permettre au logiciel malveillant de capturer le nouveau. Désactiver plutôt que supprimer conserve les traces. |
| **2. Préserver les preuves** | Exporter les journaux Windows (Sécurité, Système, Defender). Conserver le courriel original avec ses en-têtes, le fichier en quarantaine et le faux `svchost.exe`. Faire une image du disque. Documenter chaque action (qui, quoi, quand). | Ces éléments permettent de comprendre l'attaque, d'en mesurer l'ampleur et de rechercher d'autres victimes. La documentation est nécessaire si l'incident a des suites légales ou doit être déclaré. |
| **3. Investiguer** | Vérifier si Julie a visité le faux portail et saisi son mot de passe (historique du navigateur, journaux DNS, connexions inhabituelles à son compte). Rechercher les autres destinataires du courriel et les autres postes communiquant avec `203.0.113.45`. Vérifier si `support_it` a été utilisé. Mesurer le volume de données envoyées à `203.0.113.45` et les fichiers consultés sur `10.20.0.10`. Chercher d'autres mécanismes de persistance (tâches planifiées, services, clés de démarrage). Évaluer si des renseignements personnels ont été compromis. | Déterminer ce qui a été volé, si d'autres postes ou comptes sont touchés et quelles obligations légales s'appliquent. Il ne faut pas supposer que l'incident se limite à ce poste. |
| **4. Rétablir** | **Réinstaller le poste** à partir d'une image fiable plutôt que de tenter de le « nettoyer ». Supprimer le compte `support_it` (une fois les preuves conservées). Restaurer les fichiers de Julie à partir d'une **sauvegarde antérieure à 9 h 20**, vérifiée saine. Appliquer toutes les mises à jour (système et signatures). Changer les mots de passe de tous les comptes utilisés sur le poste. Surveiller le poste de près après sa remise en service. | Un attaquant ayant eu des droits administrateur a pu modifier le système de façon difficile à détecter : on ne peut plus faire confiance au poste. La sauvegarde garantit qu'on ne restaure pas des fichiers compromis. |
| **5. Prévenir** | Retirer Julie du groupe Administrateurs (compte standard ; compte administrateur distinct géré par le service informatique). **Afficher les extensions** de fichiers. Automatiser la mise à jour des signatures. Renforcer le filtrage de courriel (bloquer les exécutables, même dans une archive ; bannière « expéditeur externe » ; détection des domaines ressemblants). Activer l'**authentification multifacteur**.  Former le personnel et rappeler la procédure : le service informatique ne demande jamais un mot de passe, et toute demande inhabituelle se vérifie par un canal indépendant. | Chaque mesure corrige une faiblesse relevée à la partie 3.4. Combinées, elles appliquent le principe de **défense en profondeur** : si une mesure échoue (par exemple, l'utilisateur clique), les autres limitent les conséquences. La MFA rend un mot de passe volé beaucoup moins utile. |

{{%notice style="tip" title="À retenir"%}}
**À retenir** : l'ordre des actions compte. On **contient** d'abord, on **préserve les preuves** avant toute action irréversible (éteindre, supprimer des fichiers ou des comptes), et on change un mot de passe compromis **depuis un appareil fiable**.
{{%/notice%}}
