+++
title = 'Laboratoire'
weight = '840'
draft = true
+++
---------
 
<!-- ##### [Laboratoire - Implémentation d'un réseau sécurisé (OPNsense, WireGuard, Suricata)]({{% ref "08-securite-des-reseaux/4-laboratoires/1-reseau-securise" %}}) -->
 
###### Objectifs
- configurer des règles de pare-feu entre un LAN, une DMZ et Internet ;
- publier un serveur Web de la DMZ (redirection de port) ;
- mettre en place un VPN WireGuard pour le télétravail ;
- écrire des règles Suricata et passer du mode IDS au mode IPS ;
- porter un regard critique sur l'architecture mise en place.
{{%notice style="warning" title="Important"%}}
Le laboratoire se déroule **uniquement** dans un réseau isolé de machines virtuelles (**LAN Segment** de VMware). Ne connectez jamais les machines du laboratoire au réseau du cégep en mode **pont** (*bridged*).
{{%/notice%}}
 
 
# 8.4.1. Laboratoire : Implémentation d'un réseau sécurisé (OPNsense, WireGuard, Suricata)
---------
 
## Contexte et objectif
 
L'entreprise **ABC Logistiques** veut sécuriser son réseau. Elle possède :
- un **serveur Web** qui expose un site au public ;
- une machine interne qui ne doit être accessible **qu'aux employés**.
ABC Logistiques souhaite aussi permettre à ses employés d'accéder au réseau interne **à distance**, pour le télétravail.
 
En tant qu'administrateur système, vous devez :
1. contrôler les flux entre le réseau interne, la DMZ et Internet avec des **règles de pare-feu** ;
2. publier le site Web sur Internet ;
3. mettre en place un **VPN WireGuard** pour les employés en télétravail ;
4. mettre en place un **IDS/IPS Suricata** pour surveiller le trafic et bloquer certaines applications.
Vous utiliserez **OPNsense**, un système d'exploitation libre largement utilisé en entreprise comme routeur et pare-feu, qui intègre nativement WireGuard et Suricata.
 
## Topologie
 
![Topologie du laboratoire : Internet (NAT VMware) avec le client Ubuntu, pare-feu OPNsense, LAN 172.16.10.0/24 avec la machine Ubuntu 172.16.10.20, DMZ 172.16.20.0/24 avec le serveur Web 172.16.20.10, tunnel WireGuard 10.10.10.0/24](/images/08-labo-topologie.png)
 
| Réseau | Adresse | Machines |
|---|---|---|
| **WAN** (« Internet ») | NAT de VMware (DHCP) | Client Ubuntu (employé en télétravail) |
| **LAN** (réseau interne) | `172.16.10.0/24` | Ubuntu interne `172.16.10.20` ; interface OPNsense `172.16.10.1` |
| **DMZ** | `172.16.20.0/24` | Serveur Web `172.16.20.10` ; interface OPNsense `172.16.20.1` |
| **Tunnel VPN** | `10.10.10.0/24` | OPNsense `10.10.10.1` ; client VPN `10.10.10.2` |
 
Quatre VMs vous sont fournies :
 
| VM | Fichier | Configuration de la carte réseau |
|---|---|---|
| Client Ubuntu (« Internet ») | `ubuntu-client.ova` | **NAT** |
| Ubuntu interne | `ubuntu-lan.ova` | **LAN Segment** `LAN` |
| Serveur Web | `ubuntu-web-server.ova` | **LAN Segment** `DMZ` |
| OPNsense | `opnsense.ova` | Carte 1 : **NAT** (WAN) ; carte 2 : **LAN Segment** `LAN` ; carte 3 : **LAN Segment** `DMZ` |
 
## Configuration initiale
 
Après avoir configuré les cartes réseau, démarrez la VM **OPNsense**, puis la VM **Ubuntu interne**.
 
Sur la console de la VM OPNsense, relevez l'**adresse IP de l'interface WAN** (attribuée par le NAT de VMware) et notez-la. Vous en aurez besoin dans les parties II et III.
 
```text
IP WAN d'OPNsense : ____________________
```
 
Sur l'Ubuntu interne, ouvrez un navigateur à l'adresse `https://172.16.10.1`.
 
