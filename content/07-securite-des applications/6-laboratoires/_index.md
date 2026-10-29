

<!-- 

# 7.11 - Laboratoires

| Laboratoire | Durée suggérée |
|---|---|
| 7.11.1 Injection SQL | 1 h 15 |
| 7.11.2 XSS et vol de session | 1 h 30 |
| 7.11.3 CSRF | 1 h |
| 7.11.4 Revue de code | 45 min (ou devoir) |

## Mise en place

Les laboratoires utilisent **DVWA** (*Damn Vulnerable Web Application*), une application **volontairement vulnérable** conçue pour l'enseignement, installée sur **VM2**. Vous y accédez depuis **VM1** (ou votre poste) à l'adresse `http://<IP_VM2>:4280`.

DVWA propose plusieurs niveaux de sécurité (`Low`, `Medium`, `Impossible`) et un bouton **View Source** pour comparer le code vulnérable et le code corrigé.

1. Connectez-vous avec `admin` / `password`.
2. Menu **Setup / Reset DB** → **Create / Reset Database**, puis reconnectez-vous.
3. Menu **DVWA Security** → niveau **Low**.

{{%notice style="warning" title="Important"%}}
DVWA ne doit jamais être exposée hors du réseau du laboratoire. Tester ces techniques sur tout autre site, y compris celui du cégep, est **interdit**. En cas de problème, utilisez **Create / Reset Database**.
{{%/notice%}}

{{%notice style="info" title="Lire le code source de DVWA"%}}
DVWA est écrite en PHP. Relisez l'encadré *Lire du PHP : l'essentiel* de la section 7.3. Voici en plus les éléments que vous rencontrerez dans **View Source** :

| Code | Signification |
|---|---|
| `$id = $_REQUEST['id'];` | Récupère la valeur saisie dans le champ `id` |
| `"... WHERE user_id = '$id';"` | Entre guillemets doubles, `$id` est **remplacé par sa valeur** : c'est une concaténation |
| `mysqli_query($conn, $query)` | Exécute la requête SQL contenue dans `$query` |
| `is_numeric($id)` | Vérifie que la valeur est un nombre |
| `$db->prepare(...)` puis `bindParam(':id', $id, PDO::PARAM_INT)` | Requête préparée : associe `$id` au paramètre `:id`, **en tant qu'entier** |
| `$data->rowCount()` | Nombre de lignes retournées par la requête |
| `htmlspecialchars(...)` | Encode les caractères spéciaux HTML (section 7.4) |
| `checkToken(...)` / `generateSessionToken()` | Vérifie / crée un **jeton anti-CSRF** (section 7.6) |
{{%/notice%}}

{{%notice style="note" title="Pour l'enseignant — installation sur VM2"%}}
```bash
git clone https://github.com/digininja/DVWA.git
cd DVWA
# Dans compose.yml, remplacer "127.0.0.1:4280:80" par "4280:80"
# pour rendre DVWA accessible depuis VM1
docker compose up -d
```
{{%/notice%}}


# 7.11.1 Injection SQL

## Objectifs

- comprendre comment une entrée modifie la logique d'une requête ;
- constater les conséquences d'un mauvais stockage des mots de passe ;
- identifier la correction (requête préparée).

##### 1. Analyser le code

Ouvrez **SQL Injection**, saisissez `1`, puis cliquez sur **View Source**.

1. Recopiez la ligne qui construit la requête SQL. Comment la valeur saisie y est-elle insérée ?

##### 2. Modifier la logique de la requête

Saisissez `'` (une apostrophe), puis :

```text
1' OR '1'='1
```

2. Que provoque l'apostrophe seule ? Quelle information le message révèle-t-il à un attaquant ?
3. Combien d'utilisateurs sont affichés avec la deuxième saisie ? Écrivez la requête SQL complète exécutée et expliquez pourquoi la condition est toujours vraie.

##### 3. Extraire les mots de passe

```text
1' UNION SELECT user, password FROM users#
```

