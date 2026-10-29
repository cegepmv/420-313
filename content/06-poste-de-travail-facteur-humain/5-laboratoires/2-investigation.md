+++
pre = '2- '
title = "Investigation"
weight = '652'
draft = false
+++
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

<!-- {{% notice style="tip" title="Rappel : identifiants d'événements Windows" %}}
| ID | Événement |
|---|---|
| 4624 | Ouverture de session réussie |
| 4625 | Échec d'ouverture de session |
| 4720 | Création d'un compte utilisateur |
| 4732 | Ajout d'un membre à un groupe local de sécurité |
{{% /notice %}} -->
 

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
 
<!-- {{% notice style="warning" title="Ordre des actions" %}}
Évitez les actions irréversibles (éteindre le poste, supprimer des fichiers ou des comptes) avant d'avoir conservé les preuves. Pensez aussi à l'appareil utilisé pour changer un mot de passe compromis.
{{% /notice %}} -->
