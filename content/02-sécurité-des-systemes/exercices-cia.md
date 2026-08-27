+++
title = 'Exercices - CIA'
weight = '210'
draft = false
+++
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