(`#` est un commentaire en MySQL : il neutralise l'apostrophe finale.)

4. Quelles informations obtenez-vous ? Les mots de passe sont-ils stockés en clair ?
5. Sur VM1, exécutez `echo -n "password" | md5sum` et comparez avec l'empreinte de l'administrateur. Quel algorithme est utilisé ?
6. En vous référant au chapitre 4, expliquez pourquoi ce stockage est dangereux et quelle méthode aurait dû être utilisée.

##### 4. Comparer avec le code corrigé

Passez au niveau **Impossible**, consultez le code et retentez l'étape 2.

7. Quelle technique est utilisée pour construire la requête ? Recopiez les lignes concernées.
8. Quelle vérification supplémentaire est faite sur la valeur saisie ?
9. Quelle mesure liée au **moindre privilège** limiterait les conséquences d'une injection oubliée ailleurs dans l'application ?

Remettez le niveau à **Low**.


# 7.11.2 XSS et vol de session

## Objectifs

- exploiter un XSS réfléchi et un XSS stocké ;
- réaliser un vol de session ;
- comprendre le rôle de l'encodage des sorties et des attributs de cookies.

##### 1. Observer les cookies

Ouvrez les outils de développement (**F12**) → **Application** (Chrome) ou **Stockage** (Firefox) → **Cookies**.

1. Quels cookies DVWA utilise-t-il ? Les attributs `Secure`, `HttpOnly` et `SameSite` sont-ils présents ?

##### 2. XSS réfléchi

Ouvrez **XSS (Reflected)** et saisissez successivement :

```html
Alice
<b>Alice</b>
<script>alert(document.cookie)</script>
```

2. Que se passe-t-il dans chaque cas ? Observez l'URL : comment un attaquant pourrait-il atteindre une victime ?
3. Si le script envoyait le cookie à l'attaquant au lieu de l'afficher, quelle serait la conséquence ? Quel attribut l'aurait empêché ?

##### 3. Vol de session

1. Dans votre navigateur (la « victime »), copiez la valeur du cookie `PHPSESSID`.
2. Ouvrez une **fenêtre de navigation privée** ou un autre navigateur (« l'attaquant ») et accédez à DVWA : la page de connexion s'affiche.
3. Dans les outils de développement de cette fenêtre, remplacez `PHPSESSID` par la valeur copiée (ajoutez au besoin le cookie `security` = `low`), puis ouvrez `http://<IP_VM2>:4280/index.php`.

4. Êtes-vous connecté ? Avez-vous eu besoin du mot de passe ? Une MFA aurait-elle empêché cette attaque ?
5. Cliquez sur **Logout** dans la fenêtre « victime », puis rechargez la fenêtre « attaquant ». Que se passe-t-il ? Pourquoi est-ce important ?

##### 4. XSS stocké

Ouvrez **XSS (Stored)** et publiez le message `<script>alert('XSS stocké')</script>`. Quittez la page puis revenez-y.

Essayez ensuite d'écrire plus de 10 caractères dans le champ **Name**. Avec **Inspecter**, modifiez l'attribut `maxlength` du champ à `100` et réessayez.

6. Quelle différence y a-t-il entre XSS réfléchi et XSS stocké ? Lequel est le plus dangereux ?
7. Qu'a démontré la modification de `maxlength` ? Quelle règle de ce chapitre est illustrée ?

##### 5. Code corrigé

Consultez le code de **XSS (Reflected)** au niveau **Impossible**.

8. Quelle fonction PHP est utilisée ? Au niveau **Impossible**, que devient votre script dans le code source de la page (**Ctrl+U**) ?

Réinitialisez la base de données (**Create / Reset Database**).


# 7.11.3 Falsification de requête intersite (CSRF)

## Objectifs

- comprendre pourquoi le cookie de session est envoyé automatiquement ;
- réaliser une attaque CSRF à l'aide d'une page piégée ;
- identifier les contre-mesures dans le code.

##### 1. Analyser la page

Ouvrez **CSRF** et changez le mot de passe pour `test123`. Observez l'URL.

1. Quelle méthode HTTP et quels paramètres sont utilisés ? Le mot de passe actuel est-il demandé ? Le formulaire contient-il une valeur imprévisible ?

##### 2. Créer la page piégée

Remettez le mot de passe à `password`. Sur **VM1**, créez `piege.html` (remplacez `<IP_VM2>`) :

```html
<!DOCTYPE html>
<html lang="fr">
<head><meta charset="UTF-8"><title>Concours</title></head>
<body>
  <h1>Félicitations ! Vous avez gagné une tablette !</h1>
  <a href="http://<IP_VM2>:4280/vulnerabilities/csrf/?password_new=pirate&password_conf=pirate&Change=Change">
    Cliquez ici pour réclamer votre prix</a>
</body>
</html>
```

Servez-la depuis VM1 :

```bash
python3 -m http.server 8000
```

##### 3. Réaliser l'attaque

En restant connecté à DVWA, ouvrez `http://<IP_VM1>:8000/piege.html` dans un nouvel onglet et cliquez sur le lien. Déconnectez-vous de DVWA, puis essayez de vous reconnecter avec `password`, puis avec `pirate`.

2. Avec quel mot de passe avez-vous pu vous reconnecter ?
3. La page piégée connaissait-elle votre cookie de session ? Expliquez comment la requête a pu être authentifiée.
4. La page piégée a-t-elle pu **lire** la réponse de DVWA ? Pourquoi ?

##### 4. Code corrigé

Consultez le code du niveau **Impossible**.

5. Quelles **deux** protections sont ajoutées ? Associez chacune à une contre-mesure de la section 7.6.
6. Pourquoi l'attaquant ne peut-il pas simplement inclure le jeton anti-CSRF dans sa page ?
7. Quelle autre erreur la page de niveau **Low** commet-elle en utilisant `GET` ?

Arrêtez le serveur Python (**Ctrl+C**) et réinitialisez la base de données.


# 7.11.4 Revue de code

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
session_start();
if (!isset($_SESSION['utilisateur'])) { die("Non connecté"); }

$stmt = $pdo->prepare("SELECT * FROM factures WHERE id = ?");
$stmt->execute([$_GET['facture']]);
afficher_facture($stmt->fetch());
```

*Aide à la lecture :* `session_start()` démarre ou reprend la session de l'utilisateur. `isset($_SESSION['utilisateur'])` vérifie qu'un utilisateur est enregistré dans la session, c'est-à-dire qu'il est connecté. `die("...")` affiche le message et arrête le script. `$_GET['facture']` est le paramètre `facture` de l'URL (ex. `factures.php?facture=1001`). `$stmt->fetch()` récupère la ligne trouvée.

### Extrait 4 (PHP)

```php
if (!$compte) {
    echo "Erreur : l'utilisateur " . $_POST['nom'] . " n'existe pas.";
} elseif (md5($_POST['mdp']) !== $compte['mdp']) {
    echo "Erreur : mot de passe incorrect.";
}
```

*Aide à la lecture :* `$compte` contient la ligne trouvée dans la base de données pour le nom saisi, ou `false` si aucun compte n'existe (d'où le test `!$compte`). `$_POST['nom']` et `$_POST['mdp']` sont les champs du formulaire de connexion. `md5(...)` calcule l'empreinte MD5 du mot de passe saisi, qui est comparée à celle enregistrée. `!==` signifie « différent de ».

### Extrait 5 (HTML + Python)

```html
<a href="/admin/supprimer?compte=42">Supprimer ce compte</a>
```

```python
@app.route("/admin/supprimer")
def supprimer():
    if session.get("role") == "admin":
        db.execute("DELETE FROM comptes WHERE id = ?", (request.args["compte"],))
    return redirect("/admin")
```

### Extrait 6 (Java)

```java
@WebServlet("/outils/ping")
public class PingServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        String hote = req.getParameter("hote");
        try {
            Process p = Runtime.getRuntime().exec(
                new String[] {"sh", "-c", "ping -c 4 " + hote});
            resp.getWriter().println(new String(p.getInputStream().readAllBytes()));
        } catch (Exception e) {
            e.printStackTrace(resp.getWriter());
        }
    }
}
```

{{% expand "Solution" %}}
**Extrait 1** — Injection SQL (A05). Correction : requête préparée :
`db.execute("SELECT titre, auteur FROM livres WHERE titre LIKE ?", (f"%{titre}%",))`

**Extrait 2** — Secret en clair dans le code (A02 / A04) et compte `root` (non-respect du moindre privilège). Correction : variable d'environnement ou gestionnaire de secrets, `.env` dans `.gitignore`, changement du mot de passe exposé, compte de base de données dédié aux droits limités.

**Extrait 3** — IDOR / contrôle d'accès défaillant (A01) : on vérifie que l'utilisateur est connecté, mais pas que la facture lui appartient. La requête préparée empêche l'injection, mais **pas** ce problème d'autorisation. Correction : `WHERE id = ? AND proprietaire_id = ?` avec l'identifiant de l'utilisateur en session.

**Extrait 4** — XSS réfléchi (A05) : `$_POST['nom']` n'est pas encodé (`htmlspecialchars`). Messages différents qui révèlent si un compte existe (A07) : utiliser « Identifiants invalides ». MD5 sans sel (A04) : utiliser les fonctions PHP `password_hash()` (calcule une empreinte bcrypt ou Argon2 avec un sel, lors de l'inscription) et `password_verify()` (vérifie le mot de passe saisi, lors de la connexion).

**Extrait 5** — CSRF (A01) : action destructrice par `GET`, sans jeton. Une image `<img src="https://site/admin/supprimer?compte=42">` visitée par un administrateur connecté suffit. Correction : `POST`, jeton anti-CSRF, cookie `SameSite`. *(Bon point : la requête est préparée et le rôle est vérifié côté serveur.)*

**Extrait 6** — Injection de commandes (A05) : la valeur de `hote` est concaténée dans une commande exécutée par le shell (`sh -c`). Ex. : `?hote=8.8.8.8;cat /etc/passwd` exécute une seconde commande sur le serveur. Correction : ne pas passer par le shell et transmettre les arguments séparément, après avoir validé que `hote` est bien une adresse IP ou un nom d'hôte :
```java
if (!hote.matches("^[a-zA-Z0-9.-]{1,253}$")) {
    resp.sendError(400, "Hôte invalide");
    return;
}
Process p = new ProcessBuilder("ping", "-c", "4", hote).start();
```
Deuxième problème : `e.printStackTrace(resp.getWriter())` affiche la trace complète de l'erreur à l'utilisateur (A10 / A02). Elle révèle des noms de classes, des chemins et des versions de bibliothèques. Correction : message générique pour l'utilisateur, détails dans les journaux. Enfin, le serveur d'applications ne devrait pas s'exécuter en `root` (moindre privilège).
{{% /expand %}} -->