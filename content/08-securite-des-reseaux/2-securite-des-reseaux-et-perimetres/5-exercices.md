+++
title = 'Exercices'
weight = '825'
draft = false
+++
---------

### 1. Règles de pare-feu

Une PME possède un réseau interne (LAN `192.168.10.0/24`) et une DMZ (`192.168.20.0/24`) contenant un serveur Web `192.168.20.10` qui doit être accessible depuis Internet en HTTP et HTTPS.
 
L'administrateur a configuré les règles suivantes sur l'interface WAN (lues de haut en bas) :
 
| # | Action | Protocole | Source | Destination | Port |
|---|---|---|---|---|---|
| 1 | Block | any | any | any | any |
| 2 | Pass | TCP | any | 192.168.20.10 | 443 |
| 3 | Pass | TCP | any | 192.168.20.10 | 80 |
 
1. Les clients signalent que le site Web est inaccessible. Expliquez pourquoi.
2. Réécrivez la table de règles pour corriger le problème.
3. L'administrateur souhaite se connecter en SSH au serveur Web **uniquement** depuis son poste `192.168.10.5`. Écrivez la règle à ajouter, et précisez sur quelle interface.
4. Pourquoi placer le serveur Web dans une DMZ plutôt que dans le LAN ?
5. Faut-il ajouter une règle pour autoriser les **réponses** du serveur Web vers les clients d'Internet ? Justifiez.


<!-- {{% expand "Solution" %}}
1. Les règles sont lues dans l'ordre et la première qui correspond s'applique. La règle 1 bloque **tout** le trafic : les règles 2 et 3 ne sont jamais atteintes.
2. Table corrigée :
   | # | Action | Protocole | Source | Destination | Port |
   |---|---|---|---|---|---|
   | 1 | Pass | TCP | any | 192.168.20.10 | 443 |
   | 2 | Pass | TCP | any | 192.168.20.10 | 80 |
   | 3 | Block | any | any | any | any |
3. Sur l'interface **LAN** (le trafic entre dans le pare-feu par le LAN) : `Pass | TCP | 192.168.10.5 | 192.168.20.10 | 22`. Elle doit être placée avant toute règle qui bloquerait le trafic du LAN vers la DMZ.
4. Le serveur Web est la machine la plus exposée. S'il est compromis, la DMZ empêche l'attaquant d'accéder directement aux postes et aux serveurs du réseau interne (les flux DMZ → LAN sont bloqués).
5. Non. Le pare-feu est **avec état** : les réponses appartenant à une connexion déjà autorisée sont acceptées automatiquement.
{{% /expand %}} -->



### 2. Faux positif ou faux négatif ?
Classez chaque situation : **vrai positif**, **faux positif**, **vrai négatif** ou **faux négatif**.
 
1. L'IDS génère une alerte « injection SQL » au moment où un attaquant teste `' OR '1'='1` sur le formulaire de connexion.
2. Chaque nuit, l'IDS génère une alerte « exfiltration de données » pendant la sauvegarde légitime vers le serveur de stockage.
3. Un attaquant exploite une vulnérabilité découverte la veille, pour laquelle aucune signature n'existe. Aucune alerte n'est générée.
4. Un employé consulte le site du cégep. Aucune alerte n'est générée.
5. Un IPS bloque l'accès d'un employé à un site légitime de documentation technique, parce que son nom ressemble à celui d'un site malveillant.

<!-- {{% expand "Solution" %}}
1. Vrai positif
2. Faux positif
3. Faux négatif
4. Vrai négatif
5. Faux positif (avec un IPS, le faux positif entraîne un blocage de trafic légitime)
{{% /expand %}} -->



<!-- ### 3. Périmètre ou Zero Trust ?

Une employée clique sur un lien d'hameçonnage. Un logiciel malveillant s'installe sur son portable, connecté au VPN de l'entreprise. L'attaquant cherche ensuite à atteindre le serveur de paie.

1. Décrivez ce qui se passe dans une architecture **périmétrique** classique (VPN donnant accès à tout le réseau interne).
2. Décrivez trois mécanismes d'une architecture **Zero Trust** qui pourraient arrêter ou limiter l'attaque.

{{% expand "Solution" %}}
1. Le portable compromis est « à l'intérieur » grâce au VPN. L'attaquant peut balayer le réseau interne, découvrir le serveur de paie et tenter de l'attaquer directement : rien ne distingue son trafic de celui d'un employé légitime.
2. Au choix :
   - **Moindre privilège** : l'employée n'a accès qu'aux applications nécessaires à son travail ; si elle ne travaille pas à la paie, le serveur de paie lui est tout simplement inaccessible.
   - **Vérification de l'état de l'appareil** : l'EDR détecte le logiciel malveillant, et l'appareil, jugé non conforme, perd ses accès.
   - **Microsegmentation** : le serveur de paie n'accepte que les connexions de l'application de paie, pas celles de n'importe quel poste du réseau.
   - **Surveillance et détection d'anomalies** : le balayage du réseau depuis un portable d'employée est inhabituel et génère une alerte (IDS, SIEM).
   - **Réauthentification** (MFA) pour accéder aux ressources sensibles : l'attaquant ne possède pas le second facteur.
{{% /expand %}} -->
