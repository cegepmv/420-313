+++
pre = '<b>08. </b>'
title = 'Sécurité des réseaux'
weight = '800'
draft = false
+++
---------

Les chapitres précédents ont porté sur la protection des **données** (chapitre 4), des **identités** (chapitre 5), des **postes de travail** (chapitre 6) et des **applications** (chapitre 7). Ce chapitre s'intéresse au **chemin entre tous ces éléments** : le réseau.

La première partie présente les principales **attaques** qui exploitent les faiblesses des protocoles réseau. La deuxième partie présente les **mécanismes de défense** qui permettent de protéger un réseau et son périmètre (pare-feu, segmentation, VPN, IDS/IPS, Zero Trust).

<!-- {{%notice style="warning" title="Cadre éthique et légal"%}}
Les techniques présentées sont étudiées **pour apprendre à s'en défendre**. Au Canada, deux articles du *Code criminel* s'appliquent directement aux notions de ce chapitre :

- **art. 342.1** : utilisation non autorisée d'un ordinateur ;
- **art. 184** : interception de communications privées (la **capture de paquets** sur un réseau qui ne vous appartient pas en fait partie).

Les manipulations se font **uniquement** dans le réseau isolé des machines virtuelles du laboratoire. L'exploitation pratique de ces attaques sera reprise dans le cours *Piratage éthique*.
{{%/notice%}} -->

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