Le navigateur affiche un **avertissement de certificat** : l'interface d'administration d'OPNsense utilise un **certificat autosigné**, comme celui que vous avez créé au chapitre 4. Acceptez-le, puis connectez-vous :
 
- Identifiant : `root`
- Mot de passe : fourni par votre enseignant
{{%notice style="info" title="Versions d'OPNsense"%}}
L'interface d'OPNsense évolue d'une version à l'autre : le nom ou l'emplacement de certains menus peut légèrement différer de ce qui est indiqué dans cet énoncé. En cas de doute, appelez votre enseignant.
{{%/notice%}}
 
 
## Partie I : Règles de pare-feu
 
### Règles pour le LAN
 
Par défaut, OPNsense contient une règle **« Default allow LAN to any rule »** : tout le trafic est autorisé du LAN vers le WAN **et** vers la DMZ. Vérifiez-le en ouvrant `http://172.16.20.10` depuis le navigateur de l'Ubuntu interne.
 
Nous voulons limiter le trafic du LAN vers la DMZ à :
- **HTTP** (port 80) vers le serveur Web, pour consulter le site ;
- **SSH** (port 22) vers le serveur Web, pour l'administrer ;
- les requêtes **ping** vers le serveur Web.
Tout le reste du trafic LAN → DMZ doit être bloqué. Le trafic LAN → Internet reste autorisé.
 
Allez dans **Firewall → Rules → LAN** et ajoutez (bouton **+**) les règles suivantes.
 
**Règle A : Bloquer le LAN vers la DMZ par défaut**
- Action : `Block`
- Interface : `LAN`
- Protocol : `any`
- Source : `LAN net`
- Destination : `DMZ net`
- Description : `Bloquer les flux LAN vers DMZ par défaut`
**Règle B : Autoriser HTTP vers le serveur Web**
- Action : `Pass`
- Protocol : `TCP`
- Source : `LAN net`
- Destination : `Single host or Network` → `172.16.20.10`
- Destination port range : `HTTP` à `HTTP`
- Description : `Autoriser HTTP vers le serveur Web`
**Règle C : Autoriser SSH vers le serveur Web**
- Mêmes options que la règle B, avec le port `SSH` (22)
- Description : `Autoriser SSH vers le serveur Web`
**Règle D : Autoriser le ping vers le serveur Web**
- Action : `Pass`
- Protocol : `ICMP`
- ICMP type : `Echo Request`
- Source : `LAN net`
- Destination : `172.16.20.10`
- Description : `Autoriser le ping vers le serveur Web`
{{%notice style="warning" title="L'ordre des règles est essentiel"%}}
Les règles sont lues de haut en bas, et la **première qui correspond s'applique** (section 8.2.1). Vérifiez que l'ordre final des règles de l'interface LAN est le suivant (utilisez les flèches de déplacement ou le champ de séquence selon votre version) :
 
1. Règle B (HTTP)
2. Règle C (SSH)
3. Règle D (ping)
4. Règle A (blocage LAN → DMZ)
5. Default allow LAN to any rule
{{%/notice%}}
Cliquez sur **Apply changes**, puis testez depuis l'Ubuntu interne :
- `http://172.16.20.10` dans le navigateur : doit **fonctionner** ;
- `ping -c 2 172.16.20.10` : doit **fonctionner** ;
- `ssh infosec@172.16.20.10` : doit **fonctionner** ;
- `curl -m 3 https://172.16.20.10` : doit **échouer** (port 443 non autorisé) ;
- `ping -c 2 8.8.8.8` : doit **fonctionner** (accès à Internet).
### Règles pour la DMZ
 
Par défaut, l'interface DMZ n'a **aucune règle** : tout le trafic qui en provient est **bloqué** (refus par défaut). Nous voulons autoriser le serveur Web à faire des requêtes HTTP, HTTPS et DNS (pour ses mises à jour), tout en l'empêchant de joindre le LAN.
 
Dans **Firewall → Rules → DMZ**, ajoutez les règles suivantes, **dans cet ordre** :
 
