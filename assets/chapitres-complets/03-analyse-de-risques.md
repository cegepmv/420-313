# 3. Analyse de risques
----------

Dans ce chapitre, nous étudions comment une **analyse de risque** se réalise. À la fin de ce chapitre, vous serez capable de faire une analyse de risque simple sur une entreprise ou un cas particulier.

## Objectif de la sécurité de l'information

Empêcher l'exploitation de failles (**vulnérabilités**) contre le système. Autrement dit : s'il n'y avait pas de *menaces* ni de *vulnérabilités*, on ne ferait pas de sécurité informatique.

## Menace

Qui dit **menace** suppose qu'il y ait :

- Un **bien** à protéger (objet ou personne) ayant de la **valeur**.
- Un **acteur** ou **agent de menace** qui a pour objectif de porter atteinte à la valeur de ce bien.

### Catégorisation des menaces

- **Accidentelles** vs **malveillantes** : une menace accidentelle résulte d'une erreur humaine ou d'une défaillance (ex. : employé qui supprime un fichier par erreur), alors qu'une menace malveillante résulte d'une intention de nuire.
- **Externes** vs **internes** : une menace externe provient d'un acteur hors de l'organisation, alors qu'une menace interne provient d'un employé, d'un sous-traitant ou d'une personne ayant un accès légitime.

![menaces accidentelles vs malveillantes](/images/03-menaces.png?height=60vh)

## Vulnérabilité

Une **vulnérabilité** est une faiblesse dans un système (*technique*, *organisationnelle* ou *humaine*) qui peut être exploitée par une menace pour porter atteinte à un bien.

## Risque

Le **risque** résulte de la combinaison d'une menace et d'une vulnérabilité exploitable, pondérée par l'impact potentiel sur l'organisation.

## Évaluation des risques

L'évaluation d'un risque repose généralement sur deux dimensions :

- **L'impact** : la gravité des conséquences si le risque se matérialise (financière, réputationnelle, opérationnelle, légale).
- **La probabilité** : la vraisemblance que le risque se matérialise dans un horizon de temps donné.

Le produit (*impact × probabilité*) permet d'obtenir un **ordonnancement** des risques, souvent représenté sous forme de matrice, afin de prioriser les efforts de mitigation sur les risques les plus critiques.

![Catégorisation des risques avec la méthode de Farmer](/images/03-risques.png)

## Réduction des risques et choix des contre-mesures

Une fois les risques identifiés et priorisés, quatre traitements sont possibles pour chacun :

- **Acceptation** : l'impact est jugé négligeable (ex. : vol d'un portable ne contenant pas de données sensibles).
- **Évitement** : mise en place d'une contre-mesure ou d'une parade qui élimine le risque.
- **Transfert** : le risque est transféré à un tiers (sous-traitance, assurance) lorsqu'il ne peut être évité ni raisonnablement réduit.
- **Réduction** : ramener le risque à un niveau acceptable sans l'éliminer complètement.

Une fois le traitement choisi, il faut réévaluer le **risque résiduel** et, au besoin, définir des mesures supplémentaires. Le choix d'une contre-mesure doit toujours tenir compte du rapport *coût/bénéfice* (principe de la protection adéquate vu au chapitre 2).

## Méthodes d'analyse de risques

Plusieurs méthodologies structurées existent pour guider une analyse de risques en entreprise, parmi lesquelles :

- **EBIOS Risk Manager** (méthode française, ANSSI)
- **MEHARI**
- Des grilles simplifiées propres à l'organisation, lorsque le contexte ne justifie pas une méthode formelle complète

Toutes ces méthodes suivent globalement la même logique : identification des biens à protéger, identification des menaces et vulnérabilités associées, évaluation du risque, puis choix et suivi des mesures de traitement.





# 3.1. Atelier étude de cas
----------

### Cas 1 — TransLog Inc.

