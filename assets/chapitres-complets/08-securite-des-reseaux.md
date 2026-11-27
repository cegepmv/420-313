# 8. Sécurité des réseaux
---------

Les chapitres précédents ont porté sur la protection des **données** (chapitre 4), des **identités** (chapitre 5), des **postes de travail** (chapitre 6) et des **applications** (chapitre 7). Ce chapitre s'intéresse au **chemin entre tous ces éléments** : le réseau.

La première partie présente les principales **attaques** qui exploitent les faiblesses des protocoles réseau. La deuxième partie présente les **mécanismes de défense** qui permettent de protéger un réseau et son périmètre (pare-feu, segmentation, VPN, IDS/IPS, Zero Trust).

## Pourquoi les réseaux sont vulnérables

Les protocoles de base d'Internet (**ARP**, **IP**, **TCP**, **UDP**) ont été conçus dans les années 1970-1980, pour relier un petit nombre d'universités et de centres de recherche qui se faisaient **mutuellement confiance**. La sécurité n'était pas un objectif de conception.

Conséquence : la plupart de ces protocoles **n'authentifient pas l'expéditeur**. Un ordinateur qui reçoit un paquet ne peut pas vérifier que l'adresse source indiquée est la vraie.

{{%notice style="info" title="Rappel du chapitre 7 : ne jamais faire confiance au client"%}}
Au chapitre 7, nous avons vu qu'une application ne doit jamais faire confiance aux données envoyées par le navigateur. Le même principe s'applique au réseau : **tous les champs d'un paquet (adresses, ports...) peuvent être fabriqués de toutes pièces** par un attaquant.
{{%/notice%}}

## Rappel : le modèle TCP/IP

Vous avez étudié le modèle TCP/IP dans le cours *Conception de réseaux informatiques*. Le tableau suivant associe chaque couche aux attaques étudiées dans ce chapitre.

| Couche | Protocoles | Attaques vues dans ce chapitre |
|---|---|---|
| **Accès réseau** (liaison) | Ethernet, Wi-Fi, ARP | Usurpation MAC, **empoisonnement ARP** |
| **Internet** | IP, ICMP | **Usurpation IP**, **ICMP flood** |
| **Transport** | TCP, UDP | **SYN flood**, **amplification UDP** |
| **Application** | HTTP, DNS, SSH... | **Homme du milieu**, DoS applicatif |