| # | Action | Protocol | Source | Destination | Port | Description |
|---|---|---|---|---|---|---|
| 1 | Block | any | DMZ net | LAN net | any | Bloquer les flux DMZ vers LAN |
| 2 | Pass | TCP | DMZ net | any | HTTP (80) | Autoriser HTTP sortant |
| 3 | Pass | TCP | DMZ net | any | HTTPS (443) | Autoriser HTTPS sortant |
| 4 | Pass | TCP/UDP | DMZ net | any | DNS (53) | Autoriser DNS |
 
Appliquez les changements, puis testez :
- depuis le **serveur Web** : `curl -I https://www.wikipedia.org` doit **fonctionner** ;
- depuis l'**Ubuntu interne** : `ping -c 2 172.16.20.10` doit **toujours fonctionner** ;
- depuis le **serveur Web** : `ping -c 2 172.16.10.20` doit **échouer**.
### Publication du serveur Web (redirection de port)
 
Pour rendre le site accessible depuis Internet, la bonne pratique serait d'utiliser un **proxy inverse** (*reverse proxy*), qui reçoit les requêtes et les transmet au serveur après les avoir filtrées. Pour simplifier, nous allons utiliser une **redirection de port** (*port forwarding*) : le pare-feu transmet au serveur Web les connexions reçues sur le port 80 de son adresse WAN.
 
Allez dans **Firewall → NAT → Port Forward** (le nom du menu peut varier selon la version) et ajoutez une règle :
 
- Interface : `WAN`
- Protocol : `TCP`
- Destination : `WAN address`
- Destination port range : `HTTP` à `HTTP`
- Redirect target IP : `Single host or Network` → `172.16.20.10`
- Redirect target port : `HTTP`
- Filter rule association : `Add associated filter rule`
- Description : `Publication du site Web`
Enregistrez et appliquez. Depuis le **client Ubuntu** (« Internet »), ouvrez `http://<IP WAN d'OPNsense>`.
 
Cela ne fonctionne pas ! Pourtant, la redirection est bien configurée...
 
Allez dans **Interfaces → WAN** : les options **Block private networks** et **Block bogon networks** sont activées. Elles bloquent les paquets qui arrivent sur l'interface WAN avec une adresse source **privée** (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) ou réservée. Or, notre « Internet » est le réseau NAT de VMware, qui utilise des adresses privées.
 
Décochez ces deux options, enregistrez et appliquez, puis réessayez depuis le client Ubuntu.
 
Avant de passer à la partie suivante, appelez votre enseignant pour qu'il vérifie votre implémentation.
 
###### **Questions**
1. Pourquoi la règle A doit-elle être placée **après** les règles B, C et D, mais **avant** la règle « Default allow LAN to any » ? Que se passerait-il dans chacun des deux autres cas ?
2. Dans le TP de l'an dernier, une règle supplémentaire autorisait les réponses ICMP *Echo Reply* de la DMZ vers le LAN. Pourtant, le ping du LAN vers le serveur Web fonctionne sans elle. Pourquoi cette règle est-elle inutile ?
3. Pourquoi bloque-t-on les flux de la DMZ vers le LAN ? Décrivez un scénario d'attaque que cette règle permet de contenir.
4. L'option **Block private networks** doit rester activée en production. Quelle attaque vue à la section 8.1.1 permet-elle de bloquer ?
5. Pourquoi un proxy inverse serait-il plus sûr que la redirection de port ?
## Partie II : VPN WireGuard
 
Dans cette partie, vous allez permettre au client Ubuntu (« Internet ») d'accéder au réseau interne à travers un tunnel **WireGuard**.
 
### Étape 1 : Créer l'instance WireGuard (serveur)
 
Allez dans **VPN → WireGuard → Instances** et ajoutez une instance (bouton **+**) :
 
- Enabled : ✅
- Name : `teletravail`
- Public key / Private key : cliquez sur l'icône d'**engrenage** pour générer automatiquement la paire de clés
- Listen port : `51820`
- Tunnel address : `10.10.10.1/24`
Enregistrez. Au bas de la page, cochez **Enable WireGuard**, puis cliquez sur **Apply**.
 
### Étape 2 : Générer la configuration de l'employé
 
Allez dans l'onglet **Peer generator** :
 
