+++
pre = '<b>01. </b>'
title = 'Introduction'
weight = '100'
draft = false
+++

----------------

Nous accédons à des informations financières en ligne, nous faisons des achats sur des sites web de détaillants et nous partageons des données personnelles sur les réseaux sociaux. Comme nous utilisons de plus en plus les plateformes numériques et que nous en sommes de plus en plus tributaires, nous sommes davantage exposés à divers risques de cybersécurité. Les pirates informatiques malveillants exploitent les failles de sécurité pour capitaliser sur les données personnelles des personnes et sur l'empreinte numérique croissante des organisations.

Alors que le monde devient de plus en plus interconnecté et dépendant des technologies numériques, la cybercriminalité est en plein essor. Face aux menaces de fraude financière, d'accès non autorisé et d'usurpation d'identité, la cybersécurité n'a jamais été aussi cruciale.


## Qu'est-ce que la cybersécurité ?

La cybersécurité englobe la **technologie**, les **pratiques** et les **mesures** utilisées pour atténuer les cybermenaces et s'en protéger, notamment le *phishing*, les logiciels malveillants (*malware*), les rançongiciels (*ransomware*) et d'autres types de cyberattaques.


## La cybersécurité en quelques chiffres

+ Des **milliers de cyberattaques** recensées chaque année.
+ Des **centaines de millions de victimes** touchées.
+ Le coût moyen d'une **violation de données** atteint plusieurs millions de dollars par incident.
+ Le **courrier électronique** demeure vecteur le plus courant de diffusion des logiciels malveillants.
+ - Le coût total des dommages causés par la cybercriminalité continue de croître d'année en année, se chiffrant en **milliers de milliards de dollars** à l'échelle mondiale.
+ Les emplois en sécurité de l'information : parmi les plus en demande en technologies de l'information.

<!-- 📊 Illustration suggérée : graphique montrant l'évolution du coût mondial de la cybercriminalité année par année. -->

{{% notice tip "Atelier" %}} Rechercher les statistiques les plus récentes (rapports IBM Cost of a Data Breach). {{% /notice %}}

Ces chiffres alarmants soulignent le danger que représentent les vulnérabilités cybernétiques et mettent en évidence la nécessité de disposer de professionnels compétents en matière de cybersécurité.

## Les cyberattaques les plus courantes

### L'hameçonnage (*phishing*)
+ Utilise des courriels, textos ou sites web trompeurs.
+ L'objectif de l'attaquant est d'inciter la victime à :
    + télécharger un logiciel malveillant ;
    + divulguer des informations sensibles.
+ L'attaquant se fait passer pour une personne ou une organisation légitime.
+ C'est le point de départ d'une large majorité des attaques de prise de contrôle de comptes.

{{< figure
  src="/images/01-phishing.png"
  alt="Exemple de courriel d'hameçonnage"
  caption="Figure 1.1 - Exemple de courriel d'hameçonnage"
  width=600
>}}
<!-- capture d'écran d'un courriel de phishing typique (fausses URL, urgence artificielle, faute d'orthographe). -->

### Logiciels malveillants (*malware*)
+ En forte croissance, surtout les **rançongiciels** (*ransomware*).
+ Fonctionnement :
  1. chiffrement des données de la victime ;
  2. demande de rançon en échange de la clé de déchiffrement.
+ Coût moyen pour une entreprise : plusieurs millions de dollars (pertes d'exploitation + remise en état + rançon éventuelle).

### Déni de service distribué (DDoS)
{{< figure
  src="/images/01-ddos-botnet.png"
  alt="Exemple de courriel d'hameçonnage"
  caption="Figure 1.2 - Représentation d'une attaque DDoS"
  width=400
>}}
+ Plusieurs appareils compromis (*botnet*) inondent une cible de trafic.
+ Objectif : dépasser la capacité de traitement de la cible.
+ Résultat : le service devient inaccessible aux utilisateurs légitimes.

### Violations de données personnelles
+ Porte d'entrée classique vers l'**usurpation d'identité**.
+ Méthodes utilisées : hameçonnage, malware, exploitation de vulnérabilités.
+ Données ciblées : numéros d'assurance sociale, identifiants, informations financières.

## Études de cas connus

### Sony PlayStation Network (2011)

En avril 2011, le PlayStation Network, service en ligne de Sony, tombe en panne pendant plusieurs semaines pour des dizaines de millions d'utilisateurs à travers le monde. Sony finit par admettre qu'une intrusion a eu lieu sur ses serveurs et que des millions de données personnelles ont été volées, incluant des mots de passe et des historiques de paiement stockés de manière **non chiffrée**. L'entreprise a mis **plus de deux mois** à assainir ses serveurs.

### Yahoo
L'une des plus grandes violations de données de l'histoire : plus de **3 milliards de comptes d'utilisateurs** Yahoo ont été compromis. Les pirates ont ciblé la base de données de l'entreprise pour voler les enregistrements des comptes.

### L'attaque DDoS contre GitHub (2018)
En février 2018, GitHub a été victime de l'une des plus importantes attaques DDoS jamais enregistrées à l'époque, avec un pic de trafic dépassant **1 Tb/s**. Les attaquants ont exploité un logiciel de cache mal configuré (memcached), normalement destiné à accélérer l'accès aux bases de données, mais capable de démultiplier le volume d'une requête. Grâce à ses serveurs de sauvegarde, GitHub a pu rétablir le service en quelques minutes, sans dégât matériel majeur.

### Desjardins
Une fuite de données interne (plutôt qu'une attaque externe classique) ayant compromis les informations personnelles de millions de membres, illustrant que la menace interne (employé malveillant ou négligent) est tout aussi réelle que la menace externe.
