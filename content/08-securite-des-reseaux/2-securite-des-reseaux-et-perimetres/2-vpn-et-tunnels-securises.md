+++
title = 'VPN et tunnels sécurisés'
weight = '822'
draft = false
+++
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
 
<!-- {{%notice style="info" title="Pourquoi WireGuard est-il devenu si populaire ?"%}}
- **Simplicité** : environ 4 000 lignes de code, contre des centaines de milliers pour IPsec ou OpenVPN. Un code court est plus facile à **auditer**, donc moins susceptible de contenir des vulnérabilités.
- **Cryptographie moderne et fixe** (ChaCha20, Curve25519) : il n'y a pas d'algorithme à négocier, donc pas de risque de choisir un algorithme obsolète (comme DES ou MD5, chapitre 4).
- **Authentification par paires de clés**, sur le même principe que les clés SSH du laboratoire 5.3 du chapitre 4 : chaque pair possède une clé privée et connaît la clé publique de l'autre.
WireGuard sera utilisé dans le TP2.
{{%/notice%}} -->
 
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