{{%notice style="tip" title="Sécurité et CIA"%}}
Chaque attaque de ce chapitre vise au moins un des trois objectifs **CIA** (chapitre 2) :
- l'**écoute** et l'**homme du milieu** visent la **confidentialité** (et l'**intégrité** si les données sont modifiées) ;
- le **déni de service** vise la **disponibilité**.
{{%/notice%}}

# 8.1. Attaques et protocoles réseau
---------

Cette section présente trois grandes familles d'attaques réseau à connaître : l'**écoute et l'usurpation**, l'**homme du milieu** et le **déni de service**.

# 8.1.1. Écoute et usurpation
---------

## Capture de paquets

Vous avez déjà utilisé **Wireshark** dans le cours *Conception de réseaux informatiques* pour capturer et analyser des paquets. Un attaquant peut utiliser exactement les mêmes outils.

Sur un réseau moderne, chaque poste est relié à un **commutateur** (*switch*), qui envoie une trame **uniquement** au poste destinataire. Un poste ne voit donc normalement que **son propre trafic**. Pour intercepter le trafic des autres, l'attaquant doit d'abord **détourner** les trames vers lui : c'est le but de l'**empoisonnement ARP** (section 8.1.2).

Une fois le trafic détourné, tout ce qui circule **en clair** peut être lu : un mot de passe ou un cookie de session envoyé en **HTTP** apparaît directement dans Wireshark. Au chapitre 7, vous avez volé un cookie `PHPSESSID` en le copiant dans le navigateur ; un attaquant qui écoute le réseau peut l'obtenir **sans aucune interaction** avec la victime.

## Usurpation (spoofing)

L'**usurpation** (*spoofing*) consiste à **falsifier l'identité de l'émetteur** dans un paquet, afin de se faire passer pour une autre machine ou de cacher sa propre identité.

- **Usurpation d'adresse MAC** : le système d'exploitation peut envoyer une autre adresse MAC que celle de sa carte réseau. <!--Un **filtrage par adresse MAC** (par exemple sur un réseau Wi-Fi) se contourne donc facilement : il suffit de copier une adresse autorisée.-->
- **Usurpation d'adresse IP** : l'adresse source est un simple champ de l'en-tête IP, rempli par l'émetteur lui-même. Rien ne garantit qu'elle est vraie.

![Usurpation IP : l'attaquant envoie un paquet avec la source de la victime ; la réponse du serveur part vers la victime, pas vers l'attaquant](/images/08-usurpation-ip.png)

Une conséquence importante : **la réponse est envoyée à l'adresse usurpée**, pas à l'attaquant. L'attaquant ne peut donc pas facilement établir une vraie connexion avec une fausse adresse, mais il peut diriger des réponses vers une victime : c'est le principe de l'**amplification** (section 8.1.3).

{{%notice style="tip" title="Usurpation et authentification"%}}
Une application qui accorde des droits **sur la seule base de l'adresse IP source** fait confiance à une information que l'attaquant contrôle. On retrouve la distinction du chapitre 5 : une adresse IP **identifie**, elle n'**authentifie** pas.
{{%/notice%}}

### Contre-mesures

- **Filtrer les adresses incohérentes** : un pare-feu rejette, par exemple, un paquet qui **arrive d'Internet** avec une adresse source **privée** (`10.x.x.x`, `192.168.x.x`...), puisqu'il est forcément falsifié.
- **Authentifier avec la cryptographie** (chapitre 4) : TLS, SSH et les VPN identifient l'interlocuteur avec des **clés**, et non avec son adresse.

# 8.1.2. Empoisonnement ARP et homme du milieu
---------

## Rappel : le protocole ARP

Pour envoyer une trame sur un réseau local, un poste doit connaître l'adresse **MAC** qui correspond à l'adresse **IP** du destinataire. Le protocole **ARP** fait cette association : le poste demande à tout le réseau « Qui a l'adresse `10.10.50.10` ? », la machine concernée répond avec son adresse MAC, et le poste enregistre la réponse dans sa **table ARP**.

Problème : ARP n'a **aucun mécanisme d'authentification**. N'importe quel poste peut envoyer une réponse ARP, même si personne ne lui a rien demandé, et le poste qui la reçoit **met à jour sa table** sans vérification.

## L'empoisonnement ARP

L'**empoisonnement ARP** (*ARP poisoning*) consiste à envoyer de fausses réponses ARP pour associer **l'adresse IP d'une autre machine à l'adresse MAC de l'attaquant**.

![Empoisonnement ARP : tables ARP de la victime et du serveur avant et après l'attaque ; le trafic passe désormais par l'attaquant](/images/08-arp-empoisonnement.png)

Par exemple, l'attaquant annonce :
- à la **victime** : « l'adresse IP du serveur correspond à **ma** MAC » ;
- au **serveur** : « l'adresse IP de la victime correspond à **ma** MAC ».

Les deux machines envoient alors leurs trames **à l'attaquant**, en croyant les envoyer à l'autre. Dans la table ARP de la victime, on observe un symptôme typique : **deux adresses IP différentes associées à la même adresse MAC** (celle de l'attaquant).

{{%notice style="warning" title="Une attaque limitée au réseau local"%}}
ARP ne fonctionne qu'à l'intérieur d'un **même réseau local**. L'attaquant doit donc déjà être **connecté au réseau** : poste compromis (chapitre 6), employé malveillant, Wi-Fi public ou mal protégé. C'est une raison de plus pour **segmenter** le réseau (section 8.2.1).
{{%/notice%}}

## L'attaque de l'homme du milieu (MITM)

Si l'attaquant **réachemine** ensuite les trames vers leur vrai destinataire, la communication continue de fonctionner normalement et les victimes ne remarquent rien. L'attaquant est devenu un **homme du milieu** (*Man-in-the-Middle*, **MITM**) : il s'est inséré entre deux interlocuteurs qui croient communiquer directement.

![Attaque de l'homme du milieu : la victime et le serveur croient communiquer directement, mais tout le trafic passe par l'attaquant](/images/08-mitm.png)

Une fois au milieu, l'attaquant peut :
- **lire** les échanges non chiffrés (atteinte à la **confidentialité**) ;
- **modifier** les échanges (atteinte à l'**intégrité**) ;
- **bloquer** les échanges (atteinte à la **disponibilité**).

L'empoisonnement ARP n'est pas le seul moyen de se placer au milieu. Un attaquant peut aussi :
- envoyer une **fausse réponse DNS**, qui dirige la victime vers un faux site ;
- créer un **faux point d'accès Wi-Fi** portant le même nom qu'un réseau légitime (« Cafe-Gratuit ») ;
- prendre le contrôle d'un **équipement** par lequel le trafic passe déjà (routeur compromis).

{{%notice style="info" title="Vous avez déjà rencontré cette attaque"%}}
- Au **chapitre 4** (« Problème 1 » de la cryptographie à clé publique) : un attaquant qui substitue sa propre clé publique à celle du destinataire peut lire tous les messages. C'est ce que les **certificats** permettent d'éviter.
- Au **chapitre 7** : le vol de session par **écoute du réseau** lorsque le site utilise HTTP.
{{%/notice%}}

## Contre-mesures

| Mesure | Principe |
|---|---|
| **TLS et validation des certificats** (chapitre 4) | Même si le trafic passe par l'attaquant, il ne peut ni le lire ni le modifier. S'il présente son propre certificat, le navigateur affiche un **avertissement**. |
| **Ne jamais ignorer un avertissement de certificat** | Au laboratoire 5.5 du chapitre 4, l'avertissement était normal (certificat autosigné). Sur un site public, un avertissement inattendu peut signaler une attaque en cours. |
| **VPN** (section 8.2.2) | Chiffre tout le trafic sur les réseaux non fiables (Wi-Fi public). |
| **Protection des commutateurs** et **segmentation** | Certains commutateurs vérifient les réponses ARP et rejettent les fausses (*Dynamic ARP Inspection*) ; la segmentation réduit le nombre de victimes possibles. |
| **HSTS** | Le serveur demande au navigateur de **toujours** utiliser HTTPS pour ce site, ce qui empêche l'attaquant de forcer une connexion en HTTP. |

# 8.1.3. Déni de service (DoS / DDoS)
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

Ces attaques sont mentionnées pour les personnes désirant approfondir. Il ne sera pas demandé de les connaître pour les examens.
{{%/notice%}}

# 8.1.4. Exercices
---------

### 1. Identifiez l'attaque
Pour chaque situation, identifiez l'attaque : **empoisonnement ARP**, **homme du milieu**, **SYN flood**, **ICMP flood (DDoS)** ou **amplification**.

1. Un poste reçoit toutes les deux secondes des réponses ARP non sollicitées indiquant que l'adresse IP de la passerelle correspond à une nouvelle adresse MAC.
2. Un serveur Web conserve des centaines de connexions semi-ouvertes provenant d'adresses IP variées. Les clients légitimes n'arrivent plus à se connecter.
3. Des serveurs DNS ouverts reçoivent de petites requêtes dont l'adresse source est celle d'une entreprise. L'entreprise reçoit d'énormes réponses DNS qu'elle n'a jamais demandées.
4. Connecté au Wi-Fi gratuit d'un café, un utilisateur reçoit un avertissement de certificat en ouvrant le site de sa banque, qui n'affiche jamais d'avertissement d'habitude.
5. Un serveur reçoit des milliers de requêtes `ping` par seconde, provenant de centaines de caméras IP réparties dans le monde.

{{% expand "Solution" %}}
1. Empoisonnement ARP (probablement pour préparer une attaque de l'homme du milieu)
2. SYN flood
3. Amplification (DNS), avec usurpation de l'adresse IP de l'entreprise
4. Homme du milieu (l'attaquant présente son propre certificat ; il ne faut surtout pas accepter l'avertissement)
5. ICMP flood distribué (DDoS) lancé par un botnet d'objets connectés
{{% /expand %}}


# 8.2. Sécurité des réseaux et périmètres
---------

La section précédente a montré que les protocoles réseau, à eux seuls, ne protègent pas contre les attaques. Cette section présente les mécanismes qui permettent de **contrôler**, **protéger** et **surveiller** les communications d'un réseau d'entreprise.

On retrouve le principe de **défense en profondeur** : aucun mécanisme n'est suffisant seul, on les **superpose** pour qu'une défaillance de l'un soit compensée par les autres.

# 8.2.1. Pare-feu et segmentation
---------

## Qu'est-ce qu'un pare-feu ?
 
Un **pare-feu** (*firewall*) est un équipement ou un logiciel qui **contrôle les flux réseau** entre plusieurs zones, en autorisant ou en bloquant chaque paquet selon un ensemble de **règles**.
 
Un pare-feu peut être :
- **réseau** : un équipement placé entre plusieurs réseaux, par exemple entre le réseau de l'entreprise et Internet. C'est le cas de **PfSense**, utilisé dans le TP2 ;
- **local** : un logiciel installé sur un poste ou un serveur, qui ne protège que cette machine (pare-feu Windows, `ufw` et `firewalld` sur Linux, chapitre 6).

## Sans état ou avec état ?
 
| Type | Ce qu'il examine | Conséquence |
|---|---|---|
| **Sans état** (*stateless*) | Chaque paquet **isolément** : adresses, protocole, ports | Il faut écrire une règle pour l'aller **et** une règle pour le retour |
| **Avec état** (*stateful*) | Les paquets **et l'état des connexions** en cours | Les **réponses** à une connexion autorisée sont acceptées automatiquement |
 
Presque tous les pare-feu modernes, dont *PfSense*, sont **avec état** : une seule règle « le LAN peut joindre le serveur Web sur le port 80 » suffit, les réponses du serveur sont reconnues et acceptées.
 

{{%notice style="tip" title="NGFW et WAF"%}}
Certains pare-feu vont plus loin et examinent aussi le **contenu** du trafic : 
+ les pare-feu de **nouvelle génération** (NGFW) reconnaissent les applications et intègrent souvent un IPS (section 8.2.3)
+ les pare-feu applicatifs Web (**WAF**) filtrent les requêtes HTTP vers une application (injections SQL, XSS, chapitre 7).
{{%/notice%}}

## Les règles de pare-feu
 
Une règle de pare-feu précise généralement :
 
| Champ | Exemple |
|---|---|
| **Action** | Autoriser (*pass*), bloquer silencieusement (*block*), rejeter en avertissant l'émetteur (*reject*) |
| **Interface** et **direction** | Trafic qui **entre** dans le pare-feu par l'interface LAN |
| **Protocole** | TCP, UDP, ICMP, tous |
| **Source** | Une adresse, un réseau (`LAN net`), n'importe laquelle (`any`) |
| **Destination** et **port** | `172.16.20.10`, port `443` |
| **Description** | « Autoriser l'accès HTTPS au serveur Web » |
 
Deux principes fondamentaux :
 
1. **Les règles sont lues dans l'ordre, et la première qui correspond s'applique.** Une règle de blocage placée **avant** une règle d'autorisation l'empêche de s'appliquer.
2. **Refus par défaut** (*default deny*) : tout ce qui n'est pas **explicitement autorisé** est **interdit**. On écrit des règles pour les flux nécessaires, et une dernière règle (souvent implicite) bloque tout le reste. C'est le même principe que <!--la liste de ce qui est accepté pour la validation des entrées (chapitre 7) et que--> le **moindre privilège** (chapitre 5).

**Exemple de table de règles pour l'interface WAN d'une petite entreprise :**
 
| # | Action | Protocole | Source | Destination | Port | Description |
|---|---|---|---|---|---|---|
| 1 | Pass | TCP | any | 172.16.20.10 | 443 | Site Web public (HTTPS) |
| 2 | Pass | UDP | any | WAN address | 51820 | Accès VPN des employés |
| 3 | Block | any | any | any | any | Refus par défaut |
 
Le trafic **sortant** doit lui aussi être filtré : un logiciel malveillant installé sur un poste (chapitre 6) cherche à contacter son serveur de commande ou à exfiltrer des données.
 
## La segmentation
 
Dans le cours *Conception de réseaux informatiques*, vous avez appris à découper un réseau en **sous-réseaux** (préfixes CIDR, VLSM). Par exemple, une entreprise qui possède `192.168.100.0/24` peut créer un sous-réseau pour l'administration (`192.168.100.0/26`), un pour le développement (`192.168.100.64/27`), un pour la formation (`192.168.100.96/28`), etc.
 
Ce découpage n'est pas seulement une question d'organisation des adresses : c'est aussi une **mesure de sécurité**.
 
| Apport de la segmentation | Explication |
|---|---|
| **Un point de contrôle entre les zones** | Pour passer d'un sous-réseau à l'autre, le trafic traverse un **routeur**. Si ce routeur est un **pare-feu**, on peut décider précisément quels flux sont autorisés entre les zones. |
| **Une attaque locale reste locale** | Les attaques de la couche liaison, comme l'**empoisonnement ARP** (section 8.1.2), ne fonctionnent qu'à l'intérieur d'un même réseau local. Plus les segments sont petits, moins il y a de victimes possibles. |
| **Moins de mouvement latéral** | Si un poste est compromis, l'attaquant ne peut pas atteindre directement les serveurs ou les autres services : il doit franchir le pare-feu. |
 
Deux techniques complètent ce découpage :
- les **VLAN** (*Virtual LAN*) créent plusieurs réseaux logiques **isolés** sur les mêmes commutateurs physiques (par exemple, les postes, les serveurs, les objets connectés et le Wi-Fi des invités) ;
- la **microsegmentation** pousse la logique jusqu'à isoler chaque serveur ou chaque application, avec ses propres règles. Elle est au cœur de l'architecture **Zero Trust** (section 8.2.4).
### La zone démilitarisée (DMZ)
 
Une **DMZ** est un segment qui héberge les serveurs **accessibles depuis Internet** (site Web, serveur de courriel...), isolé du réseau interne.
 
![Architecture avec DMZ : Internet, pare-feu, réseau interne (LAN) et DMZ contenant le serveur Web ; flèches des flux autorisés et bloqués](/images/08-dmz.png)
 
| Flux | Autorisé ? | Raison |
|---|---|---|
| Internet → DMZ (port 443) | ✅ | Le site Web doit être public |
| LAN → DMZ | ✅ (ports limités) | Les employés consultent et administrent le serveur |
| DMZ → LAN | ❌ | **Si le serveur Web est compromis**, l'attaquant ne doit pas pouvoir rebondir vers le réseau interne |
| Internet → LAN | ❌ | Le réseau interne n'est jamais exposé directement |
 
Le serveur Web est la machine **la plus exposée** de l'entreprise, donc la plus susceptible d'être compromise (par exemple par une injection SQL, chapitre 7). La DMZ permet de **contenir** les conséquences d'une telle compromission.
 
# 8.2.2. VPN et tunnels sécurisés
---------


## Qu'est-ce qu'un VPN ?
 
Un **réseau privé virtuel** (*Virtual Private Network*, **VPN**) crée un **tunnel chiffré** à travers un réseau non fiable (généralement Internet) entre deux points. Les machines situées aux deux extrémités communiquent **comme si elles étaient sur le même réseau privé**.
 
Le principe est l'**encapsulation** : chaque paquet est **chiffré**, puis placé **à l'intérieur** d'un autre paquet qui traverse Internet. Un observateur situé sur le chemin ne voit que des paquets chiffrés échangés entre les deux extrémités du tunnel.
 
![Tunnel VPN : le paquet original (vers 172.16.10.20) est chiffré et encapsulé dans un paquet UDP entre l'employé et la passerelle VPN](/images/08-vpn-tunnel.png)
 
Un VPN assure la **confidentialité** et l'**intégrité** des échanges (chiffrement symétrique, chapitre 4), ainsi que l'**authentification** des deux extrémités (clés ou certificats, chapitres 4 et 5).
 
On distingue deux usages :
- **site à site** : relie **deux réseaux** entiers de façon permanente (le siège social et une succursale) ;
- **accès distant** : relie **un appareil** au réseau de l'entreprise (un employé en télétravail).
 
Les principaux protocoles VPN sont **IPsec** (le standard historique, très utilisé entre sites), **OpenVPN** (basé sur TLS et les certificats) et **WireGuard**, plus récent, qui s'est rapidement imposé.
 
## Tunnel complet ou tunnel partagé
 
Lorsqu'un employé se connecte au VPN, deux configurations sont possibles :
 
- **Tunnel complet** (*full tunnel*) : **tout** le trafic de l'appareil passe par le VPN, y compris la navigation sur Internet. L'entreprise peut filtrer ce trafic, mais la passerelle VPN doit supporter toute cette charge.
- **Tunnel partagé** (*split tunneling*) : **seul** le trafic destiné au réseau de l'entreprise passe par le VPN ; le reste passe directement par la connexion Internet de l'employé. Plus performant, mais l'appareil reste exposé directement à Internet.

{{%notice style="tip" title="AllowedIPs sur WireGuard"%}}
Avec WireGuard, ce choix se fait avec le paramètre `AllowedIPs` du client (TP2).
{{%/notice%}}

## Limites des VPN
 
{{%notice style="warning" title="Ce qu'un VPN ne fait pas"%}}
- **Un VPN ne protège pas un poste compromis.** Si l'appareil de l'employé contient un logiciel malveillant, celui-ci profite aussi du tunnel pour atteindre le réseau de l'entreprise.
- **La passerelle VPN est une cible de choix.** Exposée sur Internet, elle donne accès au réseau interne. Des vulnérabilités critiques dans des passerelles VPN de grands fournisseurs ont été massivement exploitées par des groupes de rançongiciels : **correctifs** (chapitre 6) et **MFA** (chapitre 5) sont indispensables.
{{%/notice%}}

# 8.2.3. IDS et IPS
---------

Un pare-feu décide selon des **adresses** et des **ports**. Mais une attaque peut très bien passer par un port autorisé : une injection SQL (chapitre 7) arrive sur le port 443 du serveur Web, comme n'importe quelle requête légitime.
 
Les systèmes de **détection** (*Intrusion Detection System* ou **IDS**) et de **prévention d'intrusion** (*Intrusion Prevention System* ou **IPS**) analysent le **contenu** et le **comportement** du trafic pour repérer les attaques.
 
## IDS ou IPS ?
 
| | **IDS** (*Intrusion Detection System*) | **IPS** (*Intrusion Prevention System*) |
|---|---|---|
| Rôle | **Détecter** et **alerter** | Détecter et **bloquer** |
| Position | **À côté** du trafic : il en reçoit une **copie** | **Dans** le trafic : chaque paquet passe par lui |
| En cas d'attaque | Génère une alerte ; un humain doit réagir | Supprime le paquet et peut bloquer la connexion |
| Risque d'un faux positif | Une alerte inutile | **Du trafic légitime bloqué** |
 
![IDS hors ligne recevant une copie du trafic vs IPS en ligne entre le pare-feu et le réseau](/images/08-ids-vs-ips.png)
 
+ Un IDS **réseau** (NIDS, comme **Suricata** ou **Snort**) analyse le trafic d'un segment entier ; 
+ un IDS **d'hôte** (HIDS) surveille une seule machine, comme les solutions EDR vues au chapitre 6.
 
## Méthodes de détection
 
| Méthode | Principe | Avantages | Limites |
|---|---|---|---|
| **Par signature** | Comparer le trafic à une base de **motifs d'attaques connues** (comme un antivirus, chapitre 6) | Précis, peu de faux positifs | Ne détecte pas les attaques **nouvelles** (*0-Day*); la base doit être mise à jour |
| **Par anomalie** | Apprendre le comportement **normal** du réseau et signaler ce qui s'en écarte | Peut détecter des attaques inconnues | Plus de faux positifs ; demande une période d'apprentissage |
 
## Faux positifs et faux négatifs
 
| | **Il y a une attaque** | **Il n'y a pas d'attaque** |
|---|---|---|
| **Alerte** | ✅ Vrai positif | ⚠️ **Faux positif** |
| **Pas d'alerte** | ❌ **Faux négatif** | ✅ Vrai négatif |
 
- Un **faux négatif** est dangereux : une attaque passe inaperçue.
- Trop de **faux positifs** sont dangereux aussi : les analystes, submergés d'alertes inutiles, finissent par les ignorer, et une vraie attaque se perd dans la masse. Avec un IPS, un faux positif **bloque du trafic légitime**.

## Anatomie d'une règle Suricata
 
**Suricata** est un IDS/IPS libre très répandu, disponible sous forme de paquet dans **pfSense** (TP2). Sa syntaxe de règles est presque identique à celle de **Snort**, un autre IDS historique.
 
```text
alert icmp $HOME_NET any -> any any (msg:"Ping emis depuis le LAN"; itype:8; sid:1000001; rev:1;)
```
 
| Élément | Exemple | Signification |
|---|---|---|
| **Action** | `alert` | Générer une alerte. En mode IPS, `drop` supprime le paquet. |
| **Protocole** | `icmp` | `tcp`, `udp`, `icmp`, ou un protocole applicatif : `http`, `tls`, `dns`... |
| **Source** et **port** | `$HOME_NET any` | `$HOME_NET` désigne les réseaux internes ; `any` = n'importe quel port |
| **Direction** | `->` | De la source vers la destination |
| **Destination** et **port** | `any any` | N'importe quelle adresse, n'importe quel port |
| **Options** (entre parenthèses) | `msg:"..."` | Message affiché dans l'alerte |
| | `itype:8` | Type ICMP 8 = Echo Request |
| | `sid:1000001` | Identifiant **unique** de la règle (les règles personnalisées utilisent les numéros à partir de 1 000 000) |
| | `rev:1` | Numéro de version de la règle |
 
Un exemple plus élaboré, qui détecte une connexion HTTPS vers un site précis :
 
```text
alert tls $HOME_NET any -> any any (msg:"Connexion a exemple.com"; flow:to_server,established; tls.sni; content:"exemple.com"; endswith; nocase; sid:1000002; rev:1;)
```
 
{{%notice style="info" title="Explication des options"%}}
- `flow:to_server,established` : uniquement le trafic du client vers le serveur, dans une connexion établie.
- `tls.sni` : examiner le champ **SNI** (*Server Name Indication*) de la poignée de main TLS. Dans ce champ, le navigateur indique **en clair** le nom du site qu'il veut joindre, avant que le chiffrement ne commence.
- `content:"exemple.com"; endswith; nocase` : le SNI doit **se terminer par** `exemple.com` (ce qui inclut `www.exemple.com`), sans tenir compte des majuscules.

Même si le contenu de la connexion HTTPS est chiffré (chapitre 4), le **nom** du site visité ne l'est pas toujours. C'est ce qui permet à un IDS d'identifier, voire de bloquer, certains sites.
{{%/notice%}}


{{%notice style="tip" title="IDS, journalisation et SIEM"%}} 

Un IDS produit des **alertes** qui doivent être **conservées** et **analysées**. Dans une organisation, elles sont généralement envoyées, avec les journaux des pare-feu, des serveurs et des postes (chapitre 6), vers un **SIEM** (*Security Information and Event Management*). 

Le SIEM centralise et corrèle les événements, ce qui permet aux analystes d'un **centre des opérations de sécurité** (SOC) de repérer une attaque qui laisse des traces à plusieurs endroits.
{{%/notice%}}

# 8.2.4. Architecture Zero Trust
---------

## Les limites du modèle périmétrique

Le modèle de sécurité traditionnel ressemble à un **château fort** : un mur solide (le pare-feu) protège l'**intérieur**, considéré comme **de confiance**. Une fois à l'intérieur (au bureau ou connecté au VPN), un utilisateur ou un appareil peut joindre la plupart des ressources.

![Modèle périmétrique (château fort, confiance à l'intérieur) vs Zero Trust (chaque accès à chaque ressource est vérifié)](/images/08-zero-trust.png)

Ce modèle ne correspond plus à la réalité :
- les employés travaillent **à distance**, depuis des réseaux que l'entreprise ne contrôle pas ;
- les applications sont de plus en plus hébergées **hors de l'entreprise** (services en ligne) ;
- surtout, il suffit qu'**un seul** poste soit compromis (hameçonnage, chapitre 6) ou qu'**un seul** identifiant VPN soit volé pour que l'attaquant se retrouve « à l'intérieur », où il peut se déplacer librement (**mouvement latéral**) jusqu'aux serveurs critiques.

## Le principe : "Ne jamais faire confiance, toujours vérifier"

L'architecture **Zero Trust** ("confiance zéro") part du principe qu'**aucun utilisateur, appareil ou réseau n'est digne de confiance par défaut**, qu'il soit à l'intérieur ou à l'extérieur du périmètre. **Chaque accès à chaque ressource** doit être vérifié.

Le **NIST** a publié en 2020 une référence sur ce modèle (publication **SP 800-207**). On peut résumer ses principes en trois idées :

| Principe | En pratique | Lien avec le cours |
|---|---|---|
| **Vérifier explicitement** | Chaque demande d'accès est évaluée selon l'**identité** de l'utilisateur (authentification forte), l'**état de l'appareil** (à jour, antivirus actif, chiffré), l'heure, le lieu... | MFA (ch. 5), mises à jour et EDR (ch. 6) |
| **Moindre privilège** | Accès limité à **la seule ressource** demandée, pour **une durée limitée**, et non à tout le réseau | RBAC (ch. 5) |
| **Présumer la compromission** | Concevoir le réseau comme si un attaquant était **déjà à l'intérieur** : segmenter finement, chiffrer même en interne, journaliser et surveiller | Microsegmentation (8.2.1), TLS (ch. 4), IDS et SIEM (8.2.3) |

## Comparaison

| | **Modèle périmétrique** | **Zero Trust** |
|---|---|---|
| Confiance accordée selon... | L'**emplacement** sur le réseau (intérieur = confiance) | L'**identité**, l'**état de l'appareil** et le **contexte** |
| Accès à distance | VPN : accès au **réseau entier** | **ZTNA** (*Zero Trust Network Access*) : accès à **une application précise** |
| Trafic interne | Souvent en clair, peu surveillé | Chiffré et journalisé |
| Un poste est compromis | L'attaquant peut se déplacer librement | L'attaquant n'atteint que les ressources autorisées pour ce poste, et un comportement anormal est détecté |
| Vérification | Une fois, à la connexion | **À chaque accès**, en continu |

{{%notice style="note" title="Zero Trust n'est pas un produit"%}}
*Zero Trust* est une **démarche** et une **architecture**, pas un logiciel que l'on installe. Elle combine des mécanismes que vous connaissez déjà : 
+ gestion des identités et MFA (chapitre 5) ;
+ sécurité des postes (chapitre 6) ;
+ segmentation et pare-feu (section 8.2.1) ;
+ chiffrement (chapitre 4) ;
+ surveillance (section 8.2.3). 

Une organisation y migre **progressivement**, en commençant généralement par ses ressources les plus critiques.
{{%/notice%}}


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


{{% expand "Solution" %}}
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
{{% /expand %}}



### 2. Faux positif ou faux négatif ?
Classez chaque situation : **vrai positif**, **faux positif**, **vrai négatif** ou **faux négatif**.
 
1. L'IDS génère une alerte « injection SQL » au moment où un attaquant teste `' OR '1'='1` sur le formulaire de connexion.
2. Chaque nuit, l'IDS génère une alerte « exfiltration de données » pendant la sauvegarde légitime vers le serveur de stockage.
3. Un attaquant exploite une vulnérabilité découverte la veille, pour laquelle aucune signature n'existe. Aucune alerte n'est générée.
4. Un employé consulte le site du cégep. Aucune alerte n'est générée.
5. Un IPS bloque l'accès d'un employé à un site légitime de documentation technique, parce que son nom ressemble à celui d'un site malveillant.

{{% expand "Solution" %}}
1. Vrai positif
2. Faux positif
3. Faux négatif
4. Vrai négatif
5. Faux positif (avec un IPS, le faux positif entraîne un blocage de trafic légitime)
{{% /expand %}}


# 8.3. Synthèse et références
---------
 
{{%notice style="tip" title="À retenir"%}}
| Attaque | Couche | CIA visé | Contre-mesures principales | Lien |
|---|---|---|---|---|
| Écoute du réseau | Toutes | Confidentialité | Chiffrement (TLS, VPN) | Ch. 4, ch. 7 |
| Usurpation IP / MAC | Internet, accès réseau | Authenticité | Filtrage des adresses incohérentes, authentification cryptographique | Ch. 4, ch. 5 |
| Empoisonnement ARP | Accès réseau | C, I, D | Chiffrement, protection des commutateurs, segmentation | 8.2.1 |
| Homme du milieu | Toutes | Confidentialité, intégrité | TLS et validation des certificats, HSTS, VPN | Ch. 4, ch. 7 |
| SYN flood | Transport | Disponibilité | SYN cookies, atténuation DDoS | |
| ICMP flood, amplification | Internet, transport | Disponibilité | Atténuation DDoS, limitation de débit, ne pas exposer de services UDP mal configurés | Ch. 1 (GitHub) |
 
| Défense | Rôle |
|---|---|
| **Pare-feu** | Autoriser ou bloquer les flux (adresses, ports) ; refus par défaut ; règles lues dans l'ordre |
| **Segmentation** (DMZ, VLAN) | Isoler les zones pour limiter le mouvement latéral |
| **VPN** | Tunnel chiffré et authentifié à travers un réseau non fiable |
| **IDS / IPS** | Détecter / bloquer les attaques selon le contenu et le comportement du trafic |
| **Zero Trust** | Ne jamais faire confiance selon l'emplacement ; vérifier chaque accès |
{{%/notice%}}
 
## Références
 
+ [NIST SP 800-207 : Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
+ [Suricata : documentation des règles](https://docs.suricata.io/en/latest/rules/index.html)
+ [WireGuard : site officiel](https://www.wireguard.com/)
+ [OPNsense : documentation officielle](https://docs.opnsense.org/)
+ [Centre canadien pour la cybersécurité](https://www.cyber.gc.ca/fr)