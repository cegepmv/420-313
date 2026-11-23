+++
title = 'Exercices'
weight = '814'
draft = false
+++ 
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

<!-- ### 2. Analyse d'une capture
Un administrateur capture le trafic ARP sur le poste `10.10.50.20` et obtient l'extrait suivant :

| N° | Temps (s) | Source | Destination | Protocole | Info |
|---|---|---|---|---|---|
| 1 | 0.000 | 00:0c:29:aa:aa:aa | 00:0c:29:20:20:20 | ARP | 10.10.50.1 is at 00:0c:29:aa:aa:aa |
| 2 | 0.512 | 00:0c:29:66:66:66 | 00:0c:29:20:20:20 | ARP | 10.10.50.1 is at 00:0c:29:66:66:66 |
| 3 | 2.514 | 00:0c:29:66:66:66 | 00:0c:29:20:20:20 | ARP | 10.10.50.1 is at 00:0c:29:66:66:66 |
| 4 | 4.515 | 00:0c:29:66:66:66 | 00:0c:29:20:20:20 | ARP | 10.10.50.1 is at 00:0c:29:66:66:66 |

Wireshark ajoute l'avertissement : `duplicate use of 10.10.50.1 detected!`

1. Que se passe-t-il ?
2. Quelle adresse MAC appartient probablement à l'attaquant ? Justifiez.
3. Si `10.10.50.1` est la passerelle du réseau, quelles sont les conséquences pour le poste `10.10.50.20` ?
4. Proposez deux contre-mesures. -->

<!-- {{% expand "Solution" %}}
1. Deux adresses MAC différentes se déclarent propriétaires de la même adresse IP `10.10.50.1` : c'est un empoisonnement ARP.
2. `00:0c:29:66:66:66`. Elle envoie des réponses ARP **non sollicitées** à intervalles réguliers (toutes les 2 secondes), ce qui est typique d'un outil d'empoisonnement. On peut le confirmer en consultant l'adresse MAC réelle de la passerelle sur la passerelle elle-même.
3. Tout le trafic du poste vers Internet passe désormais par l'attaquant. Celui-ci peut lire le trafic non chiffré, le modifier (homme du milieu) ou le bloquer (déni de service).
4. Au choix : chiffrement systématique (TLS, VPN) pour rendre le trafic intercepté inutilisable, vérification des réponses ARP par le commutateur (*Dynamic ARP Inspection*), segmentation du réseau, détection par un IDS (section 8.2.3).
{{% /expand %}} -->
