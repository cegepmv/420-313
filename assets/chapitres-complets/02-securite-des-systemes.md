# 2. Sécurité des systèmes
------------


Le **système d'information** représente un patrimoine essentiel de l'organisation qu'il convient de protéger. La sécurité informatique consiste à garantir que les ressources matérielles ou logicielles d'une organisation sont **uniquement utilisées dans le cadre prévu**.

C'est l'ensemble des moyens techniques, organisationnels, juridiques et humains nécessaires et mis en place pour conserver, rétablir et garantir la sécurité du système d'information. La raison principale de l'existence de l'industrie de la sécurité informatique est que **les produits et services informatiques ne sont pas naturellement sûrs**.


## Objectifs de la sécurité informatique : CIA

Les trois objectifs fondamentaux (*Confidentiality, Integrity, Availability*) :

<!-- 📊 Illustration suggérée : triangle CIA (Confidentialité / Intégrité / Disponibilité) avec un exemple pour chaque sommet. -->

![CIA](/images/02-CIA.png?height=50vh)

### Confidentialité de l'information
L'information n'est **accessible qu'à ceux qui en ont le droit** (ou le besoin dans le cadre de leur travail/responsabilité). Cette notion peut évoluer avec le temps et implique un équilibre entre intérêts publics et privés.

### Intégrité des services et de l'information

Les services et l'information ne peuvent être **modifiés que par les individus autorisés** (administrateurs, propriétaires, etc.). 
+ **Objectifs :** exactitude, précision, autorisation de modification, cohérence.

### Disponibilité des services et de l'information

Les services et l'information ne sont accessibles qu'aux personnes autorisées, et **quand elles en ont besoin**. Doit tenir compte des contraintes de temps, de qualité et de performance.

### Objectifs complémentaires
- **L'authentification** : identifier les utilisateurs afin de pouvoir gérer les accès et maintenir la confiance.
- **L'autorisation** : déterminer ce qu'un utilisateur authentifié a le droit de faire ou de consulter.
- **La non-répudiation et l'imputation** : ne pas être en mesure de nier ses propres actes, ni de s'attribuer les actes de quelqu'un d'autre.
- **La journalisation ou traçabilité** : répertorier tout accès, toute modification, etc.

### Pourquoi est-ce important pour une organisation ?
+ Protéger sa réputation.
+ Assurer la continuité de ses activités.
+ Protéger ses données stratégiques et sa propriété intellectuelle.
+ Protéger les données privées des employés et clients.
+ Se protéger de la fraude.
+ Satisfaire aux exigences légales.
+ Éviter des pertes financières.

### Principes fondamentaux

+ **Principe du point le plus faible** : une personne qui cherche à pénétrer un système utilisera tous les moyens possibles pour le faire, mais pas nécessairement le moyen le plus évident ou celui bénéficiant de la défense la plus solide.
  + Un système de sécurité est aussi fort que son maillon le plus faible.
  + Une seule faille suffit pour briser toute la sécurité.
  + Les attaquants ciblent toujours l'élément le plus vulnérable.
  + Exemples: 
    + Mot de passe : Un compte ultra-sécurisé peut être piraté si la question de sécurité ou l'e-mail de récupération est facile à deviner.
    + Réseau d'entreprise : Un pare-feu très cher ne sert à rien si un employé clique sur un lien de phishing.

+ **Principe de la protection adéquate (gestion du risque)** : le niveau et le coût de la protection doivent correspondre à l'importance et à la valeur de ce qu'on veut protéger. 
  + **Exemple données publiques vs sensibles :** Un site web vitrine d'entreprise ne nécessite pas de chiffrement lourd des pages lues par tous, contrairement à une base de données bancaire qui exige un chiffrement fort (AES-256) des données au repos et en transit.


