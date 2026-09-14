<!-- 
## 4.6 - Certificats et signature numérique

### Fonctions de hachage

Une **fonction de hachage** transforme un message de taille quelconque en une empreinte (*hash*) de taille fixe, avec les propriétés suivantes :

- **Irréversibilité** : impossible de retrouver le message d'origine à partir de l'empreinte.
- **Effet avalanche (pseudo-aléatoires)** : un changement minime du message produit une empreinte complètement différente.
- **Résistance aux collisions** : il doit être extrêmement difficile de trouver deux messages différents produisant la même empreinte.

Applications : vérification de l'intégrité d'un fichier téléchargé, stockage sécurisé de mots de passe (jamais en clair), base des signatures numériques. Algorithme couramment utilisé aujourd'hui : **SHA-256** (la famille *MD5*/*SHA-1*, plus ancienne, est aujourd'hui considérée affaiblie pour un usage sécuritaire).

### Signature numérique

Une **signature numérique** combine le hachage et le chiffrement asymétrique : on calcule d'abord l'empreinte du message, puis on chiffre cette empreinte avec la clé **privée** de l'expéditeur. Le destinataire recalcule l'empreinte du message reçu et la compare à celle obtenue en déchiffrant la signature avec la clé **publique** de l'expéditeur. Une correspondance garantit à la fois l'**intégrité** du message et son **authenticité** (non-répudiation).

> 📊 *Illustration suggérée : schéma en deux colonnes (expéditeur / destinataire) montrant le processus complet de signature puis de vérification.*

### Certificats numériques et infrastructure à clé publique

Un **certificat numérique** associe une clé publique à l'identité de son détenteur (une personne, une organisation, un serveur web) et est signé par une **autorité de certification (CA)** de confiance. C'est ce mécanisme qui permet, par exemple, à un navigateur web de faire confiance à la clé publique présentée par un site HTTPS — on approfondira le fonctionnement complet de l'**infrastructure à clé publique (PKI)** au chapitre 5. -->




<!-- 

Fonctions de hachage
Une fonction de hachage permet d’associer une valeur numérique unique à n’importe quelle information stockée sous forme de bits. Le tableau suivant montre les valeurs de hachage MD5 pour des mots, des phrases ou des valeurs numériques :

Valeur initiale	Valeur hachée avec MD5
soleil	26e2ee2b2c72fb6c2d7e8cb71ace445
MD5, pour Message Digest 5, est une fonction de hachage cryptographique qui permet d’obtenir l’empreinte numérique d’un fichier (on parle souvent de message). Il a été inventé par Ronald Rivest en 1991.	98053fe10fd2b9462a43f61126ddeec3
14	367764329430db34be92fd14a7a770ee
Ici, les valeurs de hachage sont représentées en hexadécimal (comme chacune comprend 32 caractères, à 4 bits par caractère, on en déduit que MD5 donne des « empreintes numériques » de 128 bits).

Les fonctions de hachage ont les propriétés suivantes :

Taille constante: Peu importe la taille du message original, une fonction de hachage donnée donnera un nombre dont la taille en bits sera toujours la même (128 bits pour MD5, 160 bits pour SHA-0 et SHA-1, 256 bits pour SHA-256, etc.).

Déterministes: Une valeur d’entrée produira toujours la même valeur de sortie.

Sans collisions, à date: Deux valeurs d’entrées différentes n’ont jamais produit la même valeur de sortie jusqu’à maintenant, la probabilité d’une collision est de 1 / 2^256.

Irréversibles: Il est impossible de calculer la valeur d’entrée à partir de la valeur de sortie.

Uniformes: Les résultats doivent être uniformément distribués dans les valeurs possibles.

Pseudo-aléatoires: Toute variation dans la valeur d’entrée doit avoir un impact important dans le résultat.

De nombreux algorithmes de hachage satisfont ces propriétés; certains d’entre eux (MD5, SHA1, SHA256, etc.) sont disponibles sous linux à partir de la ligne de commande.

olivier@ubusrv:~$ echo "allo" | md5sum
d40ea526169cb99ec8b81ff4d60ebd78  -

olivier@ubusrv:~$ echo "allo" | sha1sum
69faafb743f087d1306010b7f301dd509e9703e0  -

