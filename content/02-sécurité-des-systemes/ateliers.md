+++
title = 'Exercices et ateliers'
weight = '210'
draft = false
+++


## 1- CIA

### Stockage infonuagique (cloud) pour documents d'entreprise

L'adoption de services de stockage infonuagique (type Google Drive, Dropbox, OneDrive) permet aux employés de synchroniser et partager facilement des documents de travail depuis n'importe quel appareil.

**Avantages :**
+ Accessibilité depuis n'importe où et n'importe quel appareil
+ Collaboration en temps réel entre plusieurs employés
+ Sauvegarde automatique, réduisant le risque de perte de données

Quelles sont les vulnérabilités (potentielles) du stockage infonuagique ? (Confidentialité/Intégrité/Disponibilité)

### Clés USB personnelles en milieu de travail

Les employés utilisent fréquemment leurs propres clés USB pour transférer des fichiers entre leur poste de travail et leur domicile, ou entre collègues.

**Avantages :**
+ Facilité et rapidité de transfert de fichiers volumineux
+ Aucun coût pour l'entreprise
+ Indépendance vis-à-vis du réseau ou d'une connexion Internet

Quelles sont les vulnérabilités (potentielles) des clés persionnelles en milieu de travail ? (Confidentialité/Intégrité/Disponibilité)

### Télétravail via réseau Wi-Fi domestique

De plus en plus d'employés se connectent au réseau de l'entreprise depuis leur domicile, via leur propre routeur Wi-Fi, pour accéder aux applications et serveurs internes (VPN ou accès direct).

**Avantages :**
+ Flexibilité horaire et géographique pour les employés
+ Réduction des coûts immobiliers pour l'entreprise
+ Continuité des opérations en cas d'événement empêchant l'accès aux bureaux

Quelles sont les vulnérabilités (potentielles) du télétravail via réseau Wi-Fi domestique ? (Confidentialité/Intégrité/Disponibilité)

### Assistants vocaux intelligents (Alexa, Google Home) en entreprise

Certains bureaux installent des assistants vocaux dans les salles de conférence pour faciliter la prise de notes, la planification de réunions ou le contrôle de l'éclairage/climatisation.

**Avantages :**
+ Gain de temps pour les tâches administratives (prise de rendez-vous, rappels)
+ Contrôle mains libres des équipements de la salle
+ Impression de modernité pour les visiteurs et clients

Quelles sont les vulnérabilités (potentielles) des assistants vocaux intelligents dans les lieux de travail ? (Confidentialité/Intégrité/Disponibilité)

### Dispositifs médicaux connectés (IoT santé)

Certains hôpitaux et cliniques utilisent des dispositifs médicaux connectés (pompes à perfusion intelligentes, moniteurs cardiaques sans fil) reliés au réseau interne pour la surveillance à distance des patients.

**Avantages :**
+ Surveillance continue et en temps réel des signes vitaux
+ Réduction du temps de déplacement du personnel infirmier
+ Alertes automatiques en cas d'anomalie

Quelles sont les vulnérabilités (potentielles) du stockage infonuagique ? (Confidentialité/ Intégrité/Disponibilité)

## 2- Mise en place d'une politique de sécurité

### TransLog Inc.

**TransLog Inc.** est une PME de transport et logistique comptant 60 employés : chauffeurs, répartiteurs, personnel administratif et un responsable TI. Elle utilise un logiciel de gestion de flotte (géolocalisation GPS des camions, itinéraire de livraison), un système de facturation client, une messagerie courriel, et un site web avec formulaire de demande de soumission. Un Wi-Fi public est aussi offert aux visiteurs dans la salle d'attente du garage.

Pour élaborer sa politique de sécurité, *TransLog* suit la méthode en 4 étapes :

#### Étape 1 — Définir le périmètre

On identifie tout ce qui est couvert par la politique : systèmes, locaux, personnes.

*Exemple TransLog* :

+ **Systèmes couverts :** logiciel de gestion de flotte, système de facturation, messagerie, site web, postes administratifs.
+ **Locaux couverts :** bureaux administratifs, garage (accès aux clés des véhicules).
+ **Personnes couvertes :** employés administratifs, répartiteurs, chauffeurs (accès à l'application mobile de livraison), responsable TI.
+ **Exclu du périmètre :** réseau Wi-Fi public offert aux visiteurs dans la salle d'attente du garage.

#### Étape 2 — Identifier et classifier l'information

On liste les types d'information manipulés, puis on les regroupe en catégories (typiquement 3 à 5 : public, interne, confidentiel, restreint).

*Exemple TransLog* :

|Catégorie|	Exemples chez TransLog|
|---------|-----------------------|
|Public|	Contenu du site web, liste des services de transport offerts|
|Interne|	Horaires de répartition, procédures internes de chargement|
|Confidentiel|	Contrats clients, données de facturation, dossiers RH des employés|
|Restreint|	Données de géolocalisation en temps réel des camions, itinéraires de livraison de marchandises sensibles|

#### Étape 3 — Évaluer la valeur et le risque de chaque catégorie

On applique ici le **principe de la protection adéquate** : la protection doit correspondre à la valeur de ce qu'on protège, ni plus ni moins.

*Exemple TransLog :* les données de géolocalisation sont jugées à risque élevé (elles pourraient permettre de planifier un vol de marchandises), donc elles justifient une protection renforcée malgré le coût.

#### Étape 4 — Associer une mesure de sécurité à chaque catégorie

On choisit des mesures techniques, organisationnelles ou humaines, proportionnées au risque.

*Exemple TransLog :*

|Catégorie |	Mesure |
|----------|---------|
|Public|	Validation du contenu avant publication|
|Interne|	Accès par compte employé authentifié|
|Confidentiel|	Accès limité par rôle ; chiffrement des courriels contenant des contrats|
|Restreint|	Chiffrement des données GPS ; accès à l'application de suivi limité aux répartiteurs et à la direction ; journalisation des consultations d'itinéraires|

## BioVerte

**BioVerte** est une PME agroalimentaire de 40 employés qui produit et distribue des aliments biologiques. Elle exploite :

+ un système de gestion des stocks et de la chaîne de production (recettes, formulations propriétaires) ;
+ un logiciel de commandes en ligne pour les clients (épiceries, restaurants) ;
+ une messagerie courriel avec les fournisseurs et distributeurs ;
+ un site web vitrine présentant les produits ;
+ un réseau interne reliant les postes du bureau et les tablettes utilisées en usine pour le suivi de production.

Les formulations de *BioVerte* sont le fruit de plusieurs années de recherche et développement. Elles permettent à l'entreprise de se différencier de ses concurrents sur le marché des aliments biologiques (aucun autre fournisseur n'offre exactement les mêmes recettes).

En suivant exactement les 4 étapes de la méthode illustrée avec *TransLog*, élaborez une ébauche de politique de sécurité pour BioVerte :

1. **Définissez le périmètre :** quels systèmes, locaux et personnes sont couverts ? Y a-t-il des exclusions ?
2. **Identifiez et classifiez l'information :** proposez 3 à 5 catégories, avec un exemple concret pour chacune tiré du contexte de BioVerte.
3. **Évaluez la valeur et le risque** de chaque catégorie : laquelle mérite la protection la plus stricte, et pourquoi ? (Indice : que représente la formulation propriétaire des produits pour BioVerte ?)
4. **Associez une mesure de sécurité** à chaque catégorie, proportionnée au risque identifié à l'étape 3.

*Présentez votre réponse sous forme de tableau, comme dans l'exemple de TransLog.*