## Organismes de certification et normes
- **Suite ISO/IEC 27000** : « Technologies de l'information — Techniques de sécurité — Systèmes de gestion de la sécurité de l'information » (anciennement ISO 17799). La suite comprend notamment :
  - ISO/IEC 27001 : exigences du SMSI
  - ISO/IEC 27002 : code de pratique pour la gestion de la sécurité de l'information
  - ISO/IEC 27005 : gestion des risques
  - ISO/IEC 27032 : cybersécurité
  - ISO/IEC 27035 : gestion des incidents de sécurité
- **CERT (Computer Emergency Response Team)** : organismes officiels chargés d'assurer des services de prévention des risques et d'assistance au traitement d'incidents.
- **(ISC)² (International Information Systems Security Certification Consortium)** : organisation chargée de certifier des professionnels de la sécurité (CISSP, SSCP, CAP, CSSLP, etc.).
- **Certified Ethical Hacker (CEH)** : certification évaluant la capacité du candidat à identifier les failles d'un système en adoptant une posture d'attaquant.
- **CISM (Certified Information Security Manager)** et autres certifications ISACA (CISA, CGEIT, CRISC), orientées gestion du risque.
- **GIAC/SANS Institute** : certifications techniques (détection d'intrusion, enquêtes numériques, gestion d'incidents).
- Normes sectorielles : par exemple HIPAA pour les systèmes traitant des informations médicales aux États-Unis.

### Norme ISO 27001 et mise en place d'un SMSI

Se conformer à ISO 27001 aide une organisation à protéger ses informations et garantit la confiance des parties intéressées. La norme adopte une approche par processus pour la création, la mise en œuvre, la surveillance, le maintien et l'amélioration continue du système de gestion de la sécurité de l'information (SMSI). Elle :

- atteste du caractère indépendant des contrôles internes ;
- atteste du respect de la législation et de la réglementation en vigueur ;
- fournit un avantage concurrentiel en répondant aux exigences contractuelles ;
- permet de vérifier indépendamment que les risques sont correctement identifiés, audités et gérés ;
- atteste de l'engagement de la direction envers la sécurité de l'information.

### Le cycle PDCA (roue de Deming)

![PDCA](/images/02-pdca.webp)

Le SMSI fonctionne selon un modèle cyclique en 4 étapes appelé **PDCA** (Plan, Do, Check, Act).

