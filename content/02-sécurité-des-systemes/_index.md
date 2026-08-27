+++
pre = '<b>02. </b>'
title = 'Sécurité des systèmes'
weight = '200'
draft = false
+++
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