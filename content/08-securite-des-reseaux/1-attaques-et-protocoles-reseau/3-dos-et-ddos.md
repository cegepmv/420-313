+++
title = 'Déni de service (DoS / DDoS)'
weight = '813'
draft = false
+++
---------

Une attaque par **déni de service** (*Denial of Service*, **DoS**) vise à rendre un service **indisponible** pour ses utilisateurs légitimes. Elle s'attaque au **A** de **CIA** : la disponibilité (*Availability*).

Lorsque l'attaque provient d'un grand nombre de machines à la fois, on parle de déni de service **distribué** (**DDoS**). Il est alors impossible de la bloquer simplement en filtrant une adresse IP.

## Trois catégories d'attaques

| Catégorie | Ressource épuisée | Exemple |
|---|---|---|
| **Volumétrique** | La **bande passante** du lien réseau | ICMP flood, amplification |
| **Protocolaire** | Les ressources des **équipements** (tables de connexions, mémoire) | SYN flood |
| **Applicative** | Les ressources de l'**application** (processeur, base de données) | Des milliers de requêtes HTTP vers une page de recherche coûteuse |

### Inondation ICMP (ICMP flood)

L'attaquant envoie un très grand nombre de requêtes **ICMP Echo Request** (`ping`) à la cible, qui voit sa bande passante saturée et consomme des ressources pour répondre. Une seule machine suffit rarement : l'attaque devient efficace lorsqu'elle est **distribuée** sur des milliers de machines.

### Inondation SYN (SYN flood)

Pour ouvrir une connexion TCP, le client envoie un **SYN**, le serveur répond **SYN/ACK** puis attend un **ACK** final. Pendant cette attente, le serveur garde la connexion **semi-ouverte** dans une file d'attente de taille limitée.

![SYN flood : l'attaquant envoie des SYN avec des adresses usurpées ; le serveur attend des réponses qui n'arrivent jamais et sa file se remplit](/images/08-syn-flood.png)

Dans un **SYN flood**, l'attaquant envoie un grand nombre de SYN avec des **adresses usurpées** et ne termine jamais la poignée de main. La file du serveur se remplit, et celui-ci **refuse les nouvelles connexions**, y compris celles des clients légitimes.

{{%notice style="tip" title="Contre-mesure : les SYN cookies"%}}
La principale contre-mesure, les **SYN cookies**, permet au serveur de ne plus rien réserver à la réception d'un SYN tant que la connexion n'est pas confirmée. Elle est activée par défaut sous Linux.
{{%/notice%}}

### Amplification

Dans une attaque par **amplification**, l'attaquant envoie de petites requêtes **UDP** à des serveurs légitimes mal configurés (DNS, memcached...), en **usurpant l'adresse IP de la victime** (section 8.1.1). Les serveurs envoient leurs réponses, **beaucoup plus volumineuses** que les requêtes, à la victime.

![Amplification : l'attaquant envoie de petites requêtes usurpées à des serveurs ouverts, qui renvoient des réponses volumineuses à la victime](/images/08-amplification.png)

L'attaquant **multiplie** ainsi sa puissance : avec une requête qui génère une réponse 50 fois plus grosse, 1 Gbit/s envoyé par l'attaquant devient 50 Gbit/s reçus par la victime.

{{%notice style="tip" title="Retour sur un cas du chapitre 1 : GitHub (2018)"%}}
Au chapitre 1, nous avons étudié l'attaque DDoS de 2018 contre **GitHub**, qui a dépassé 1 Tbit/s. Vous pouvez maintenant en comprendre le mécanisme : c'était une attaque par **amplification**. Les attaquants envoyaient de petites requêtes, avec l'adresse de GitHub comme source usurpée, à des serveurs **memcached** laissés ouverts sur Internet. Pour ce protocole, une réponse peut être des **dizaines de milliers de fois** plus grosse que la requête.

La leçon vaut aussi pour les défenseurs : un service mal configuré et exposé sur Internet peut servir d'arme contre **d'autres** organisations.
{{%/notice%}}

## Les botnets

Un **botnet** est un réseau de machines **compromises** (ordinateurs, serveurs, objets connectés) contrôlées à distance par un attaquant, souvent à l'insu de leurs propriétaires (logiciels malveillants, chapitre 6). Les botnets sont le principal moyen de lancer des attaques DDoS, et ils sont souvent **loués** à d'autres criminels.

![Botnet : l'attaquant envoie un ordre au serveur de commande, qui le relaie à des milliers d'appareils compromis (ordinateurs, caméras IP, routeurs) ; tous attaquent la victime en même temps](/images/08-botnet.png)

{{%notice style="info" title="Cas réel : Mirai (2016)"%}}
Le botnet **Mirai** a infecté des centaines de milliers d'**objets connectés** (caméras IP, enregistreurs vidéo) en essayant simplement leurs **mots de passe par défaut**... En octobre 2016, il a lancé une attaque DDoS contre **Dyn**, un important fournisseur DNS. Pendant plusieurs heures, de nombreux sites majeurs (Twitter, Netflix...) sont devenus inaccessibles pour une partie des internautes.
{{%/notice%}}

## Contre-mesures

| Contre-mesure | Principe |
|---|---|
| **Services d'atténuation DDoS**  | Le trafic passe d'abord par d'immenses centres de filtrage répartis dans le monde, qui absorbent le volume et ne transmettent que le trafic légitime. |
| **Limitation de débit** (*rate limiting*) | Les pare-feu, routeurs et applications limitent le nombre de requêtes acceptées par seconde. |
| **SYN cookies** | Protègent contre le SYN flood. |
| **Plan de réponse aux incidents** | Savoir quoi faire, qui appeler et comment communiquer pendant l'attaque. |


{{%notice style="tip" title="Pour aller plus loin : d'autres attaques réseau"%}}
- **Injection TCP RST** : un attaquant qui voit le trafic fabrique un faux segment TCP avec le drapeau **RST**, qui coupe immédiatement une connexion, même chiffrée (HTTPS, SSH). Le chiffrement protège les données, pas la disponibilité de la connexion.
- **Empoisonnement du cache DNS** : un attaquant injecte une fausse réponse dans le cache d'un serveur DNS, qui la redistribue ensuite à **tous** ses clients. La contre-mesure à long terme est **DNSSEC**, qui signe les réponses DNS (chapitre 4).
<!-- - **Saut de VLAN** (*VLAN hopping*) : techniques permettant d'envoyer des trames dans un VLAN auquel l'attaquant n'appartient pas, en exploitant une mauvaise configuration des commutateurs.
- **Détournement BGP** : un opérateur annonce, par erreur ou volontairement, qu'il est le meilleur chemin vers des adresses IP qui ne lui appartiennent pas. Une partie du trafic Internet mondial est alors détournée. -->

Ces attaques sont mentionnées pour les personnes désirant approfondir. Il ne sera pas demandé de les connaître pour les examens.
{{%/notice%}}