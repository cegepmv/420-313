# 7. Sécurité des applications
---------

Les chapitres précédents ont porté sur la protection des **données** (chapitre 4), des **identités** (chapitre 5) et des **postes de travail** (chapitre 6). Ce chapitre s'intéresse aux **applications**, en particulier les **applications Web** : comment des erreurs de programmation permettent de lire une base de données, de voler une session ou de faire agir un utilisateur à son insu, et comment s'en protéger.

{{%notice style="warning" title="Cadre éthique et légal"%}}
Les techniques présentées sont étudiées **pour apprendre à s'en défendre**. Tester une attaque contre un système sans l'autorisation de son propriétaire est **illégal** (au Canada, notamment l'article 342.1 du *Code criminel*). Les manipulations se font **uniquement** dans l'environnement de laboratoire. L'exploitation approfondie sera reprise dans le cours *Piratage éthique*.
{{%/notice%}}

## Pourquoi la sécurité applicative est cruciale

Rappel du chapitre 2 : les produits et services informatiques **ne sont pas naturellement sûrs**. Une grande partie des **vulnérabilités** (chapitre 6) corrigées par les mises à jour sont des *erreurs de programmation*.

Une application Web est particulièrement exposée : elle est souvent **accessible publiquement**, elle reçoit des **données fournies par des inconnus** (formulaires, paramètres d'URL, cookies, fichiers) et elle est reliée à des **ressources sensibles** (base de données, comptes, fichiers).

![Surface d'attaque d'une application Web : les entrées contrôlées par l'attaquant (formulaires, paramètres d'URL, cookies, fichiers téléversés) entre le navigateur et le serveur](/images/07-surface-attaque.svg)

{{%notice style="info" title="Règle d'or : ne jamais faire confiance au client"%}}
Tout ce qui se trouve **dans le navigateur** (HTML, JavaScript, champs cachés, cookies, longueur maximale d'un champ...) peut être lu et modifié par l'utilisateur avec les outils de développement (**F12**).

Une validation en JavaScript améliore l'expérience utilisateur, mais **n'apporte aucune sécurité**. Toute vérification de sécurité doit être faite **côté serveur**.
{{%/notice%}}

## Rappels : cookies et sessions

Comme vu au **chapitre 5 (section 5.3)**, pour garder une trace d'authentification et éviter de s'authentifier à chaque requête, l'application crée une **session** dont l'identifiant est conservé dans un **cookie**.

<!-- Le protocole **HTTP** est **sans état** : chaque requête est indépendante.  -->

Le serveur crée le cookie avec l'en-tête `Set-Cookie` :

```YAML
Set-Cookie: SESSIONID=8f3a9c1e7b2d4f60a1c5; Secure; HttpOnly; SameSite=Lax
```

Ensuite, le navigateur le **renvoie automatiquement** à chaque requête vers ce site :

```YAML
Cookie: SESSIONID=8f3a9c1e7b2d4f60a1c5
```

{{%notice style="tip" title="Le point clé"%}}
**Quiconque possède le cookie de session est, aux yeux du serveur, l'utilisateur authentifié.** Et ce cookie est envoyé **automatiquement**, sans action de l'utilisateur. Plusieurs attaques de ce chapitre exploitent ces deux faits.
{{%/notice%}}

## Attributs de sécurité des cookies

| Attribut | Rôle | Protège contre |
|---|---|---|
| `Secure` | Cookie envoyé **uniquement en HTTPS** | Interception sur le réseau |
| `HttpOnly` | Cookie **inaccessible au JavaScript** | Vol par **XSS** |
| `SameSite=Strict` / `Lax` | Cookie non envoyé (ou envoyé seulement lors d'un simple clic de lien) quand la requête provient d'un **autre site** | **CSRF** |
| `Max-Age` / `Expires` | Durée de vie limitée | Réutilisation d'un cookie volé |

## Politique de même origine

Le navigateur applique une règle fondamentale : le code JavaScript d'une page ne peut **lire** les données d'une autre page que si elles proviennent de la **même origine**.
 
Une **origine** est définie par le triplet : **schéma + nom d'hôte + port**.
 
| URL comparée à `https://www.cegep.ca/page` | Même origine ? | Raison |
|---|---|---|
| `https://www.cegep.ca/autre/page` | ✅ Oui | Seul le chemin diffère |
| `http://www.cegep.ca/page` | ❌ Non | Schéma différent |
| `https://moodle.cegep.ca/page` | ❌ Non | Hôte différent |
| `https://www.cegep.ca:8443/page` | ❌ Non | Port différent |
 
Cette politique empêche un site malveillant de lire directement le contenu de votre boîte courriel ouverte dans un autre onglet. Nous verrons que deux attaques majeures la **contournent** :
 
- le **XSS**, en faisant exécuter du code malveillant **à l'intérieur** de l'origine légitime ;
- le **CSRF**, qui n'a pas besoin de *lire* la réponse : il lui suffit d'*envoyer* une requête.

# 7.1. Injection SQL
---------
Une **injection** survient lorsque des données fournies par l'utilisateur sont transmises à un **interpréteur** (base de données, shell, navigateur) **mélangées aux instructions**. L'attaquant fournit alors des "données" qui sont interprétées comme du **code**.

Une **injection SQL** survient lorsqu'une application intègre directement une entrée utilisateur dans une requête SQL, permettant de **modifier la logique de la requête**.

### Exemple : contournement d'une authentification

```python
# ❌ Code VULNÉRABLE Python : concaténation

requete = "SELECT * FROM comptes WHERE nom = '" + utilisateur + \
          "' AND mdp = '" + mot_de_passe + "'"
```

Si l'attaquant saisit `admin' --` comme nom d'utilisateur :

```sql
SELECT * FROM comptes WHERE nom = 'admin' --' AND mdp = '...'
```

En SQL, `--` débute un **commentaire** : la vérification du mot de passe disparaît, l'attaquant est connecté en tant qu'`admin`.

Avec l'opérateur `UNION`, un attaquant peut aussi **lire d'autres tables**, par exemple les noms d'utilisateurs et les empreintes de mots de passe.

{{%notice style="tip" title="Injection SQL et CIA"%}}
Selon les droits du compte de base de données, une injection SQL peut atteindre les trois objectifs **CIA** : lire (confidentialité), modifier (intégrité) ou supprimer (disponibilité) des données.
{{%/notice%}}

### Contre-mesure : les requêtes préparées
 
![Comparaison d'une requête construite par concaténation et d'une requête préparée avec la même saisie malveillante admin' --](/images/07-requete-preparee.png)
 
La **contre-mesure principale** est l'utilisation systématique de **requêtes préparées** (*prepared statements*) avec paramètres liés :
 
```python
# ✅ Python
curseur.execute("SELECT * FROM comptes WHERE nom = ?", (utilisateur,))
```
 
```php
// ✅ PHP (PDO)
$stmt = $pdo->prepare("SELECT * FROM comptes WHERE nom = :nom");
$stmt->execute(['nom' => $utilisateur]);
```

{{%notice style="info" title="Explication du code PHP"%}} 
- `$pdo` représente la connexion à la base de données. **PDO** est la bibliothèque standard de PHP pour accéder aux bases de données.
- `$pdo->prepare(...)` prépare la requête ; `:nom` est un **paramètre nommé** qui joue le même rôle que le `?` en Python. Le résultat est stocké dans la variable `$stmt`.
- `$stmt->execute(['nom' => $utilisateur])` exécute la requête en associant la valeur de `$utilisateur` au paramètre `:nom`. 
{{%/notice%}}

La structure de la requête est **fixée à l'avance** ; la valeur est transmise séparément et ne peut jamais être interprétée comme du SQL. `admin' --` devient un simple texte.
 
{{%notice style="tip" title="Défense en profondeur"%}}
- **Moindre privilège** (chapitre 5) : le compte de base de données de l'application n'a que les droits nécessaires (pas de `DROP TABLE` pour une page qui lit des produits).
- **Messages d'erreur génériques** : ne jamais afficher l'erreur SQL à l'utilisateur.
{{%/notice%}}


# 7.2. Cross-Site Scripting (XSS)
---------


Le **XSS** est une injection dont l'interpréteur est **le navigateur de la victime**. Il survient lorsqu'une application insère une donnée fournie par un utilisateur dans une page HTML **sans l'encoder**.

```php
// ❌ Code VULNÉRABLE PHP
echo "<p>Résultats pour : " . $_GET['q'] . "</p>";

```
{{%notice style="info" title="Explication du code PHP"%}}
`$_GET['q']` contient la valeur du paramètre `q` de l'URL, saisie par l'utilisateur. Le point (`.`) la concatène entre deux morceaux de HTML, et `echo` écrit le résultat dans la page envoyée au navigateur. La valeur est donc insérée **telle quelle** dans le HTML.
{{%/notice%}}

Avec l'URL `recherche.php?q=<script>alert('XSS')</script>`, le navigateur exécute le script, **dans l'origine du site légitime**. Le script peut donc lire la page, agir au nom de l'utilisateur et, si le cookie n'est pas `HttpOnly`, **lire le cookie de session** (`document.cookie`) pour l'envoyer à l'attaquant.

| Type | Où est le code malveillant ? | Victimes |
|---|---|---|
| **XSS réfléchi** | Dans la requête (paramètre d'URL) | La personne qui clique sur le lien piégé (hameçonnage) |
| **XSS stocké** | Dans la base de données (commentaire, profil, forum) | **Tous** ceux qui consultent la page (le plus dangereux) |

### Contre-mesures

1. **Encoder les sorties** : `<` devient `&lt;`, `>` devient `&gt;`, etc. Le texte est alors **affiché** au lieu d'être **interprété**.
   ```php
   echo "<p>Résultats pour : " . htmlspecialchars($_GET['q'], ENT_QUOTES, 'UTF-8') . "</p>";
   ```
   {{%notice style="info" title="Explication du code PHP"%}}
   La fonction PHP `htmlspecialchars(...)` remplace les caractères spéciaux du HTML par leur code. `ENT_QUOTES` demande d'encoder aussi les apostrophes et les guillemets, et `'UTF-8'` indique l'encodage des caractères. Le script de l'attaquant s'affiche alors comme du texte.
   {{%/notice%}}
   {{%notice style="tip" title=" "%}}
   Les moteurs de gabarits modernes (Jinja2, React...) encodent automatiquement.
   {{%/notice%}}
2. **Cookie `HttpOnly`** : limite les conséquences (le cookie ne peut plus être lu).


# 7.3. Attaques sur sessions et cookies
---------

Au chapitre 5, nous avons vu que voler un identifiant de session permet de se faire passer pour l'utilisateur. Voici les principales attaques et leurs contre-mesures.

## Principales attaques sur les sessions
### Vol de session (*session hijacking*)
 
Un attaquant peut obtenir un identifiant de session valide de plusieurs façons :
 
| Méthode | Description | Contre-mesures |
|---|---|---|
| **Écoute du réseau** | Si le site utilise HTTP sans chiffrement, le cookie circule **en clair** et peut être intercepté (Wi-Fi public, réseau compromis). | **HTTPS partout** (chapitre 4), attribut **`Secure`** |
| **XSS** | Un script injecté lit `document.cookie` (section précédente). | **`HttpOnly`** |
| **Logiciel malveillant** | Un *infostealer* sur le poste de la victime copie les cookies stockés par le navigateur (chapitre 6). | Protection du poste, sessions de courte durée, détection d'anomalies |
| **Fuite dans l'URL** | Un identifiant placé dans l'URL (`?sessionid=...`) se retrouve dans l'historique, et les journaux| Ne **jamais** placer d'identifiant de session dans l'URL |
 
### Prédiction de l'identifiant de session
 
Si les identifiants de session sont **prévisibles** (par exemple, un compteur incrémenté : `session=1001`, `session=1002`...), un attaquant peut simplement **deviner** l'identifiant d'une autre personne.
 
Un identifiant de session doit donc être :
 
- **long** ;
- **généré pseudo-aléatoirement** ;
- **sans signification** : il ne doit contenir ni le nom d'utilisateur, ni la date, ni aucune information devinable.
 
### Fixation de session
 
Dans une attaque par **fixation de session**, l'attaquant ne vole pas l'identifiant : il **impose** à la victime un identifiant qu'il connaît déjà.
 
**Exemple :** 
1. L'attaquant obtient un identifiant de session valide (non authentifié) : `ABC123`.
2. Il amène la victime à utiliser cet identifiant (lien piégé, poste partagé...).
3. La victime se connecte : le serveur associe son compte à la session `ABC123`.
4. L'attaquant utilise `ABC123` : il est connecté avec le compte de la victime.

 
**Contre-mesure** : **régénérer l'identifiant de session** après chaque authentification réussie (et après tout changement de privilèges). L'ancien identifiant devient alors inutile.
 
### Manipulation de cookies (*cookie tampering*)
 
Certaines applications mal conçues stockent dans un cookie des **informations sur lesquelles reposent des décisions de sécurité** :
 
```yaml
Set-Cookie: utilisateur=alice; role=etudiant
```
 
Puisque le cookie est stocké dans le navigateur, l'utilisateur peut le modifier (onglet *Application* des outils de développement) :
 
```yaml
Cookie: utilisateur=alice; role=enseignant
```
 
Si le serveur fait confiance à cette valeur, l'utilisateur obtient les privilèges d'un enseignant : c'est une **escalade de privilèges** (chapitre 5), et un défaut de **contrôle d'accès**.
 
**Contre-mesures** :
 
- conserver les informations sensibles (rôle, permissions) **côté serveur**, dans la session ; le cookie ne contient qu'un identifiant opaque ;
- si des données doivent être stockées côté client, les **signer** afin de détecter toute modification.
 
### Jetons JWT
 
Au chapitre 5, nous avons vu que les API utilisent souvent des **jetons d'accès**. Un format très répandu est le **JWT** (*JSON Web Token*), composé de trois parties séparées par des points :

![Structure d'un jeton JWT](/images/07-jwt.png)
 
L'en-tête et le contenu sont simplement **encodés en Base64URL**, et non chiffrés. Le contenu ci-dessus se décode ainsi :
 
```json
{"sub": "alice", "role": "etudiant"}
```
 
La **signature** (chapitre 4) permet au serveur de vérifier que le jeton n'a pas été modifié.

## Cross-Site Request Forgery (CSRF / XSRF)

Une attaque **CSRF** amène le navigateur d'une victime **connectée** à envoyer, à son insu, une requête vers le site visé. Comme le navigateur **joint automatiquement le cookie de session**, le site croit que la requête est légitime. L'attaquant ne vole rien : il **fait agir la victime à sa place**.

### Exemple

La victime est connectée à sa banque. L'attaquant l'attire sur sa propre page, qui contient :

```html
<form id="piege" action="https://banque.exemple/virement" method="POST">
  <input type="hidden" name="destinataire" value="compte-attaquant">
  <input type="hidden" name="montant" value="1000">
</form>
<script>document.getElementById("piege").submit();</script>
```

![Attaque CSRF : la victime connectée à sa banque visite une page piégée, qui envoie automatiquement un virement ; le navigateur joint le cookie de session de la banque](/images/07-csrf.png)

Le formulaire est envoyé automatiquement, le navigateur joint le cookie de la banque, et le virement est effectué. Si l'action est possible par une requête `GET`, une simple image suffit :

```html
<img src="https://banque.exemple/virement?destinataire=compte-attaquant&montant=1000">
```

### Vidéo explicative
{{<youtube xsnBRYZnBUc>}}

### Contre-mesures

1. **Jeton anti-CSRF** : une valeur aléatoire et secrète, liée à la session, est ajoutée à chaque formulaire et vérifiée par le serveur. L'attaquant ne peut pas la connaître (politique de même origine). Les cadriciels l'offrent généralement par défaut.
2. **Attribut `SameSite`** sur le cookie de session.
3. **Jamais d'action avec `GET`** : `GET` sert à consulter, `POST` à modifier.
4. **Réauthentification** pour les actions critiques (mot de passe actuel, MFA).

| | **XSS** | **CSRF** |
|---|---|---|
| D'où vient l'attaque ? | Du code s'exécute **dans le site visé** | La requête vient **d'un autre site** |
| Le site fait trop confiance... | aux **données** de l'utilisateur | au **navigateur** (le cookie suffit) |
| L'attaquant lit la réponse ? | Oui | Non |
| Contre-mesures | Encodage, `HttpOnly` | Jeton anti-CSRF, `SameSite` |

{{%notice style="tip" title="Exercice"%}}
Identifiez l'attaque : **XSS réfléchi**, **XSS stocké**, **CSRF**, **fixation de session** ou **manipulation de cookie**.

1. Un étudiant modifie le cookie `admin=false` en `admin=true` et accède au panneau d'administration.
2. Un courriel contient le lien `https://boutique.exemple/recherche?q=<script src=//pirate.exemple/x.js></script>`.
3. En visitant un blogue, l'adresse courriel d'un utilisateur est modifiée à son insu sur un réseau social où il est connecté.
4. Un attaquant envoie le lien `https://site.exemple/login?PHPSESSID=abc123`, puis utilise `abc123` après que la victime s'est connectée.
{{%/notice%}}

{{% expand "Solution" %}}
1. Manipulation de cookie
2. XSS réfléchi
3. CSRF
4. Fixation de session
{{% /expand %}}


{{%notice style="tip" title="Pour aller plus loin: d'autres attaques courantes"%}}
Voici d'autres attaques courantes liées aux sessions : 
- **Contrôle d'accès défaillant (IDOR)** : l'application vérifie que l'utilisateur est **connecté**, mais pas qu'il a **le droit** d'accéder à la ressource. Ex. : changer `bulletin?etudiant=2024123` en `2024124` affiche le bulletin d'un autre étudiant. 
  
  On retrouve la distinction **authentification / autorisation** du chapitre 5 ; la solution est de vérifier les permissions **côté serveur, à chaque requête** (RBAC). C'est le **1er risque** du Top 10 de l'OWASP.
- **Téléversement non contrôlé** : une « photo de profil » qui est en réalité un script exécuté par le serveur.
- **Détournement de clic** (*clickjacking*) : le site légitime est affiché dans un cadre invisible, sous un bouton piégé.

Ces attaques sont mentionnées pour les personnes désirant approfondir et aller plus loin. Il ne sera pas demandé de les connaître pour les examens.
{{%/notice%}}

# 7.4. Buffer Overflow
---------

{{%notice style="info" title="Contenu bonus"%}}
Le débordement de tampon est une attaque présentée en bonus : Elle ne sera pas étudiée en classe et il ne sera pas demandé de la connaître pour les examens.
{{%/notice%}}

Un **débordement de tampon** se produit lorsqu'un programme écrit des données **au-delà de l'espace mémoire qui leur est alloué**. Il concerne surtout les langages qui gèrent la mémoire manuellement (**C**, **C++**) : systèmes d'exploitation, navigateurs, serveurs, objets connectés.

```c
void saluer(char *nom) {
    char tampon[16];       // 16 octets prévus
    strcpy(tampon, nom);   // ❌ copie sans vérifier la taille
}
```

Si `nom` contient 100 caractères, les octets excédentaires écrasent la mémoire voisine, notamment l'**adresse de retour** de la fonction. Un attaquant peut alors faire planter le programme ou détourner son exécution vers son propre code.

###### Short explicatif
{{< youtube-short 4yjZV1Xt_OU >}}

**Contre-mesures** : fonctions qui limitent la taille (`snprintf`, `fgets` plutôt que `strcpy`, `gets`), langages à mémoire sécurisée (Rust, Java, C#, Python) et **application des correctifs** (chapitre 6).

# 7.5. Programmation sécurisé et références
---------
## OWASP Top 10
L'**OWASP** (*Open Worldwide Application Security Project*) est une fondation qui produit des ressources libres sur la sécurité des applications. Son **Top 10** est une référence de l'industrie qui recense les **catégories de risques les plus critiques** des applications Web. La version actuelle date de **2025** :

| Rang | Catégorie | Dans ce cours |
|---|---|---|
| A01 | Contrôle d'accès défaillant | Ch.7 - Attaques sur les sessions, Ch.5 |
| A02 | Mauvaise configuration de sécurité | Ch.7 - Principes de programmation sécurisée |
| A03 | Défaillances de la chaîne d'approvisionnement logicielle | Ch.7 - Principes de programmation sécurisée |
| A04 | Défaillances cryptographiques | Ch. 4 |
| A05 | Injection (SQL, commandes, XSS) | Ch.7 - Injection SQL et XSS |
| A06 | Conception non sécurisée | Ch. 3 |
| A07 | Défaillances d'authentification | Ch.7 - Attaques sur les sessions, Ch. 5 |
| A08 | Défaillances d'intégrité des logiciels ou des données | Ch. 4 (signatures) |
| A09 | Défaillances de journalisation et d'alerte | Ch. 6 |
| A10 | Mauvaise gestion des conditions exceptionnelles | Ch.7 - Principes de programmation sécurisée |

On remarque que le premier risque n'est pas une attaque sophistiquée, mais le plus souvent un simple **oubli de vérification des permissions**.

## Principes de programmation sécurisée

1. **Valider toutes les entrées côté serveur**, de préférence avec une liste de ce qui est **accepté** (type, longueur, format).
2. **Séparer le code des données** : requêtes préparées, encodage des sorties.
3. **Gérer les erreurs sans révéler d'information** : message générique pour l'utilisateur, détails dans les journaux. En cas d'erreur, **refuser** l'accès plutôt que l'accorder.
4. **Moindre privilège** (chapitre 5) pour le code, les services et les comptes de base de données.
5. **Secrets jamais en clair dans le code source** : variables d'environnement, fichier `.env` exclu du dépôt (`.gitignore`).
6. **Maintenir les dépendances à jour** (ex pour Node: `npm audit`) et n'installer que des bibliothèques connues.
7. **Journaliser** les échecs d'authentification et les refus d'accès (chapitre 6), sans jamais journaliser de mots de passe.


{{%notice style="tip" title="À retenir"%}}
| Attaque | Contre-mesure principale | Lien |
|---|---|---|
| Injection SQL | Requêtes préparées | Moindre privilège (ch. 5) |
| XSS | Encodage des sorties | `HttpOnly` |
| Vol de session | HTTPS, `Secure`, `HttpOnly` | TLS (ch. 4), sessions (ch. 5) |
| Fixation de session | Régénérer l'identifiant à la connexion | Sessions (ch. 5) |
| Manipulation de cookie | Données côté serveur ou signées | Signatures (ch. 4) |
| CSRF / XSRF | Jeton anti-CSRF, `SameSite` | Sessions (ch. 5) |
| Débordement de tampon | Vérifier les tailles, langages sécuritaires | Correctifs (ch. 6) |
{{%/notice%}}

## Références

+ [OWASP Top 10 : 2025](https://owasp.org/Top10/2025/)
+ [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
+ [MDN — Cookies HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Cookies)
+ [DVWA](https://github.com/digininja/DVWA)

# 7.6. Laboratoires
---------------


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

Les laboratoires utilisent **DVWA** (*Damn Vulnerable Web Application*), une application **volontairement vulnérable** conçue pour l'enseignement, installée sur une des **VMs des laboratoires du chapitre 4**. Vous y accédez depuis votre poste à l'adresse `http://<IP_VM>:4280`.

DVWA propose plusieurs niveaux de sécurité (`Low`, `Medium`, `Impossible`) et un bouton **View Source** pour comparer le code vulnérable et le code corrigé.

1. Connectez-vous avec `admin` / `password`.
2. Menu **Setup / Reset DB** → **Create / Reset Database**, puis reconnectez-vous.
3. Menu **DVWA Security** → niveau **Low**.

{{%notice style="warning" title="Important"%}}
DVWA ne doit jamais être exposée hors du réseau du laboratoire. Tester ces techniques sur tout autre site, y compris celui du cégep, est **interdit**. En cas de problème, utilisez **Create / Reset Database**.
{{%/notice%}}

{{%notice style="tip" title="Lire le code source de DVWA"%}}
DVWA est écrite en PHP. Relisez l'encadré [*Lire du PHP : l'essentiel* de l'annexe]({{% ref "07-securite-des-applications/a-php" %}}). Voici en plus les éléments que vous rencontrerez dans **View Source** :

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


# 7.6.1. Injection SQL
-------------

### 1. Analyser le code

Ouvrez **SQL Injection**, saisissez `1`, puis cliquez sur **View Source**.

1. Recopiez la ligne qui construit la requête SQL. Comment la valeur saisie y est-elle insérée ?

### 2. Modifier la logique de la requête

Saisissez `'` (une apostrophe), puis :

```text
1' OR '1'='1
```

2. Que provoque l'apostrophe seule ? Quelle information le message révèle-t-il à un attaquant ?
3. Combien d'utilisateurs sont affichés avec la deuxième saisie ? Écrivez la requête SQL complète exécutée et expliquez pourquoi la condition est toujours vraie.

### 3. Extraire les mots de passe

```text
1' UNION SELECT user, password FROM users#
```

(`#` est un commentaire en MySQL : il neutralise l'apostrophe finale.)

4. Quelles informations obtenez-vous ? Les mots de passe sont-ils stockés en clair ?
5. Sur VM2, exécutez `echo -n "password" | md5sum` et comparez avec l'empreinte de l'administrateur. Quel algorithme est utilisé ?
6. En vous référant au chapitre 4, expliquez pourquoi ce stockage est dangereux et quelle méthode aurait dû être utilisée.

### 4. Comparer avec le code corrigé

Passez au niveau **Impossible**, consultez le code et retentez l'étape 2.

7. Quelle technique est utilisée pour construire la requête ? Recopiez les lignes concernées.
8. Quelle vérification supplémentaire est faite sur la valeur saisie ?
9. Quelle mesure liée au **moindre privilège** limiterait les conséquences d'une injection oubliée ailleurs dans l'application ?

Remettez le niveau à **Low**.

# 7.6.2. XSS et vol de session
---------------
### 1. Observer les cookies

Ouvrez les outils de développement (**F12**) → **Application** (Chrome) ou **Stockage** (Firefox) → **Cookies**.

1. Quels cookies DVWA utilise-t-il ? Les attributs `Secure`, `HttpOnly` et `SameSite` sont-ils présents ?

### 2. XSS réfléchi

Ouvrez **XSS (Reflected)** et saisissez successivement :

```html
Alice
<b>Alice</b>
<script>alert(document.cookie)</script>
```

2. Que se passe-t-il dans chaque cas ? Observez l'URL : comment un attaquant pourrait-il atteindre une victime ?
3. Si le script envoyait le cookie à l'attaquant au lieu de l'afficher, quelle serait la conséquence ? Quel attribut l'aurait empêché ?

### 3. Vol de session

1. Dans votre navigateur (la "victime"), copiez la valeur du cookie `PHPSESSID`.
2. Ouvrez une **fenêtre de navigation privée** ou un autre navigateur (« l'attaquant ») et accédez à DVWA : la page de connexion s'affiche.
3. Dans les outils de développement de cette fenêtre, remplacez `PHPSESSID` par la valeur copiée (ajoutez au besoin le cookie `security` = `low`), puis ouvrez `http://<IP_VM>:4280/index.php`.

4. Êtes-vous connecté ? Avez-vous eu besoin du mot de passe ? Une MFA aurait-elle empêché cette attaque ?
5. Cliquez sur **Logout** dans la fenêtre "victime", puis rechargez la fenêtre "attaquant". Que se passe-t-il ? Pourquoi est-ce important ?

### 4. XSS stocké

Ouvrez **XSS (Stored)** et publiez le message `<script>alert('XSS stocké')</script>`. Quittez la page puis revenez-y.

Essayez ensuite d'écrire plus de 10 caractères dans le champ **Name**. Avec **Inspecter**, modifiez l'attribut `maxlength` du champ à `100` et réessayez.

6. Quelle différence y a-t-il entre XSS réfléchi et XSS stocké ? Lequel est le plus dangereux ?
7. Qu'a démontré la modification de `maxlength` ? Quelle règle de ce chapitre est illustrée ?

### 5. Code corrigé

Consultez le code de **XSS (Reflected)** au niveau **Impossible**.

8. Quelle fonction PHP est utilisée ? Au niveau **Impossible**, que devient votre script dans le code source de la page (**Ctrl+U**) ?

Réinitialisez la base de données (**Create / Reset Database**).

Remettez le niveau à **Low**.

# 7.6.3. CSRF
-------------
### 1. Analyser la page

Ouvrez **CSRF** et changez le mot de passe pour `test123`. Observez l'URL.

1. Quelle méthode HTTP et quels paramètres sont utilisés ? Le mot de passe actuel est-il demandé ? Le formulaire contient-il une valeur imprévisible ?

### 2. Créer la page piégée

Remettez le mot de passe à `password`. Sur la **VM**, créez `piege.html` (remplacez `<IP_VM>`) :

```html
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Concours</title></head>
<body>
  <h1>Félicitations ! Vous avez gagné une tablette !</h1>
  <a href="http://<IP_VM>:4280/vulnerabilities/csrf/?password_new=pirate&password_conf=pirate&Change=Change#">
    Cliquez ici pour réclamer votre prix</a>
</body>
</html>
```

Servez-la depuis la VM :

```bash
python3 -m http.server 8000
```

### 3. Réaliser l'attaque

En restant connecté à DVWA, ouvrez `http://<IP_VM>:8000/piege.html` dans un nouvel onglet et cliquez sur le lien. Déconnectez-vous de DVWA, puis essayez de vous reconnecter avec `password`, puis avec `pirate`.

2. Avec quel mot de passe avez-vous pu vous reconnecter ?
3. La page piégée connaissait-elle votre cookie de session ? Expliquez comment la requête a pu être authentifiée.
4. La page piégée a-t-elle pu **lire** la réponse de DVWA ? Pourquoi ?

### 4. Code corrigé

Consultez le code du niveau **Impossible**.

5. Quelles **deux** protections sont ajoutées ? Associez chacune à une contre-mesure de la section 7.6.
6. Pourquoi l'attaquant ne peut-il pas simplement inclure le jeton anti-CSRF dans sa page ?
7. Quelle autre erreur la page de niveau **Low** commet-elle en utilisant `GET` ?

Arrêtez le serveur Python (**Ctrl+C**) et réinitialisez la base de données.


# 7.6.4. Revue de code
------------------

Pour chaque extrait : identifiez la vulnérabilité, décrivez un scénario d'attaque, associez-la à une catégorie du Top 10 et proposez une correction.

### Extrait 1 (Python)

```python
titre = request.args.get("titre", "")
requete = f"SELECT titre, auteur FROM livres WHERE titre LIKE '%{titre}%'"
resultats = db.execute(requete).fetchall()
```

### Extrait 2 (Node.js)

```javascript
const db = mysql.createConnection({
  host: "db.interne", user: "root", password: "Admin2024!"
});
```

### Extrait 3 (PHP)

```php
if (!$compte) {
    echo "Erreur : l'utilisateur " . $_POST['nom'] . " n'existe pas.";
} elseif (md5($_POST['mdp']) !== $compte['mdp']) {
    echo "Erreur : mot de passe incorrect.";
}
```
{{%notice style="tip" title="Aide à la lecture"%}}
+ `$compte` contient la ligne trouvée dans la base de données pour le nom saisi, ou `false` si aucun compte n'existe (d'où le test `!$compte`). 
+ `$_POST['nom']` et `$_POST['mdp']` sont les champs du formulaire de connexion. 
+ `md5(...)` calcule l'empreinte MD5 du mot de passe saisi, qui est comparée à celle enregistrée. 
+ `!==` signifie "différent de".
{{%/notice%}}

{{% expand "Solution" %}}
**Extrait 1** — Injection SQL (A05). 
+ Correction : requête préparée : `db.execute("SELECT titre, auteur FROM livres WHERE titre LIKE ?", (f"%{titre}%",))`

**Extrait 2** — Secret en clair dans le code (A02 / A04) et compte `root` (non-respect du moindre privilège). 
+ Correction : variable d'environnement ou gestionnaire de secrets, `.env` dans `.gitignore`, changement du mot de passe exposé, compte de base de données dédié aux droits limités.

**Extrait 3** — XSS réfléchi (A05) : 
+ `$_POST['nom']` n'est pas encodé 
    + Correction : Encoder (`htmlspecialchars`). 
+ Messages différents qui révèlent si un compte existe (A07).
    + Correction : utiliser un message plus générique ("Identifiants invalides"). 
+ MD5 sans sel (A04)
    Correction : Utiliser une empreinte avec un sel.

{{% /expand %}}