- Instance : `teletravail`
- Endpoint : `<IP WAN d'OPNsense>:51820`
- Name : `employe1`
- Address : laissez l'adresse proposée (`10.10.10.2/32`)
- Allowed IPs : `172.16.10.0/24`
- DNS Servers : **laissez vide**
La zone de configuration affiche le contenu du fichier destiné au client. **Copiez-le** (vous pouvez le coller dans un fichier texte temporaire).
 
{{%notice style="warning" title="Attention : enregistrer le pair"%}}
Le pair n'est **pas enregistré** tant que vous n'avez pas cliqué sur le bouton d'enregistrement **au bas de la page** du générateur. Vérifiez ensuite que `employe1` apparaît bien dans l'onglet **Peers**, puis cliquez sur **Apply**.
{{%/notice%}}
 
### Étape 3 : Règles de pare-feu pour le VPN
 
Contrairement à l'assistant OpenVPN, WireGuard ne crée pas automatiquement de règles de pare-feu. Il en faut deux :
 
**Sur l'interface WAN** (Firewall → Rules → WAN), pour que les clients puissent joindre le serveur VPN :
- Action : `Pass`
- Protocol : `UDP`
- Source : `any`
- Destination : `WAN address`
- Destination port range : `51820` à `51820`
- Description : `Autoriser les connexions WireGuard`
**Sur l'interface WireGuard** (Firewall → Rules → **WireGuard (Group)**), pour que le trafic du tunnel puisse atteindre le LAN :
- Action : `Pass`
- Protocol : `any`
- Source : `10.10.10.0/24`
- Destination : `LAN net`
- Description : `Autoriser le VPN vers le LAN`
Appliquez les changements.
 
### Étape 4 : Configurer le client
 
Sur le **client Ubuntu**, créez le fichier de configuration et collez-y la configuration copiée à l'étape 2 :
 
```bash
sudo nano /etc/wireguard/wg0.conf
```
 
Le fichier devrait ressembler à ceci :
 
```ini
[Interface]
PrivateKey = <clé privée du client>
Address = 10.10.10.2/32
 
[Peer]
PublicKey = <clé publique d'OPNsense>
Endpoint = <IP WAN d'OPNsense>:51820
AllowedIPs = 172.16.10.0/24
```
 
{{%notice style="info" title="Explication du fichier de configuration"%}}
- `[Interface]` décrit **le client** :
  - `PrivateKey` : la clé privée du client. Comme au chapitre 4, elle ne doit **jamais** être partagée ;
  - `Address` : l'adresse IP du client **dans le tunnel**.
- `[Peer]` décrit **le serveur** (OPNsense) :
  - `PublicKey` : la clé publique d'OPNsense, qui permet d'authentifier le serveur ;
  - `Endpoint` : l'adresse et le port où joindre le serveur sur Internet ;
  - `AllowedIPs` : les réseaux dont le trafic doit passer **dans le tunnel**. Ici, seul le trafic vers le LAN (`172.16.10.0/24`) y passe : c'est un **tunnel partagé** (section 8.2.2).
{{%/notice%}}
{{%notice style="tip" title="Si le fichier contient une ligne DNS"%}}
Si le fichier contient une ligne `DNS = ...`, supprimez-la : elle nécessite un outil (`resolvconf`) qui n'est pas installé sur le client et provoquerait une erreur.
{{%/notice%}}
 
Protégez le fichier, qui contient une clé privée, puis activez le tunnel :
 
```bash
sudo chmod 600 /etc/wireguard/wg0.conf
sudo wg-quick up wg0
```
 
WireGuard n'affiche pas de message de confirmation. Pour vérifier que le tunnel fonctionne :
 
```bash
sudo wg show
```
 
La ligne `latest handshake` doit indiquer une poignée de main récente (quelques secondes). Dans OPNsense, **VPN → WireGuard → Status** doit aussi afficher le pair `employe1` avec une poignée de main récente.
 
Testez l'accès au réseau interne depuis le client :
 
```bash
ping -c 2 172.16.10.20
ssh infosec@172.16.10.20
```
 
### Étape 5 : Observer le trafic chiffré
 
Sur le **client Ubuntu**, lancez Wireshark sur l'interface **physique** (`ens33`) avec le filtre `wg`. Dans un terminal, relancez `ping 172.16.10.20`.
 
