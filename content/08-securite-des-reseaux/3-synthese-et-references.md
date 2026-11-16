+++
title = 'Synthèse et références'
weight = '830'
draft = false
+++
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
