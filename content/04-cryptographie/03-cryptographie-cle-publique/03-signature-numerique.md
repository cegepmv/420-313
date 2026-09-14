+++
title = 'Signature numérique'
weight = '433'
draft = false
+++
-------------


L’authentification n’est qu’une des applications des **signatures numériques**.

Une signature numérique permet notamment de vérifier :

+ **l'authenticité :** la signature a été produite à l'aide de la clé privée associée au signataire ;

+ **l'intégrité :** les données n'ont pas été modifiées depuis leur signature ;

+ **la non-répudiation**, dans certains contextes, lorsque les conditions techniques et juridiques appropriées sont réunies.

Une signature numérique utilise une paire de clés :
```text
              Clé privée
                  │
                  ▼
Message ──► Hachage ──► Signature
                         │
                         ▼
              Message + signature
```
Le destinataire utilise ensuite la clé publique correspondante pour vérifier la signature :
```text

Message ──► Hachage ───────────┐
                              │
Signature ──► Vérification ◄──┘
                              │
                         Valide / invalide
```

Le principe général est donc :

1. Le signataire calcule une empreinte du message.
2. Il utilise sa clé privée dans l'algorithme de signature pour produire une signature.
3. Il transmet le message accompagné de la signature.
4. Le destinataire utilise la clé publique du signataire pour vérifier la signature.
5. Le destinataire recalcule l'empreinte du message et vérifie que la signature correspond.

Si le message est modifié après sa signature, la vérification échouera.


{{%notice style="warning" title="Signature ≠ chiffrement"%}}

Une signature numérique ne sert pas à cacher le contenu du message.

Le message peut rester parfaitement lisible :
```text
Message en clair + signature
```
La signature permet plutôt de vérifier **qui a signé le message** et **si celui-ci a été modifié**.

Pour assurer la confidentialité, on utilise un mécanisme de chiffrement.

{{%/notice%}}

## Exemples d'algorithmes de signature

Parmi les algorithmes utilisés pour les signatures numériques, on retrouve notamment :

- **RSA-PSS**
- **ECDSA**
- **Ed25519**

Le fonctionnement précis varie selon l'algorithme, mais le principe général reste le même : une signature est produite avec la clé privée et vérifiée avec la clé publique correspondante.

{{%notice style="tip" title="À retenir"%}}

+ **Clé publique du destinataire → chiffrement → confidentialité**

+ **Clé privée du signataire → signature → authenticité et intégrité**
{{%/notice%}}