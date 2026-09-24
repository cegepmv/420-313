+++
title = '3- Certificat autosigné'
weight = '443'
draft = false
+++
-------------


## Objectifs

- générer une clé privée avec OpenSSL;
- générer un certificat X.509;
- comprendre le lien entre une clé privée, une clé publique et un certificat;
- créer un certificat autosigné;
- examiner le contenu d'un certificat avec OpenSSL.


##### 1. Vérifier la présence d'OpenSSL

Sur **VM2** :

```bash
openssl version
```

Vous devriez obtenir une version d'OpenSSL.

##### 2. Créer les répertoires nécessaires

Vérifiez que les répertoires existent :

```bash
ls /etc/ssl/private
ls /etc/ssl/certs
```

Nous utiliserons `/etc/ssl/private/` pour la clé privée et `/etc/ssl/certs/` pour le certificat.


##### 3. Générer le certificat autosigné

Dans ce laboratoire, nous allons générer simultanément :

- une clé privée RSA;
- un certificat X.509;
- un certificat autosigné.

Utilisez :

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/nginx-selfsigned.key \
  -out /etc/ssl/certs/nginx-selfsigned.crt
```

OpenSSL vous demandera différentes informations.

Entrez notamment :

```text
Country Name (2 letter code) [AU]: CA
State or Province Name (full name) [Some-State]: Quebec
Locality Name (eg, city) []: Montreal
Organization Name (eg, company) [Internet Widgits Pty Ltd]: Cégep Marie-Victorin
Organizational Unit Name (eg, section) []: Département Techniques de l'informatique
Common Name (e.g. server FQDN or YOUR name) []: <nom du serveur>
Email Address []: <votre adresse courriel du cégep>
```


{{%notice style="tip" title="Pourquoi utiliser `-nodes`?"%}}

L'option :

```text
-nodes
```

demande à OpenSSL de ne pas chiffrer la clé privée avec une passphrase.

Cela simplifie l'utilisation de la clé par Nginx, car Nginx doit pouvoir lire automatiquement sa clé privée lorsqu'il démarre.

Dans un environnement réel, la protection de la clé privée doit être étudiée avec soin.
{{%/notice%}}

##### 4. Vérifier les fichiers créés

```bash
sudo ls -l /etc/ssl/private/nginx-selfsigned.key
sudo ls -l /etc/ssl/certs/nginx-selfsigned.crt
```

Vous avez maintenant :

```text
nginx-selfsigned.key
        │
        └── clé privée

nginx-selfsigned.crt
        │
        └── certificat X.509 autosigné
```

##### 5. Examiner le certificat

Utilisez :

```bash
sudo openssl x509 \
  -in /etc/ssl/certs/nginx-selfsigned.crt \
  -text \
  -noout
```

Repérez notamment :

- `Subject`
- `Issuer`
- `Validity`
- `Not Before`
- `Not After`
- `Public Key Algorithm`
- `Public-Key`
- `Signature Algorithm`

###### **Questions**

1. Qui est le sujet (`Subject`) du certificat ?
2. Qui est l'émetteur (`Issuer`) ?
3. Pourquoi le sujet et l'émetteur sont-ils identiques dans ce certificat ?
4. Quelle est la période de validité du certificat ?
5. Quel algorithme est utilisé pour la clé publique ?
6. Quelle est la taille de la clé ?
7. Quel algorithme est utilisé pour signer le certificat ?
8. Quelle différence y a-t-il entre le certificat et la clé privée ?

##### 6. Vérifier la clé privée

Vous pouvez examiner les informations de la clé avec :

```bash
sudo openssl rsa \
  -in /etc/ssl/private/nginx-selfsigned.key \
  -text \
  -noout
```

{{%notice style="warning" title="Attention"%}}
**Ne transmettez jamais le contenu complet de cette clé à une autre personne.**
{{%/notice%}}

##### 7. Ajouter un nom de domaine au certificat

Dans les certificats modernes, le nom du serveur doit normalement être présent dans l'extension **Subject Alternative Name (SAN)**.

Si votre environnement utilise un nom comme :

```text
benachourgha-pokedex.lan
```

vous pouvez générer un certificat avec :

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/nginx-selfsigned.key \
  -out /etc/ssl/certs/nginx-selfsigned.crt \
  -subj "/C=CA/ST=Quebec/L=Montreal/O=Cegep Marie-Victorin/OU=Departement Techniques de l'informatique/CN=webghazi" \
  -addext "subjectAltName=DNS:benachourgha-pokedex.lan,IP:10.10.2.251"
```

Si vous utilisez directement l'adresse IP du serveur, vous pouvez plutôt ajouter :

```text
-addext "subjectAltName=IP:<IP_VM2>"
```

Vous pouvez aussi avoir les deux :

```bash
-addext "subjectAltName=DNS:<nom-de-domaine-du-serveur>,IP:<IP_VM2>"
```

##### 8. Vérifier le SAN

```bash
sudo openssl x509 \
  -in /etc/ssl/certs/nginx-selfsigned.crt \
  -text \
  -noout
```

Cherchez :

```text
X509v3 Subject Alternative Name:
```

Vous devriez voir le nom et/ou l'adresse IP utilisés.