olivier@ubusrv:~$ echo "allo" | sha256sum
1ddfb769eb0b8876bc570e25580e6a53afcf973362ee1ee4b54a807da2e5eed7  -
Applications des fonctions de hachage
Les propriétés des fonctions de hachage font qu’on les utilise dans de nombreux contextes en informatique :

Validation de l’intégrité (“checksum”): Lors de la transmission d’un message, l’expéditeur fournit l’empreinte du message ; ainsi le destinataire peut lancer la fonction de hachage lors de la réception du message et en comparant les deux empreintes, vérifier que le message a bien été transmis.

Structures de données (“hashtables”): En programmation, pour des données représentées en paires « clé-valeur » (par exemple, une adresse courriel et le nom de son propriétaire), on stocke les valeurs à des adresses mémoire correspondant aux empreintes des clés hachées auxquelles elles sont associées.

Hashmap

Mots de passe: Stocker les empreintes des mots de passe permet d’avoir l’assurance que leurs valeurs ne pourraient pas être décryptées si elles venaient à être divulguées.
Par exemple, dans le fichier /etc/shadow sur Linux, les champs sont les suivants :

Fonction de hachage – 6 désigne l’algorithme SHA-512
Sel cryptographique – une valeur numérique (représentée en base-64) qui est ajoutée au mot de passe entré par l’utilisateur lors du calcul de l’empreinte. Permet de bloquer les attaques par « rainbow tables » car les empreintes qui auraient été calculées à l’avance pour des milliards de mots de passe possibles ne correspondront jamais à l’empreinte du mot de passe « salé ».
Empreinte du mot de passe – le résultat de la fonction de hachage (en base-64) appliquée sur le mot de passe de l’utilisateur + le sel cryptographique
Date du dernier changement de mot de passe en nombre de jours depuis le 1er janvier 1970.
Nombre de jours minimum entre deux changements de mots de passe pour un utilisateur.
Nombre de jours maximum entre deux changements de mot de passe (= durée de validité).
Nombre de jours précédant la fin de validité du mot de passe durant lesquels l’utilisateur sera prévenu qu’il doit le renouveler.
Validation de l’identité
Les signatures numériques sont une technique qui permet (un peu comme une signature « manuelle ») d’attester que le contenu d’un message provient bel et bien d’un expéditeur donné, et que ce contenu n’a pas été altéré entre sa source et sa destination. Elles utilisent à la fois le chiffrement et le hachage ; cependant, le message lui-même n’a pas besoin d’être chiffré : c’est la signature qu’on lui ajoute qui l’est.

Dans une signature numérique, on inverse les rôles de la clé privée et de la clé publique, c’est-à-dire qu’on chiffre avec la clé privée et qu’on déchiffre avec la clé publique. En effet, si on reçoit des données chiffrées et qu’on arrive à les déchiffrer avec la clé publique de A, on peut supposer que c’est bien A qui nous a envoyé ces données, partant du principe que seul A dispose de la clé privée qui a permis de les chiffrer.

Supposons que A et B ont déjà échangé leurs clés publiques. Par la suite, B veut s’assurer que les messages qu’il reçoit proviennent bel et bien de A et n’ont pas été modifiés durant la transmission.

Lorsqu’il envoie son message, A appliquera alors la démarche suivante:

A génère une empreinte E de son message M

A utilise sa clé privée pour chiffrer cette empreinte. C’est cette empreinte chiffrée qui constitue la signature; A la joint à son message

Pour vérifier l’authenticité du message, B appliquera la procédure suivante:

B utilise la clé publique de A pour déchiffrer l’empreinte E

B génère sa propre empreinte du message

B compare les deux empreintes; si elles sont identiques, alors le message n’a pas été modifié (les empreintes le prouvent) et il provient bien de A (le déchiffrement de la signature le prouve).




Certificats numériques
TLS (Transport Layer Security) est le protocole de chiffrement utilisé dans les échanges HTTPS. Dans ce protocole, on utilise à la fois le chiffrement symétrique et asymétrique. Le principe est le suivant :

Alice veut envoyer un message à Bob. Elle doit :

Générer une clé symétrique
Chiffrer le message avec cette clé
Utiliser la clé publique de Bob pour chiffrer la clé symétrique
Envoyer le message chiffré et la clé chiffrée à Bob
Bob veut lire le message d’Alice. Il doit :

