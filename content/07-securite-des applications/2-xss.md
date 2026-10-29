+++
title = 'Cross-Site Scripting (XSS)'
weight = '720'
draft = true
+++
---------

Le **XSS** est une injection dont l'interpréteur est **le navigateur de la victime**. Il survient lorsqu'une application insère une donnée fournie par un utilisateur dans une page HTML **sans l'encoder**.

```php
// Code VULNÉRABLE
echo "<p>Résultats pour : " . $_GET['q'] . "</p>";
```

**Lecture du code** : `$_GET['q']` contient la valeur du paramètre `q` de l'URL, saisie par l'utilisateur. Le point (`.`) la concatène entre deux morceaux de HTML, et `echo` écrit le résultat dans la page envoyée au navigateur. La valeur est donc insérée **telle quelle** dans le HTML.

Avec l'URL `recherche.php?q=<script>alert('XSS')</script>`, le navigateur exécute le script, **dans l'origine du site légitime**. Le script peut donc lire la page, agir au nom de l'utilisateur et, si le cookie n'est pas `HttpOnly`, **lire le cookie de session** (`document.cookie`) pour l'envoyer à l'attaquant.

| Type | Où est le code malveillant ? | Victimes |
|---|---|---|
| **XSS réfléchi** | Dans la requête (paramètre d'URL) | La personne qui clique sur le lien piégé (hameçonnage) |
| **XSS stocké** | Dans la base de données (commentaire, profil, forum) | **Tous** ceux qui consultent la page — le plus dangereux |

### Contre-mesures

1. **Encoder les sorties** : `<` devient `&lt;`, `>` devient `&gt;`, etc. Le texte est alors **affiché** au lieu d'être **interprété**.
   ```php
   echo "<p>Résultats pour : " . htmlspecialchars($_GET['q'], ENT_QUOTES, 'UTF-8') . "</p>";
   ```
   La fonction PHP `htmlspecialchars(...)` remplace les caractères spéciaux du HTML par leur code. `ENT_QUOTES` demande d'encoder aussi les apostrophes et les guillemets, et `'UTF-8'` indique l'encodage des caractères. Le script de l'attaquant s'affiche alors comme du texte.

   Les moteurs de gabarits modernes (Jinja2, React...) encodent automatiquement.
2. **Cookie `HttpOnly`** : limite les conséquences (le cookie ne peut plus être lu).
<!-- 3. **Content Security Policy (CSP)** : en-tête qui indique au navigateur quels scripts sont autorisés. -->

<!-- {{%notice style="tip" title="À retenir"%}}
**Valider à l'entrée, encoder à la sortie.**
{{%/notice%}} -->