Lancez ensuite une deuxième capture sur l'interface **`wg0`** avec le filtre `icmp`.
 
Si vous y êtes arrivés, bravo ! Appelez votre enseignant pour qu'il vérifie cette partie.
 
###### **Questions**
6. Dans la capture sur `ens33`, quels sont les adresses IP, le protocole de transport et le port utilisés ? Pouvez-vous voir qu'il s'agit d'un `ping` ?
7. Dans la capture sur `wg0`, que voyez-vous ? Expliquez la différence entre les deux captures à l'aide de la notion d'**encapsulation**.
8. Contrairement à OpenVPN, WireGuard n'utilise pas de certificats ni d'autorité de certification. Comment le client et le serveur s'authentifient-ils mutuellement ? À quel laboratoire du chapitre 4 ce mécanisme ressemble-t-il ?
9. Pendant que le tunnel est actif, ouvrez `https://www.wikipedia.org` sur le client. Ce trafic passe-t-il par le VPN ? Justifiez à l'aide du paramètre `AllowedIPs`.
10. Que faudrait-il modifier pour obtenir un **tunnel complet** ? Donnez un avantage et un inconvénient de chaque configuration pour l'entreprise.
11. Pourquoi la règle WAN n'autorise-t-elle **que** le port UDP 51820, et non tout le trafic vers le pare-feu ?
## Partie III : Détection et prévention d'intrusion avec Suricata
 
L'entreprise souhaite :
- être **alertée** lorsqu'une machine du LAN envoie un `ping` ;
- **interdire** l'accès à **Facebook**, **Netflix** et **ChatGPT** depuis le LAN.
Vous allez utiliser **Suricata**, l'IDS/IPS intégré à OPNsense.
 
### Étape 1 : Préparer les interfaces
 
Suricata en mode IPS ne fonctionne pas correctement avec les fonctions d'accélération matérielle des cartes réseau. Allez dans **Interfaces → Settings** et cochez les options qui **désactivent** :
- Hardware CRC ;
- Hardware TSO ;
- Hardware LRO.
Enregistrez.
 
### Étape 2 : Écrire vos règles
 
Les règles personnalisées seront écrites dans un fichier sur l'**Ubuntu interne**, puis téléchargées par OPNsense (votre enseignant a préconfiguré OPNsense pour aller chercher le fichier `labo.rules` à l'adresse `http://172.16.10.20:8000/`).
 
Sur l'**Ubuntu interne**, créez un répertoire et le fichier de règles :
 
```bash
mkdir ~/regles
cd ~/regles
nano labo.rules
```
 
Écrivez-y les deux règles suivantes :
 
```text
alert icmp $HOME_NET any -> any any (msg:"LABO - Ping emis depuis le LAN"; itype:8; sid:1000001; rev:1;)
alert tls $HOME_NET any -> any any (msg:"LABO - Connexion a Facebook"; flow:to_server,established; tls.sni; content:"facebook.com"; endswith; nocase; sid:1000002; rev:1;)
```
 
En vous inspirant de la deuxième règle et de la section 8.2.3, **ajoutez vous-même** :
- une règle (`sid:1000003`) qui alerte lorsqu'une machine du LAN se connecte à **Netflix** (`netflix.com`) ;
- une règle (`sid:1000004`) qui alerte lorsqu'une machine du LAN se connecte à **ChatGPT** (`chatgpt.com`).
Dans le même répertoire, démarrez un serveur Web pour rendre le fichier accessible à OPNsense, et **laissez-le tourner** :
 
```bash
python3 -m http.server 8000
```
 
### Étape 3 : Activer Suricata en mode IDS
 
Dans OPNsense, allez dans **Services → Intrusion Detection → Administration**.
 
Dans l'onglet **Download** :
1. cochez l'ensemble de règles **labo** ;
2. cliquez sur **Enable selected** ;
3. cliquez sur **Download & Update Rules**.
Dans le terminal du serveur Python, vous devriez voir une requête `GET /labo.rules` provenant d'OPNsense.
 
Dans l'onglet **Settings** :
- Enabled : ✅
- Capture mode : **PCAP live mode (IDS)**
- Interfaces : `LAN`
Cliquez sur **Apply**.
 