Reprenons le contexte de **TransLog Inc.**, une PME de transport et logistique de 60 employés (chauffeurs, répartiteurs, personnel administratif, un responsable TI), qui exploite un logiciel de gestion de flotte avec géolocalisation GPS, un système de facturation, une messagerie courriel et un site web.
 
### Méthode en 3 étapes
 
**Étape 1 — Identifier le risque** : chaque risque se décrit comme la combinaison d'une **menace** (ce qui pourrait causer un dommage), d'une **vulnérabilité** (la faiblesse exploitée) et d'un **bien touché** (l'actif affecté).
 
**Étape 2 — Évaluer le risque** : pour chaque risque, on estime :
- l'**impact** (conséquence si le risque se matérialise), de 1 (négligeable) à 5 (catastrophique) ;
- la **probabilité** (chance que ça arrive), de 1 (très rare) à 5 (quasi certain) ;
- le **niveau de risque** = impact × probabilité (échelle de 1 à 25).

**Étape 3 — Prioriser et traiter** : les risques avec le score le plus élevé sont traités en priorité, selon les quatre types de traitement vus à la section 3.6 (acceptation, évitement, transfert, réduction).
 
### Grille d'analyse — TransLog
 
| # | Menace | Vulnérabilité | Bien touché | Impact | Probabilité | Risque | Priorité |
|---|---|---|---|---|---|---|---|
| 1 | Vol de données de géolocalisation par un concurrent | Absence de chiffrement des communications GPS | Données de géolocalisation des camions | 4 | 3 | 12 | Élevée |
| 2 | Employé malveillant ou négligent consultant des dossiers clients sans raison | Absence de contrôle d'accès par rôle | Contrats et données de facturation | 3 | 3 | 9 | Moyenne |
| 3 | Rançongiciel (ransomware) via pièce jointe courriel | Absence de formation du personnel à l'hameçonnage | Système de facturation, postes administratifs | 5 | 3 | 15 | Critique |
| 4 | Panne du serveur hébergeant le logiciel de gestion de flotte | Absence de sauvegarde ou de redondance | Disponibilité du service de répartition | 4 | 2 | 8 | Moyenne |
| 5 | Défiguration (*defacement*) du site web par un attaquant externe | Logiciel du site web non maintenu à jour | Réputation de l'entreprise, site web | 2 | 3 | 6 | Faible |
| 6 | Interception de courriels non chiffrés contenant des soumissions clients | Absence de chiffrement de la messagerie | Confidentialité des données clients | 3 | 2 | 6 | Faible |
| 7 | Vol physique d'un ordinateur portable d'un répartiteur | Absence de chiffrement du disque dur | Données de facturation et itinéraires | 4 | 2 | 8 | Moyenne |
 
### Contre-mesures priorisées (risques critiques et élevés)
 
| Risque priorisé | Contre-mesure proposée |
|---|---|
| **#3 — Rançongiciel** (score 15) | Formation obligatoire du personnel sur l'hameçonnage ; filtre anti-pourriel avancé ; sauvegardes régulières hors ligne (isolées du réseau) |
| **#1 — Vol de données GPS** (score 12) | Chiffrement des communications entre les camions et le serveur ; authentification renforcée pour l'accès à l'application de suivi |
 
