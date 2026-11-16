+++
title = 'Écoute et usurpation'
weight = '811'
draft = false
+++
---------

## Capture de paquets

Vous avez déjà utilisé **Wireshark** dans le cours *Conception de réseaux informatiques* pour capturer et analyser des paquets. Un attaquant peut utiliser exactement les mêmes outils.

Sur un réseau moderne, chaque poste est relié à un **commutateur** (*switch*), qui envoie une trame **uniquement** au poste destinataire. Un poste ne voit donc normalement que **son propre trafic**. Pour intercepter le trafic des autres, l'attaquant doit d'abord **détourner** les trames vers lui : c'est le but de l'**empoisonnement ARP** (section 8.1.2).

Une fois le trafic détourné, tout ce qui circule **en clair** peut être lu : un mot de passe ou un cookie de session envoyé en **HTTP** apparaît directement dans Wireshark. Au chapitre 7, vous avez volé un cookie `PHPSESSID` en le copiant dans le navigateur ; un attaquant qui écoute le réseau peut l'obtenir **sans aucune interaction** avec la victime.

<!-- {{%notice style="info" title="Démonstration en classe"%}}
Votre enseignant vous montrera une capture Wireshark de la connexion à DVWA (chapitre 7) :
- en **HTTP** : le nom d'utilisateur, le mot de passe et le cookie de session sont lisibles ;
- en **HTTPS** : seul du trafic chiffré par TLS est visible.
{{%/notice%}} -->

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