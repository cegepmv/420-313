+++
title = 'Certificats numériques'
weight = '434'
draft = false
+++
-------------


Un problème demeure : **comment savoir à qui appartient réellement une clé publique ?**

Une clé publique peut être distribuée librement, mais un attaquant pourrait tenter de remplacer la clé publique d'un utilisateur par la sienne afin de se faire passer pour lui.

Un certificat numérique permet notamment de créer une association vérifiable entre une **identité** et une **clé publique**.

Un certificat contient notamment des informations comme :

- le nom du domaine ou l'identité concernée ;
- la clé publique ;
- la période de validité ;
- l'identité de l'autorité ayant émis le certificat ;
- la signature de l'autorité de certification.

```text

Identité
   │
   ├── Nom / domaine
   ├── Période de validité
   └── Clé publique

```

Le certificat est signé par une **autorité de certification (CA — Certificate Authority)**.

La signature de la CA permet au client de vérifier que le certificat n'a pas été modifié et qu'il a été émis par une autorité reconnue.

## Exemple simplifié

Une autorité de certification possède elle-même une paire de clés :

```text

CA
│
├── Clé privée
└── Clé publique

```

Lorsqu'un serveur obtient un certificat, celui-ci contient notamment :
```text


        ┌────────────────────────────────┐
        │       Certificat du serveur    │
        │                                │
        │ Domaine                        │
        │ Clé publique                   │
        │ Validité                       │
        │ Émetteur                       │
        │ Signature de la CA             │
        └────────────────────────────────┘
```

La CA utilise sa clé privée pour signer le certificat.

Le navigateur utilise ensuite la clé publique de la CA pour vérifier cette signature.

```text

                 Autorité de certification
                          │
                    clé privée
                          │
                          ▼
                  Signature du certificat
                          │
                          ▼
                Certificat du serveur
                          │
                          ▼
                      Navigateur
                          │
                  vérifie la signature
                          │
                          ▼
                    Certificat valide
                    
```

{{%notice style="info" title="Le rôle d'un certificat"%}}

Un certificat ne signifie pas simplement « ce site est fiable ».

Il permet notamment d'établir une **association vérifiable entre une identité et une clé publique**, selon les règles de validation de l'autorité de certification.

{{%/notice%}}

