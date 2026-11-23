+++
title = 'Pare-feu et segmentation'
weight = '821'
draft = false
+++
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
 