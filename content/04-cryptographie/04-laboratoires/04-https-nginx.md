+++
title = '4- HTTPS'
weight = '443'
draft = false
+++
-------------

## Objectifs

- configurer un serveur web (Nginx) pour utiliser un certificat TLS;
- activer HTTPS;
- associer une clé privée à un certificat;
- vérifier une configuration Nginx;
- tester une connexion HTTPS;
- comprendre le fonctionnement d'un certificat autosigné dans un serveur Web.

Vous utiliserez la même VM2 que dans les laboratoires précédents.

<!-- ```text
                    ┌─────────────────┐
                    │      VM1        │
                    │                 │
                    │ Client SSH/Web  │
                    └────────┬────────┘
                             │
                       HTTP / HTTPS
                             │
                             ▼
                    ┌─────────────────┐
                    │      VM2        │
                    │                 │
                    │ Nginx           │
                    │                 │
                    │ Pokédex         │
                    └─────────────────┘
``` -->

Avant le laboratoire, le site fonctionne en `HTTP`. Votre objectif est de permettre également `HTTPS`

##### 1. Vérifier le fonctionnement actuel

Depuis VM1, ouvrez :

```text
http://<IP_VM2>
```

Vous devriez voir le site Web **Pokédex**.

##### 2. Vérifier la configuration actuelle de Nginx

Sur VM2 :

```bash
sudo nginx -t
```

Puis :

```bash
sudo systemctl status nginx
```

Identifiez le fichier de configuration utilisé par le serveur Web.

Sur Ubuntu, vous pouvez notamment examiner :

```bash
ls -l /etc/nginx/sites-enabled/
```

et :

```bash
ls -l /etc/nginx/sites-available/
```

---

##### 3. Ajouter HTTPS

Ouvrez le fichier de configuration du site :

```bash
sudo nano /etc/nginx/sites-available/default
```

Dans le bloc `server`, ajoutez une configuration HTTPS.

Par exemple :

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name <nom-du-serveur>;

    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    root <répertoire-du-site>;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Adaptez :

```text
<nom-du-serveur>
```

et :

```text
<répertoire-du-site>
```

à la configuration existante du serveur.

{{%notice style="note" title="Attention"%}}
Ne remplacez pas aveuglément la configuration existante du site. Ajoutez ou adaptez le bloc HTTPS en conservant les paramètres nécessaires au fonctionnement du Pokédex.
{{%/notice%}}

##### 4. Vérifier la configuration

Avant de redémarrer Nginx :

```bash
sudo nginx -t
```

Vous devriez obtenir :

```text
syntax is ok
test is successful
```

Si une erreur apparaît, **ne redémarrez pas Nginx**. Corrigez d'abord la configuration.

##### 5. Redémarrer Nginx

```bash
sudo systemctl restart nginx
```

Vérifiez ensuite :

```bash
sudo systemctl status nginx
```

##### 6. Tester HTTPS

Depuis VM1 :

```bash
curl -k https://<IP_VM2>
```

L'option :

```text
-k
```

demande à `curl` d'accepter un certificat qui n'est pas reconnu par une autorité de certification de confiance.

Vous devriez recevoir le contenu du site Pokédex.


##### 7. Tester avec un navigateur

Dans le navigateur de VM1, ouvrez :

```text
https://<IP_VM2>
```

Vous devriez voir le site.

Le navigateur devrait toutefois afficher un avertissement concernant le certificat.

C'est **normal** dans ce laboratoire.

Pourquoi ?

Parce que le certificat a été créé par vous-même :

```text
VM2
 │
 └── certificat autosigné
```

Il n'a pas été signé par une autorité de certification reconnue par le navigateur.

##### 8. Examiner le certificat depuis le navigateur

Affichez les informations du certificat.

Vérifiez notamment :

- le sujet;
- l'émetteur;
- la période de validité;
- la clé publique;
- le nom du serveur;
- le fait que le certificat soit autosigné.

Comparez ces informations avec celles obtenues au laboratoire 3.

###### **Questions**
1. Pourquoi le navigateur affiche-t-il un avertissement ?
2. Le certificat est-il nécessairement faux ou mal formé parce qu'il est autosigné ?
3. Quelle différence y a-t-il entre un certificat autosigné et un certificat signé par une CA reconnue ?
4. Pourquoi le certificat contient-il une clé publique ?
5. Où se trouve la clé privée utilisée par Nginx ?
6. Pourquoi cette clé doit-elle rester secrète ?
7. Quel est le rôle du certificat dans une connexion HTTPS ?
8. Quel est le rôle de la clé privée dans le serveur Nginx ?
9. Pourquoi votre navigateur ne considère-t-il pas automatiquement votre certificat autosigné comme digne de confiance ?
10. Dans un environnement réel, comment pourrait-on remplacer le certificat autosigné utilisé dans ce laboratoire ?