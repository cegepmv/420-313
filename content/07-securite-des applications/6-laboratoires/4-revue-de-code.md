+++
title = 'Revue de code'
weight = '764'
draft = false
+++
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

<!-- ### Extrait 3 (PHP)

```php
session_start();
if (!isset($_SESSION['utilisateur'])) { die("Non connecté"); }

$stmt = $pdo->prepare("SELECT * FROM factures WHERE id = ?");
$stmt->execute([$_GET['facture']]);
afficher_facture($stmt->fetch());
```

{{%notice style="tip" title="Aide à la lecture"%}}
+ * `session_start()` démarre ou reprend la session de l'utilisateur. 
+ `isset($_SESSION['utilisateur'])` vérifie qu'un utilisateur est enregistré dans la session, c'est-à-dire qu'il est connecté. 
+ `die("...")` affiche le message et arrête le script. 
+ `$_GET['facture']` est le paramètre `facture` de l'URL (ex. `factures.php?facture=1001`). 
+ `$stmt->fetch()` récupère la ligne trouvée.
{{%/notice%}} -->

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


<!-- ### Extrait 4 (HTML + Python)

```html
<a href="/admin/supprimer?compte=42">Supprimer ce compte</a>
```

```python
@app.route("/admin/supprimer")
def supprimer():
    if session.get("role") == "admin":
        db.execute("DELETE FROM comptes WHERE id = ?", (request.args["compte"],))
    return redirect("/admin")
``` -->

{{% expand "Solution" %}}
**Extrait 1** — Injection SQL (A05). 
+ Correction : requête préparée : `db.execute("SELECT titre, auteur FROM livres WHERE titre LIKE ?", (f"%{titre}%",))`

**Extrait 2** — Secret en clair dans le code (A02 / A04) et compte `root` (non-respect du moindre privilège). 
+ Correction : variable d'environnement ou gestionnaire de secrets, `.env` dans `.gitignore`, changement du mot de passe exposé, compte de base de données dédié aux droits limités.

<!-- **Extrait 3** — IDOR / contrôle d'accès défaillant (A01) : on vérifie que l'utilisateur est connecté, mais pas que la facture lui appartient. La requête préparée empêche l'injection, mais **pas** ce problème d'autorisation. Correction : `WHERE id = ? AND proprietaire_id = ?` avec l'identifiant de l'utilisateur en session. -->

**Extrait 3** — XSS réfléchi (A05) : 
+ `$_POST['nom']` n'est pas encodé 
    + Correction : Encoder (`htmlspecialchars`). 
+ Messages différents qui révèlent si un compte existe (A07).
    + Correction : utiliser un message plus générique ("Identifiants invalides"). 
+ MD5 sans sel (A04)
    Correction : Utiliser une empreinte avec un sel.

<!-- **Extrait 5** — CSRF (A01) : action destructrice par `GET`, sans jeton. Une image `<img src="https://site/admin/supprimer?compte=42">` visitée par un administrateur connecté suffit. Correction : `POST`, jeton anti-CSRF, cookie `SameSite`. *(Bon point : la requête est préparée et le rôle est vérifié côté serveur.)* -->
{{% /expand %}}