Utiliser sa clé privée pour déchiffrer la clé symétrique
Utiliser la clé symétrique pour déchiffrer le message
Sur un site HTTPS, lorsqu’on souhaite envoyer des données confidentielles (comme par exemple un numéro de carte de crédit), on doit donc avoir en notre possession la clé publique du serveur. Mais comment être certain que ce serveur est digne de confiance?

Lorsqu’une connexion TLS s’établit entre un client et un serveur, vient un moment où le serveur doit faire parvenir sa clé publique au client : cette clé est transmise en même temps qu’un ensemble d’informations qu’on appelle le certificat numérique. Ces informations sont par exemple le nom de l’entreprise, le nom de domaine associé, une adresse, un email, etc.

Pour la majorité des sites HTTPS, le certificat n’est pas émis par l’instance qui émet la clé publique, mais par une autorité de certification, qui est une entreprise qui atteste de l’authenticité de l’émetteur.

Les autorités de certification émettent elles-mêmes une paire de clés publique et privée, et distribuent leur clé publique dans un certificat. Les fabricants de logiciels incluent ces certificats “racine” dans leurs applications (par exemple Firefox, IE, etc.).

Une entreprise X désirant obtenir un certificat pour opérer un site HTTPS enverra ses coordonnées et sa clé publique à un autorité de certification, qui fera les vérifications nécessaires pour valider son identité. Si l’entreprise est digne de confiance, l’autorité émet un certificat qu’elle signe avec sa propre clé privée. Elle envoie ensuite ce certificat à l’entreprise.

Un client qui se connecte au site web de l’entreprise X reçoit donc ce certificat, qui comprend la clé publique de l’entreprise, des informations qui la concernent, et un message chiffré avec la clé privée de l’autorité de certification. Le client peut donc valider l’authenticité de la clé publique jointe au certificat s’il est capable de déchiffrer la signature avec la clé publique de l’autorité de certification.

Cette séquence d’évènements est représentée dans le schéma suivant :

certificat

Comprendre le chiffrement SSL / TLS avec des emojis (et le HTTPS)

Tutoriel Sécuriser son ordinateur : Les certificats de chiffrement

Exercice – Génération d’un certificat autosigné avec OpenSSL
En règle générale, lorsqu’on a besoin d’un certificat, on doit faire une demande auprès d’une autorité de certification ; mais dans certains cas exceptionnels (par exemple, un serveur intranet à accès privé, un serveur de courrier d’entreprise, etc.), on n’a pas besoin de faire attester l’authenticité de notre certificat. Dans ces cas, on signera numériquement nous-mêmes notre certificat. C’est ce qu’on appelle un certificat autosigné. OpenSSL fournit les outils nécessaires pour générer et signer nous-mêmes des certificats numériques.

Générer la clé privée du serveur
La première étape consiste à générer la clé privée du serveur ; le certificat contiendra la clé publique correspondante. La commande est la suivante ; par défaut, la clé a une taille de 512 bits, mais ici, on la définit sur 1024 bits :

openssl genrsa -out certif.key 1024
Générer la demande de certificat
Ensuite, nous générons un fichier contenant les informations nécessaires pour créer notre certificat :

openssl req -new -key certif.key -out certif.req
L’option -new spécifie que nous créons un nouveau certificat, ce qui entraînera l’affichage de questions pour l’utilisateur.

L’option -key indique la clé privée associée au certificat, car une clé publique est générée au moment de la création de la demande.

L’option -out détermine le nom du fichier de sortie.

Après avoir répondu à plusieurs questions pour identifier le serveur, vous pouvez constater que la demande est encodée en base-64. Pour observer le contenu de votre demande, vous pouvez exécuter la commande suivante :

openssl req -text -noout -verify -in certif.req
Vous pouvez constater que la clé publique correspondant à votre clé privée est incluse dans la demande.

Générer et signer le certificat
Enfin, la dernière étape consiste à générer le certificat de la manière suivante :

openssl x509 -req -days 365 -in certif.req -signkey certif.key -out certif.crt
L’option -x509 définit la norme de création de certificats utilisée.
L’option -req signifie que nous traitons une demande de certificat existante.
L’option -days définit la durée de validité de la signature.
L’option -signkey spécifie la clé privée utilisée pour générer la signature.
Une fois cette commande exécutée, votre certificat sera généré. Vous pouvez afficher son contenu en utilisant la commande suivante :

openssl x509 -in certif.crt -text -noout -->


