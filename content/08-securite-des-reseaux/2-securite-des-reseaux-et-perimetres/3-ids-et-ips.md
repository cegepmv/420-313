+++
title = "IDS et IPS"
weight = '823'
draft = false
+++
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
| **Par signature** | Comparer le trafic à une base de **motifs d'attaques connues** (comme un antivirus, chapitre 6) | Précis, peu de faux positifs | Ne détecte pas les attaques **nouvelles** ; la base doit être mise à jour |
| **Par anomalie** | Apprendre le comportement **normal** du réseau et signaler ce qui s'en écarte | Peut détecter des attaques inconnues | Plus de faux positifs ; demande une période d'apprentissage |
 
## Faux positifs et faux négatifs
 
| | **Il y a une attaque** | **Il n'y a pas d'attaque** |
|---|---|---|
| **Alerte** | ✅ Vrai positif | ⚠️ **Faux positif** |
| **Pas d'alerte** | ❌ **Faux négatif** | ✅ Vrai négatif |
 
- Un **faux négatif** est dangereux : une attaque passe inaperçue.
- Trop de **faux positifs** sont dangereux aussi : les analystes, submergés d'alertes inutiles, finissent par les ignorer, et une vraie attaque se perd dans la masse. Avec un IPS, un faux positif **bloque du trafic légitime**.

## Anatomie d'une règle Suricata
 
**Suricata** est un IDS/IPS libre très répandu, intégré au pare-feu **OPNsense**. Sa syntaxe de règles est presque identique à celle de **Snort**, un autre IDS historique.
 
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
