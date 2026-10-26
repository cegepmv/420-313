+++
pre = '2- '
title = "Investigation"
weight = '652'
draft = false
+++
--------------

### Contexte

Un membre du personnel signale que son poste de travail se comporte de manière inhabituelle. Il indique avoir reçu un courriel inattendu et remarque depuis quelques minutes des ralentissements ainsi que des connexions réseau qu'il ne reconnaît pas.

L'équipe de soutien souhaite déterminer ce qui a pu se produire et quelles mesures devraient être prises.

On vous remet un dossier d'investigation contenant plusieurs éléments de preuve. Vous devez les analyser comme si vous participiez à une première intervention en sécurité informatique.

### Matériel fourni

Téléchargez [le dossier fourni](https://github.com/cegepmv/420-313/raw/refs/heads/main/assets/files/06-kit-labo2.zip). Il comprend les éléments suivants :

- `01_Courriel_suspect.txt`: Un courriel suspect reçu par l'utilisateur.
- `02_Extrait_journal_securite.csv`: Un extrait de journaux Windows.
- `03_Liste_processus`: Une liste de processus en cours d'exécution.
- `04_Connexions_reseau.csv`: Un relevé de connexions réseau.
- `05_Rapport_antivirus`: Un rapport d'analyse antivirus.

### 1. Examiner les indices

Examinez chaque élément du dossier et relevez les informations utiles à l'investigation.

Identifiez :

1. Courriel suspect
    - L'expéditeur et les éléments inhabituels de son adresse.
    - Le sujet et le contenu du message.
    - Les liens ou pièces jointes qui pourraient présenter un risque.
    - Les techniques de manipulation éventuellement utilisées.
2. Journaux Windows
    - Les ouvertures de session réussies ou échouées.
    - Les événements inhabituels liés aux comptes.
    - Les événements dont l'heure pourrait correspondre aux activités signalées.
    - Expliquez ce que chaque événement permet réellement d'affirmer.
3. Processus en cours d'exécution
    - Relevez les processus qui méritent une vérification.
    - Expliquez pourquoi leur nom, leur emplacement ou leur comportement pourrait être pertinent, si ces informations sont disponibles.
    - Distinguez les processus réellement suspects de ceux qui sont simplement inconnus.
    - Un nom de processus inhabituel ne suffit pas, à lui seul, à prouver qu'il est malveillant.
4. Connexions réseau
    - Relevez les adresses IP, ports et états disponibles.
    - Identifiez les connexions qui nécessiteraient une vérification.
    - Établissez les liens possibles avec les autres éléments du dossier.
    - Ne concluez pas qu'une adresse est malveillante uniquement parce qu'elle est inconnue.
5. Rapport antivirus
    - Les menaces ou éléments signalés.
    - Le chemin du fichier concerné, s'il est indiqué.
    - L'action effectuée par l'antivirus.
    - Les éléments qui restent à vérifier.

### 2. Reconstituer la chronologie

Construisez une chronologie des événements à partir de l'ensemble des indices.

Pour chaque étape, indiquez :

| Heure ou période | Indice observé | Source | Interprétation | Niveau de certitude |
|---|---|---|---|---|
| À compléter | À compléter | À compléter | À compléter | Fait / hypothèse |

Votre chronologie doit permettre de répondre aux questions suivantes :

1. Quel événement pourrait être à l'origine de l'incident?
2. Quels indices suggèrent qu'une activité inhabituelle s'est produite?
3. Quels éléments corroborent les autres?
4. Quelles informations manquent pour confirmer le scénario?

Si l'heure exacte d'un événement n'est pas disponible, indiquez une période approximative ou précisez que l'ordre ne peut pas être établi avec certitude.

### 3. Évaluer l'incident

À partir des indices, répondez aux questions suivantes :

1. Le poste semble-t-il compromis? Justifiez votre évaluation.
2. Quel scénario explique le mieux les observations?
3. Quels indices sont les plus probants? Lesquels restent ambigus?
4. Quelles autres vérifications seraient nécessaires avant de tirer une conclusion définitive?
5. Quel est le risque principal pour l'utilisateur ou l'organisation?

Distinguez clairement les faits observés, les hypothèses et les conclusions.

### 4. Proposer un plan d'intervention

Supposez que vous êtes responsable de la première intervention sur le poste.

Proposez un plan comprenant les actions suivantes :

1. **Contenir l'incident :** quelles mesures immédiates permettraient de limiter les risques?
2. **Préserver les preuves :** quelles données devraient être conservées avant toute modification importante?
3. **Investiguer :** quelles vérifications supplémentaires permettraient de mieux comprendre l'incident?
4. **Rétablir le poste :** quelles mesures pourraient être nécessaires pour retrouver un environnement fiable?
5. **Prévenir une récidive :** quelles améliorations techniques ou humaines recommanderiez-vous?

Justifiez vos choix. Évitez les actions irréversibles avant d'avoir évalué la nécessité de préserver les preuves.