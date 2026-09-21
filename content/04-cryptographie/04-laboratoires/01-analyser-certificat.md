+++
title = '1- Analyser un certificat'
weight = '442'
draft = false
+++

## Objectifs

- identifier les principales informations contenues dans un certificat numérique;
- identifier l'autorité de certification (CA) qui a signé un certificat;
- déterminer la période de validité d'un certificat;
- identifier le nom de domaine associé à un certificat;
- identifier les algorithmes utilisés;
- comprendre le rôle d'un certificat dans une connexion HTTPS.

##### 1. Choisir un site HTTPS

Choisissez un site Web utilisant **HTTPS**.

Par exemple :

```text
https://www.wikipedia.org
```

##### 2. Examiner le certificat avec le navigateur

Ouvrez le site dans votre navigateur, cliquez sur l'icône indiquant que la connexion est sécurisée, généralement représentée par un **cadenas** à gauche de l'adresse du site.

Accédez ensuite aux informations détaillées du certificat.

##### 3. Relever les informations du certificat

Complétez le tableau suivant à partir du certificat du site choisi.

| Information | Réponse |
|---|---|
| Nom du site | |
| Nom du certificat / sujet (Subject) | |
| Autorité ayant émis le certificat (Issuer) | |
| Date de début de validité | |
| Date d'expiration | |
| Algorithme de signature | |
| Algorithme de clé publique | |
| Taille de la clé publique | |
| Nom(s) de domaine dans le certificat (SAN) | |
| Le certificat est-il autosigné ? | |
| Le certificat est-il actuellement valide ? | |

<!-- ## 4. Comprendre la chaîne de confiance

Un certificat de serveur n'est généralement pas directement signé par une autorité racine.

Une chaîne de certificats peut ressembler à ceci :

```text
Autorité racine
      │
      ▼
Autorité intermédiaire
      │
      ▼
Certificat du serveur
      │
      ▼
www.exemple.com
```

À partir des informations affichées par votre navigateur, essayez d'identifier :

1. le certificat du serveur;
2. l'autorité intermédiaire;
3. l'autorité racine, si elle est affichée. -->

###### **Questions**
1. Quelle organisation a émis le certificat du serveur ?
2. Pourquoi le navigateur fait-il confiance à cette autorité ?
3. Quelle est la différence entre l'**émetteur (Issuer)** et le **sujet (Subject)** d'un certificat ?
4. À quoi sert la date d'expiration d'un certificat ?
5. Pourquoi un certificat doit-il contenir le nom du domaine auquel il s'applique ?
6. Que se passerait-il si vous consultiez un site dont le certificat était expiré ?
7. Quelle différence y a-t-il entre un certificat signé par une CA reconnue et un certificat autosigné ?

<!-- 
## 1. Quelle organisation a émis le certificat du serveur ?
L'organisation exacte dépend du site web que vous visitez. Dans la majorité des cas sur le web actuel, il s'agit d'Autorités de Certification (CA) majeures comme Let's Encrypt, DigiCert, Sectigo (Comodo) ou GlobalSign. Pour le savoir précisément, il faut cliquer sur le cadenas à gauche de l'URL dans votre navigateur et afficher les détails du certificat.
## 2. Pourquoi le navigateur fait-il confiance à cette autorité ?
Le navigateur fait confiance à cette autorité parce qu'elle est inscrite dans son magasin de certificats de confiance (Trust Store). Les systèmes d'exploitation (Windows, macOS) et les navigateurs (Chrome, Firefox) intègrent nativement une liste de certificats "Racines" (Root Certificates) appartenant à des organisations auditées et jugées hautement sécurisées.
## 3. Quelle est la différence entre l'émetteur (Issuer) et le sujet (Subject) d'un certificat ?

* L'Émetteur (Issuer) est l'autorité de confiance qui a vérifié l'identité et signé le certificat (la CA, par exemple DigiCert).
* Le Sujet (Subject) est le titulaire du certificat, c'est-à-dire l'entité ou le site web qui est sécurisé (par exemple, www.wikipedia.org).

## 4. À quoi sert la date d'expiration d'un certificat ?
La date d'expiration sert à limiter la validité dans le temps pour deux raisons de sécurité majeures :

* Vérification régulière : Elle force le propriétaire du site à prouver périodiquement qu'il contrôle toujours le domaine.
* Sécurité cryptographique : Si les clés de chiffrement du certificat venaient à être compromises ou cassées en secret, la date d'expiration garantit que le certificat obsolète deviendra automatiquement inutilisable après un certain délai.

## 5. Pourquoi un certificat doit-il contenir le nom du domaine auquel il s'applique ?
Il doit contenir le nom de domaine (dans le champ Common Name ou Subject Alternative Name) pour empêcher l'usurpation d'identité. Lorsque vous tapez banque.com, votre navigateur vérifie que le certificat envoyé par le serveur mentionne explicitement banque.com. Si ce n'était pas le cas, un pirate pourrait utiliser un certificat valide de son propre site (pirate.com) pour intercepter vos connexions sur le site de votre banque.
## 6. Que se passerait-il si vous consultiez un site dont le certificat était expiré ?
Votre navigateur bloquerait l'accès immédiat et afficherait une page d'avertissement de sécurité rouge ou intermédiaire (du type "Votre connexion n'est pas privée" ou "Risque de sécurité potentiel"). Le chiffrement fonctionnerait toujours techniquement, mais le navigateur ne peut plus garantir que le site appartient toujours à son propriétaire légitime.
## 7. Quelle différence y a-t-il entre un certificat signé par une CA reconnue et un certificat autosigné ?

| Caractéristique | Certificat signé par une CA reconnue | Certificat auto-signé |
|---|---|---|
| Créateur | Une autorité publique et auditée | Un administrateur système ou vous-même |
| Confiance | Automatique par tous les navigateurs | Rejeté avec une alerte de sécurité |
| Usage | Sites web publics, e-commerce, applications de production | Environnements de test, développement local, réseaux d'entreprise internes |

Souhaitez-vous que nous examinions ensemble le certificat d'un site spécifique pour identifier son émetteur, ou voulez-vous savoir comment générer un certificat auto-signé pour vos propres tests ?

-->

##### 4. Observer le certificat avec OpenSSL

Il est également possible d'obtenir des informations sur un certificat directement à partir d'un terminal.

La commande suivante permet d'établir une connexion TLS avec un serveur :

```bash
openssl s_client -connect www.wikipedia.org:443 -servername www.wikipedia.org
```

La sortie contient beaucoup d'informations.

Pour obtenir uniquement les informations principales du certificat, utilisez :

```bash
openssl s_client -connect www.wikipedia.org:443 -servername www.wikipedia.org </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

Vous devriez obtenir des informations similaires à :

```text
subject=...
issuer=...
notBefore=...
notAfter=...
```

###### **Questions**

8. Quelle information correspond au sujet du certificat ?
9. Quelle information correspond à l'autorité de certification ?
10. Que représentent `notBefore` et `notAfter` ?