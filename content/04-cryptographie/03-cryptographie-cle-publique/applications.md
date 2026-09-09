+++
title = 'Applications'
weight = '431'
draft = false
+++
-------------

La cryptographie à clé publique est très utile et possède de nombreuses applications : **échange de clés, signatures numériques, authentification, certificats**, etc. Elle est cependant beaucoup plus coûteuse en termes de ressources que la cryptographie symétrique.

C’est pourquoi les deux types de cryptographie sont aujourd’hui utilisés de manière **complémentaire** : la cryptographie asymétrique permet notamment d'établir une relation de confiance ou d’échanger une clé, tandis que la cryptographie symétrique est ensuite utilisée pour chiffrer efficacement les données.

## Échange de clés

L’une des applications de la cryptographie à clé publique est de permettre l’**échange sécurisé d’une clé symétrique** entre deux interlocuteurs.

Par exemple, RSA peut être utilisé pour chiffrer une clé symétrique et la transmettre de manière confidentielle entre deux participants. Dans ce contexte, RSA constitue une autre approche que l’échange de clés Diffie-Hellman présenté précédemment.

Le principe est le suivant :

![Échange d'une clé de session avec RSA](/images/04-rsa-echange-cle.png)

A et B s’échangent d’abord leurs clés publiques. Cette étape peut être réalisée une seule fois.

Ensuite, chaque fois que A et B souhaitent communiquer, l’un des deux participants (ici, A) génère une **clé de session** aléatoire pour cette communication :

1. A génère une clé de session.
2. A chiffre cette clé avec la clé publique de B.
3. B reçoit la clé chiffrée et utilise sa clé privée pour la déchiffrer.
4. A et B possèdent maintenant la même clé de session.
5. Ils peuvent utiliser cette clé avec un algorithme symétrique comme **AES** ou **ChaCha20** pour chiffrer leurs communications.

La cryptographie asymétrique n’est donc pas nécessairement utilisée pour chiffrer l’ensemble de la communication. Elle sert plutôt à **établir ou transmettre de manière sécurisée les éléments nécessaires à une communication symétrique**.

{{%notice style="tip" title="À retenir"%}}
La cryptographie asymétrique permet de résoudre le problème de l’échange initial, tandis que la cryptographie symétrique permet de chiffrer efficacement les données.
{{%/notice%}}

## Authentification

La cryptographie à clé publique peut également être utilisée à des fins d’**authentification**.

Sur de nombreux sites, l’authentification repose sur un mot de passe. Celui-ci constitue une preuve d’identité parce qu’il est supposé être **secret et connu uniquement de son propriétaire**. 

Cependant, les mots de passe peuvent être faibles, réutilisés ou compromis et sont donc vulnérables à différentes attaques.

Une autre approche consiste à utiliser une **paire de clés publique et privée**.

Comme un mot de passe, la clé privée est un secret que son propriétaire doit conserver et ne jamais divulguer. Elle possède toutefois des caractéristiques qui la rendent particulièrement difficile à deviner :

+ Elle est **générée aléatoirement**.
+ Elle possède généralement une **taille importante**.
+ Elle n’a pas besoin d’être mémorisée par l’utilisateur.

La clé privée peut ainsi servir à **prouver la possession d’une identité cryptographique** sans avoir à transmettre le secret lui-même.

Le principe repose sur la **signature numérique** :

![Authentification avec une paire de clés](/images/04-authentification_A_B.png)

Supposons que A souhaite s’authentifier auprès de B et que B possède déjà la **clé publique de A**.

1. B génère un **défi aléatoire**.
2. B transmet ce défi à A.
3. A signe le défi avec sa **clé privée**.
4. A transmet la signature à B.
5. B vérifie la signature à l’aide de la **clé publique de A**.
6. Si la signature est valide, B peut vérifier que le message a bien été signé avec la clé privée correspondante.

A n’a donc jamais besoin de transmettre sa clé privée à B.

{{%notice style="tip" title="Pourquoi utiliser un défi aléatoire ?"%}}
Le message utilisé pour l’authentification doit être différent à chaque tentative. On utilise généralement un **nombre aléatoire appelé nonce** afin d’éviter qu’un attaquant puisse simplement enregistrer une ancienne réponse valide et la réutiliser plus tard.
{{%/notice%}}

## Signature numérique

L’authentification n’est qu’une des applications des **signatures numériques**.

Une signature numérique permet notamment de vérifier :

+ **l’authenticité :** le message a été signé par le détenteur de la clé privée ;
+ **l’intégrité :** le message n’a pas été modifié depuis sa signature ;
+ **la non-répudiation**, dans certains contextes : le signataire ne peut pas facilement nier avoir produit la signature, sous réserve des conditions juridiques et techniques applicables.

Le fonctionnement est différent du chiffrement utilisé pour assurer la confidentialité :

+ Pour assurer la **confidentialité**, on chiffre avec la **clé publique du destinataire** et seul le détenteur de la clé privée correspondante peut déchiffrer.
+ Pour produire une **signature**, le détenteur utilise sa **clé privée** et les autres participants utilisent sa **clé publique** pour vérifier la signature.

Dans la pratique, on ne signe généralement pas directement l’ensemble du message avec l’algorithme asymétrique. On calcule plutôt une **empreinte cryptographique** du message, puis cette empreinte est signée avec la clé privée.

![Principe général d'une signature numérique](/images/04-signature-numerique.png)

Ainsi, si le message est modifié après sa signature, son empreinte ne correspondra plus à celle utilisée lors de la signature et la vérification échouera.

{{%notice style="tip" title="À retenir"%}}
+ **Clé publique du destinataire → chiffrement → confidentialité**

+ **Clé privée du signataire → signature → authenticité et intégrité**
{{%/notice%}}


## Certificats et infrastructure à clé publique (PKI)

Un dernier problème demeure : **comment savoir à qui appartient réellement une clé publique ?**

Une clé publique peut être distribuée librement, mais un attaquant pourrait tenter de remplacer la clé publique d’un utilisateur par la sienne afin de se faire passer pour lui.

C’est précisément le problème auquel répondent les **certificats numériques**.

Un certificat permet notamment d’associer une **identité** à une **clé publique**. Cette association est validée par une **autorité de certification (CA)**, qui signe elle-même le certificat.

L’ensemble des mécanismes, technologies et organisations permettant de gérer ces clés et certificats constitue une **infrastructure à clé publique (PKI — Public Key Infrastructure)**.

Les certificats et la PKI jouent notamment un rôle essentiel dans **HTTPS**, où ils permettent au navigateur de vérifier l’identité du serveur avant d’établir une communication sécurisée.

{{%notice style="info" title="Le rôle de la PKI"%}}
La cryptographie à clé publique permet de résoudre plusieurs problèmes, mais elle ne permet pas à elle seule de savoir **à qui appartient une clé publique**.

La **PKI** ajoute une infrastructure de confiance permettant d’associer une clé publique à une identité grâce aux **certificats numériques** et **aux autorités de certification**.
{{%/notice%}}