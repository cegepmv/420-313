+++
pre = '<b>03. </b>'
title = 'Analyse de risques'
weight = '300'
draft = false
+++
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



