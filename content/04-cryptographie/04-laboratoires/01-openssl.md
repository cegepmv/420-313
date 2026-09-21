+++
title = 'Laboratoire OpenSSL'
weight = '441'
draft = true
+++
-------------
## Certificat autosigné avec OpenSSL

En règle générale, lorsqu’un serveur doit obtenir un certificat reconnu publiquement, celui-ci est émis par une **autorité de certification**.

Dans certains contextes, notamment un laboratoire, un environnement de développement ou un serveur interne, il peut être utile de générer soi-même un certificat.

Un **certificat autosigné** est un certificat signé par sa propre clé privée plutôt que par une autorité de certification.

OpenSSL fournit les outils nécessaires pour générer et examiner des certificats numériques.

{{%notice style="warning" title="Certificat autosigné"%}}

Un certificat autosigné permet de comprendre le fonctionnement d'un certificat, mais il ne bénéficie pas automatiquement de la confiance des navigateurs et des systèmes.

Il est donc particulièrement adapté aux **laboratoires, tests et environnements internes**.

{{%/notice%}}

## Générer une clé privée

La première étape consiste à générer la clé privée du serveur.
```bash
openssl genrsa -out certif.key 2048
```
La clé privée doit être conservée secrète et protégée contre tout accès non autorisé.

## Générer une demande de certificat

Ensuite, nous générons une demande de certificat :
```bash
openssl req -new -key certif.key -out certif.csr
```
Cette commande crée un fichier contenant notamment les informations nécessaires à la création du certificat ainsi que la clé publique correspondant à la clé privée.

## Vérifier la demande

Pour afficher le contenu de la demande :
```bash
openssl req -text -noout -verify -in certif.csr
```
On peut notamment observer les informations du sujet et la clé publique.

## Générer le certificat autosigné

Enfin, nous générons le certificat :
```bash
openssl x509 -req -days 365 \
    -in certif.csr \
    -signkey certif.key \
    -out certif.crt
```
La clé privée du serveur est utilisée ici pour signer le certificat.

Le fichier `certif.crt` contient notamment :

- les informations du certificat ;
- la clé publique ;
- la période de validité ;
- la signature ;
- les informations relatives à l'émetteur.

## Afficher le certificat

Pour afficher son contenu :
```bash
openssl x509 -in certif.crt -text -noout
```

Vous devriez notamment être capables d'identifier :

- le **sujet** ;
- l'**émetteur** ;
- la **période de validité** ;
- la **clé publique** ;
- l'**algorithme de signature** ;
- la **signature du certificat**.

{{%notice style="tip" title="Lien avec la PKI"%}}

Dans ce laboratoire, le certificat est signé par le serveur lui-même.

Dans une PKI réelle, le certificat d'un serveur est généralement signé par une **autorité de certification**.

Le laboratoire permet donc de comprendre le fonctionnement d'un certificat avant d'introduire la notion de **chaîne de confiance**.

{{%/notice%}}