**Phase Plan** — fixer les objectifs, en 4 étapes :
1. Définir le périmètre et la politique du SMSI (le périmètre définit le domaine d'application ; la politique fixe le niveau de sécurité recherché en fonction de la CIA).
2. Faire l'analyse des risques (voir chapitre 3).
3. Traiter le risque et identifier le risque résiduel — quatre traitements possibles : **acceptation**, **évitement**, **transfert**, **réduction**.
4. Choisir les mesures de sécurité à mettre en place.

**Phase Do** — mettre en œuvre les objectifs du Plan, en 4 étapes :
1. Établir un plan de traitement.
2. Déployer les mesures de sécurité.
3. Générer des indicateurs permettant de valider l'efficacité des mesures.
4. Former et sensibiliser le personnel.

**Phase Check** — gestion quotidienne du SMSI et détection des incidents, par des audits internes vérifiant la conformité et l'efficacité du SMSI.

**Phase Act** — corriger les écarts constatés, en 3 étapes :
1. **Actions correctives** : corriger les effets.
2. **Actions préventives** : agir sur les causes pour éviter la récurrence.
3. **Actions d'amélioration** : améliorer la performance d'un processus du SMSI.

### La certification
La certification n'est pas obligatoire, mais elle permet d'afficher la conformité à la norme. Il existe trois types d'audits :

- **Audit initial** : porte sur l'ensemble du SMSI ; si accordée, la certification est valide 3 ans.
- **Audit de surveillance** : un par année, portant sur les points à surveiller, le traitement des plaintes et la viabilité du SMSI.
- **Audit de renouvellement** : à l'échéance du certificat.

### Limites de la certification
- Les audits sont conduits sur des périodes souvent trop courtes.
- La mise en place d'une méthodologie rigoureuse est fastidieuse.
- L'application de la norme ne garantit pas, à elle seule, la réduction du risque de piratage.

## Politique de sécurité et classification de l'information
Une **politique de sécurité** formalise les règles et responsabilités en matière de sécurité de l'information au sein de l'organisation. Elle s'accompagne généralement d'une **classification de l'information** (par exemple : public, interne, confidentiel, restreint), qui permet d'appliquer des mesures de protection proportionnées à la sensibilité de chaque catégorie.

## Références et liens
+ [Suite ISO/CEI 27000 - Wikipédia](https://fr.wikipedia.org/wiki/Suite_ISO/CEI_27000)
+ [Computer emergency response team (CERT) - Wikipédia](https://fr.wikipedia.org/wiki/Computer_emergency_response_team)
+ [(ISC)² - Site officiel](https://www.isc2.org/)
+ [ECCounsil - Certified Ethical Hacker (CEH)](https://www.eccouncil.org/train-certify/certified-ethical-hacker-ceh-v13-north-america/)
+ [ISACA-CICM - Site officiel](https://www.isaca.org/credentialing/cism)
+ [Certifications GIAC - Site officiel](https://www.giac.org/)
+ [HIPAA - Wikipédia](https://fr.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)




# 2.1. Ateliers - Politique de sécurité
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


# 2.2. Exercices - CIA
----------

Identifiez les vulnérabilités potentielles (confidentialité/intégrité/disponibilité) pour les cas suivants :

### Stockage infonuagique (cloud) pour documents d'entreprise

L'adoption de services de stockage infonuagique (type Google Drive, Dropbox, OneDrive) permet aux employés de synchroniser et partager facilement des documents de travail depuis n'importe quel appareil.

**Avantages :**
+ Accessibilité depuis n'importe où et n'importe quel appareil
+ Collaboration en temps réel entre plusieurs employés
+ Sauvegarde automatique, réduisant le risque de perte de données

{{% expand title="Solution" %}}
**Confidentialité**
  + Accès non autorisé si les identifiants de connexion sont compromis (hameçonnage, mot de passe faible). 
  + Partage de liens de documents mal configurés (lien "public" au lieu de "restreint"). 
  + Le fournisseur infonuagique lui-même (ou un employé du fournisseur) pourrait potentiellement accéder aux données stockées. 

**Intégrité**
  + Modification accidentelle ou malveillante d'un document partagé par un utilisateur autorisé. 
  + Synchronisation erronée pouvant écraser une version plus récente d'un fichier. 
  + Absence de contrôle de versions pouvant masquer des modifications non autorisées. 

**Disponibilité**
  + Panne ou interruption de service chez le fournisseur infonuagique (hors du contrôle de l'entreprise). 
  + Perte de connexion Internet empêchant l'accès aux fichiers. 
  + Suppression accidentelle d'un fichier par un utilisateur, sans sauvegarde locale. 
{{% /expand %}}

### Clés USB personnelles en milieu de travail

Les employés utilisent fréquemment leurs propres clés USB pour transférer des fichiers entre leur poste de travail et leur domicile, ou entre collègues.

**Avantages :**
+ Facilité et rapidité de transfert de fichiers volumineux
+ Aucun coût pour l'entreprise
+ Indépendance vis-à-vis du réseau ou d'une connexion Internet

{{% expand title="Solution" %}}
**Confidentialité**
  + Perte ou vol physique de la clé USB contenant des données sensibles non chiffrées. 
  + Fuite de données lorsque la clé est réutilisée sur un poste personnel ou public non sécurisé. 

**Intégrité**
  + Introduction de logiciels malveillants (virus, ransomware) depuis une clé USB infectée branchée sur le réseau de l'entreprise. 
  + Modification non tracée de fichiers transférés hors du contrôle des systèmes de l'entreprise. 

**Disponibilité**
  + Défaillance matérielle de la clé USB entraînant la perte des données qui n'existaient que sur ce support.  
  + Contamination du réseau par un maliciel propagé via la clé, pouvant rendre des systèmes indisponibles (ex. : ransomware). 
  + Suppression accidentelle d'un fichier par un utilisateur, sans sauvegarde locale. 
{{% /expand %}}

### Télétravail via réseau Wi-Fi domestique

De plus en plus d'employés se connectent au réseau de l'entreprise depuis leur domicile, via leur propre routeur Wi-Fi, pour accéder aux applications et serveurs internes (VPN ou accès direct).

**Avantages :**
+ Flexibilité horaire et géographique pour les employés
+ Réduction des coûts immobiliers pour l'entreprise
+ Continuité des opérations en cas d'événement empêchant l'accès aux bureaux

{{% expand title="Solution" %}}
**Confidentialité**
  + Routeur domestique mal configuré (mot de passe par défaut, chiffrement faible ou absent) permettant l'interception du trafic. 
  + Interception de communications non chiffrées par un tiers connecté au même réseau (voisin, colocataire).
  + Absence de VPN exposant les échanges avec les serveurs de l'entreprise. 

**Intégrité**
  + Attaque de l'homme du milieu (*man-in-the-middle*) permettant de modifier des données en transit. 

**Disponibilité**
  + Panne de la connexion Internet résidentielle, sans solution de secours.   
  + Attaque par déni de service ciblant le réseau domestique ou le VPN de l'employé. 
  + Partage de bande passante avec d'autres appareils du foyer ralentissant l'accès aux applications de l'entreprise.
{{% /expand %}}

### Assistants vocaux intelligents (Alexa, Google Home) en entreprise

Certains bureaux installent des assistants vocaux dans les salles de conférence pour faciliter la prise de notes, la planification de réunions ou le contrôle de l'éclairage/climatisation.

**Avantages :**
+ Gain de temps pour les tâches administratives (prise de rendez-vous, rappels)
+ Contrôle mains libres des équipements de la salle
+ Impression de modernité pour les visiteurs et clients

{{% expand title="Solution" %}}
**Confidentialité**
  + Écoute continue (activation accidentelle) captant des conversations confidentielles de réunion. 
  + Transmission et stockage des enregistrements sur les serveurs du fabricant, hors du contrôle de l'entreprise. 
  + Accès par un tiers non autorisé au compte associé à l'assistant.

**Intégrité**
  + Commandes vocales usurpées (rejouées ou imitées) pouvant déclencher des actions non désirées (ex. : modifier un rendez-vous). 
  + Manipulation du dispositif par une personne ayant un accès physique à la salle. 

**Disponibilité**
  + Panne de connexion Internet rendant l'assistant inutilisable pour les fonctions dépendant du cloud. 
  + Panne du service du fabricant (hors du contrôle de l'entreprise). 
{{% /expand %}}

### Dispositifs médicaux connectés (IoT santé)

Certains hôpitaux et cliniques utilisent des dispositifs médicaux connectés (pompes à perfusion intelligentes, moniteurs cardiaques sans fil) reliés au réseau interne pour la surveillance à distance des patients.

**Avantages :**
+ Surveillance continue et en temps réel des signes vitaux
+ Réduction du temps de déplacement du personnel infirmier
+ Alertes automatiques en cas d'anomalie

{{% expand title="Solution" %}}
**Confidentialité**
  + Interception de données de santé transmises sans chiffrement adéquat sur le réseau. 
  + Accès non autorisé au dispositif si les protocoles d'authentification sont faibles ou absents (fréquent sur les dispositifs IoT plus anciens). 

**Intégrité**
  + Falsification des données transmises (ex. : signes vitaux modifiés), pouvant mener à un mauvais diagnostic ou traitement. 
  + Piratage du dispositif permettant de modifier ses paramètres de fonctionnement (dosage d'une pompe à perfusion, par exemple). 

**Disponibilité**
  + Panne réseau empêchant la transmission des alertes en temps réel au personnel infirmier. 
  + Attaque par déni de service ciblant le réseau hospitalier, affectant plusieurs dispositifs connectés simultanément.
  + Défaillance de la batterie ou du dispositif lui-même sans redondance.
{{% /expand %}}
