+++
title = 'Empoisonnement ARP et homme du milieu'
weight = '812'
draft = false
+++
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

