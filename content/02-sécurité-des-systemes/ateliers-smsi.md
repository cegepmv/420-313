+++
title = 'Ateliers - Politique de sécurité'
weight = '220'
draft = false
+++
----------


### Cas 1 — TransLog Inc.

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

### Cas 2 — BioVerte

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