{{% notice note "Remarque" %}}
On ne traite pas nécessairement tous les risques immédiatement : selon le **principe de la protection adéquate**, l'entreprise peut choisir d'**accepter** temporairement certains risques faibles (ex. : #5, #6) si le coût de la contre-mesure dépasse la valeur du bien à protéger.
{{% /notice %}}
 
## Cas 2 — BioVerte
 
En reprenant le contexte de **BioVerte**, une PME agroalimentaire de 40 employés qui produit et distribue des aliments biologiques (système de gestion des stocks et de production, formulations propriétaires, logiciel de commandes en ligne, messagerie avec fournisseurs, site web, réseau interne avec tablettes en usine), et **en suivant exactement la même méthode que pour TransLog** :
 
1. **Identifiez 5 à 8 risques distincts** touchant BioVerte, en décrivant pour chacun la **menace**, la **vulnérabilité** exploitée, et le **bien touché**.
2. **Évaluez chaque risque** sur une échelle de 1 à 5 pour l'impact et la probabilité, puis calculez le niveau de risque (impact × probabilité).
3. **Classez les risques** du plus critique au moins critique.
4. **Proposez une contre-mesure priorisée** pour les 2 ou 3 risques les plus critiques, en justifiant pourquoi ils méritent d'être traités en premier.
Présentez votre réponse sous forme de tableau, comme dans l'exemple de TransLog.
<!--  
{{% expand "Solution indicative" %}}
 
### Grille d'analyse
 
| # | Menace | Vulnérabilité | Bien touché | Impact | Probabilité | Risque | Priorité |
|---|---|---|---|---|---|---|---|
| 1 | Vol ou espionnage industriel visant les formulations propriétaires | Absence de chiffrement et de contrôle d'accès strict sur les fichiers de recettes | Formulations et recettes propriétaires | 5 | 2 | 10 | Élevée |
| 2 | Employé quittant l'entreprise et copiant des recettes avant son départ | Absence de restriction sur les supports amovibles (USB) et de journalisation des accès | Formulations propriétaires | 5 | 3 | 15 | Critique |
| 3 | Rançongiciel affectant le système de gestion de la production | Tablettes d'usine non mises à jour, absence de segmentation réseau | Système de gestion des stocks et de production | 4 | 3 | 12 | Élevée |
| 4 | Erreur humaine lors de la saisie des commandes en ligne | Absence de validation ou de double vérification des données saisies | Intégrité des commandes clients | 2 | 4 | 8 | Moyenne |
| 5 | Panne du serveur de gestion des stocks | Absence de sauvegarde régulière ou de système redondant | Disponibilité de la production | 4 | 2 | 8 | Moyenne |
| 6 | Interception de courriels non chiffrés avec les fournisseurs | Absence de chiffrement de la messagerie | Confidentialité des ententes commerciales | 3 | 2 | 6 | Faible |
| 7 | Défiguration du site web par un attaquant externe | Logiciel du site web non maintenu à jour | Réputation de l'entreprise | 2 | 3 | 6 | Faible |
| 8 | Accès non autorisé au réseau interne via une tablette d'usine volée ou perdue | Absence de verrouillage automatique ou de chiffrement sur les tablettes | Réseau interne, données de production | 3 | 3 | 9 | Moyenne |
 
### Contre-mesures priorisées
 
| Risque priorisé | Contre-mesure proposée |
|---|---|
| **#2 — Vol de recettes par un employé** (score 15) | Restriction et journalisation des accès aux fichiers de formulation ; blocage des ports USB sur les postes ayant accès aux recettes ; clause de confidentialité renforcée et processus de révocation d'accès au départ d'un employé |
| **#3 — Rançongiciel via tablettes d'usine** (score 12) | Segmentation du réseau (isoler les tablettes de production du reste du réseau) ; mise à jour régulière des systèmes ; sauvegardes hors ligne du système de production |
| **#1 — Espionnage industriel des formulations** (score 10) | Chiffrement des fichiers de recettes ; authentification à deux facteurs ; accès restreint à un nombre limité de personnes nommément identifiées (direction, R&D) |
 
### Point de discussion en classe
 
Contrairement à TransLog, où le risque le plus critique provenait d'une menace **externe** (hameçonnage/rançongiciel), le risque le plus critique de BioVerte (#2) provient d'une menace **interne** — ce qui illustre que l'analyse de risques dépend fortement du bien le plus précieux de l'entreprise (ici, la propriété intellectuelle) et pas seulement du type de technologie utilisée.
 
On peut aussi faire remarquer que le risque #1, bien qu'ayant l'impact le plus élevé possible (5), n'est pas le plus critique : sa probabilité plus faible (2) illustre bien que le **niveau de risque combine les deux facteurs**, et qu'un impact catastrophique mais peu probable peut être moins prioritaire qu'un risque plus probable à impact élevé.
 
{{% /expand %}} -->