Depuis l'**Ubuntu interne**, ouvrez un **nouveau** terminal et générez du trafic :
 
```bash
ping -c 3 172.16.20.10
curl -s -o /dev/null https://www.facebook.com
curl -s -o /dev/null https://www.netflix.com
curl -s -o /dev/null https://chatgpt.com
```
 
Consultez l'onglet **Alerts** : vous devriez voir une alerte pour chacune de vos règles.
 
### Étape 4 : Passer en mode IPS
 
Vous allez maintenant **bloquer** les trois sites, tout en continuant de seulement **alerter** pour les `ping`.
 
1. Dans l'onglet **Settings**, changez le **Capture mode** pour **Netmap (IPS)** (ou **Divert (IPS)** si votre enseignant vous l'indique), puis cliquez sur **Apply**.
2. Dans l'onglet **Rules**, recherchez `LABO`. Pour les règles `1000002`, `1000003` et `1000004`, changez l'**action** de `Alert` à `Drop`. Laissez la règle `1000001` (ping) à `Alert`.
3. Cliquez sur **Apply**.
Testez de nouveau depuis l'Ubuntu interne :
 
```bash
ping -c 3 172.16.20.10
curl -m 5 -s -o /dev/null -w "%{http_code}\n" https://www.facebook.com
curl -m 5 -s -o /dev/null -w "%{http_code}\n" https://www.wikipedia.org
```
 
Le ping doit toujours fonctionner (et générer une alerte), l'accès à Facebook doit échouer, et l'accès à Wikipédia doit fonctionner.
 
Si vous avez terminé, bravo ! Appelez votre enseignant pour qu'il vérifie cette partie.
 
###### **Questions**
12. Quelle est la différence entre le mode **PCAP live (IDS)** et le mode **Netmap (IPS)** ? Pourquoi Suricata doit-il être placé **dans** le trafic pour pouvoir bloquer ?
13. Expliquez chacun des éléments de la règle que vous avez écrite pour ChatGPT.
14. Le trafic vers Facebook est chiffré par HTTPS. Comment Suricata peut-il savoir qu'une connexion est destinée à Facebook ?
15. Pourquoi est-il prudent de commencer en mode IDS et d'observer les alertes **avant** de passer en mode IPS ? Quel risque prend-on en bloquant directement ?
16. Un employé utilise le VPN d'un fournisseur commercial depuis son poste du LAN pour accéder à Facebook. Vos règles le bloquent-elles ? Pourquoi ?
## Partie IV : Analyse critique
 
Répondez aux questions suivantes en quelques phrases.
 
17. L'architecture que vous avez mise en place est **périmétrique**. En vous appuyant sur la section 8.2.4, expliquez en quoi elle n'est **pas** une architecture Zero Trust. Proposez **trois** modifications qui la rapprocheraient de ce modèle.
18. Un employé en télétravail se fait voler son portable, sur lequel le fichier `wg0.conf` est enregistré. Quelles sont les conséquences ? Quelles mesures auraient pu les limiter ?
19. Une extension récente de TLS, **ECH** (*Encrypted Client Hello*), chiffre le champ SNI. Quelle conséquence aurait sa généralisation sur les règles que vous avez écrites dans la partie III ?
## Méthode d'évaluation
 
Pour chaque élément de la grille, appelez votre enseignant pour lui montrer votre implémentation. Si elle fonctionne, il vous accordera les points associés. Remettez ensuite vos réponses aux questions.
 
| Élément évalué | Points |
|---|---|
| Règles de pare-feu du LAN (ordre correct, tests réussis) | 3 |
| Règles de pare-feu de la DMZ | 2 |
| Publication du site Web (accessible depuis le WAN) | 2 |
| Tunnel WireGuard fonctionnel (SSH vers le LAN depuis le client) | 3 |
| Règles Suricata en mode IDS (4 alertes, dont les 2 règles écrites par vous) | 2 |
| Blocage de Facebook, Netflix et ChatGPT en mode IPS (ping toujours autorisé) | 2 |
| Questions d'analyse (parties I à IV) | 1 |
| **TOTAL** | **15** |