+++
title = 'Laboratoires'
weight = '760'
draft = false
+++
---------------


<!-- # 7.11 - Laboratoires

| Laboratoire | Objectifs |
|---|---|
| Injection SQL | 1 h 15 |
| XSS et vol de session | 1 h 30 |
| CSRF | 1 h |
| Revue de code | 45 min (ou devoir) | -->

##### [Laboratoire 1 - Injection SQL]({{% ref "07-securite-des-applications/6-laboratoires/1-injection-sql" %}})

###### Objectifs 
- comprendre comment une entrée modifie la logique d'une requête ;
- constater les conséquences d'un mauvais stockage des mots de passe ;
- identifier la correction (requête préparée).

##### [Laboratoire 2 - XSS]({{% ref "07-securite-des-applications/6-laboratoires/2-xss" %}})

###### Objectifs 
- exploiter un XSS réfléchi et un XSS stocké ;
- réaliser un vol de session ;
- comprendre le rôle de l'encodage des sorties et des attributs de cookies.

##### [Laboratoire 3 - CSRF]({{% ref "07-securite-des-applications/6-laboratoires/3-csrf" %}})
###### Objectifs 
- comprendre pourquoi le cookie de session est envoyé automatiquement ;
- réaliser une attaque CSRF à l'aide d'une page piégée ;
- identifier les contre-mesures dans le code.

##### [Laboratoire 4 - Extraits de code]({{% ref "07-securite-des-applications/6-laboratoires/4-revue-de-code" %}})

## Mise en place

Les laboratoires utilisent **DVWA** (*Damn Vulnerable Web Application*), une application **volontairement vulnérable** conçue pour l'enseignement, installée sur une des **VM des laboratoires du chapitre 4**. Vous y accédez depuis votre poste à l'adresse `http://<IP_VM>:4280`.

DVWA propose plusieurs niveaux de sécurité (`Low`, `Medium`, `Impossible`) et un bouton **View Source** pour comparer le code vulnérable et le code corrigé.

1. Connectez-vous avec `admin` / `password`.
2. Menu **Setup / Reset DB** → **Create / Reset Database**, puis reconnectez-vous.
3. Menu **DVWA Security** → niveau **Low**.

{{%notice style="warning" title="Important"%}}
DVWA ne doit jamais être exposée hors du réseau du laboratoire. Tester ces techniques sur tout autre site, y compris celui du cégep, est **interdit**. En cas de problème, utilisez **Create / Reset Database**.
{{%/notice%}}

{{%notice style="tip" title="Lire le code source de DVWA"%}}
DVWA est écrite en PHP. Voici en plus les éléments que vous rencontrerez dans **View Source** : 
<!-- Relisez l'encadré [*Lire du PHP : l'essentiel* de l'annexe)-->
| Code | Signification |
|---|---|
| `$id = $_REQUEST['id'];` | Récupère la valeur saisie dans le champ `id` |
| `"... WHERE user_id = '$id';"` | Entre guillemets doubles, `$id` est **remplacé par sa valeur** : c'est une concaténation |
| `mysqli_query($conn, $query)` | Exécute la requête SQL contenue dans `$query` |
| `is_numeric($id)` | Vérifie que la valeur est un nombre |
| `$db->prepare(...)` puis `bindParam(':id', $id, PDO::PARAM_INT)` | Requête préparée : associe `$id` au paramètre `:id`, **en tant qu'entier** |
| `$data->rowCount()` | Nombre de lignes retournées par la requête |
| `htmlspecialchars(...)` | Encode les caractères spéciaux HTML (section 7.2) |
| `checkToken(...)` / `generateSessionToken()` | Vérifie / crée un **jeton anti-CSRF** (section 7.3) |
{{%/notice%}}

<!-- {{%notice style="note" title="Pour l'enseignant — installation sur VM2"%}}
```bash
git clone https://github.com/digininja/DVWA.git
cd DVWA
# Dans compose.yml, remplacer "127.0.0.1:4280:80" par "4280:80"
# pour rendre DVWA accessible depuis VM1
docker compose up -d
```
{{%/notice%}} -->



