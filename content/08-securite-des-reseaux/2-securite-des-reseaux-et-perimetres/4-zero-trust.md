+++
title = 'Architecture Zero Trust'
weight = '824'
draft = false
+++
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
