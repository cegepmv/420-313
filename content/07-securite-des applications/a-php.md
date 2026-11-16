+++
pre = '<b>Annexe - </b>'
title = 'PHP'
weight = '770'
draft = true
+++
-----------------
Plusieurs exemples de ce chapitre, ainsi que l'application DVWA utilisée en laboratoire, sont écrits en **PHP**, un langage très répandu pour les sites Web. Il n'est pas nécessaire de savoir écrire du PHP : il suffit de pouvoir le **lire**.
 
### Lire du PHP : l'essentiel

| PHP | Signification | Équivalent Java |
|---|---|---|
| `<?php ... ?>` | Délimite le code PHP dans une page | — |
| `$nom` | Une variable (toutes commencent par `$`, sans type déclaré) | `String nom` |
| `"Bonjour " . $nom` | Le point **concatène** des chaînes | `"Bonjour " + nom` |
| `"Bonjour $nom"` | Entre **guillemets doubles**, PHP remplace `$nom` par sa valeur | `"Bonjour " + nom` |
| `$pdo->prepare(...)` | La flèche appelle une **méthode** d'un objet | `cnx.prepareStatement(...)` |
| `['nom' => $x]` | Tableau associatif (clé → valeur) | `Map.of("nom", x)` |
| `$_GET['q']` | Paramètre `q` de l'URL | `@QueryParam("q")` |
| `$_POST['nom']` | Champ `nom` d'un formulaire envoyé en `POST` | `@FormParam("nom")` |
| `$_REQUEST['id']` | Paramètre `id`, qu'il vienne de l'URL ou du formulaire | — |
| `$_SESSION['x']` | Donnée conservée dans la session, côté serveur | `request.getSession().getAttribute("x")` |
| `echo "..."` | Écrit le texte dans la page HTML envoyée au navigateur | Valeur retournée par une ressource `@Produces(MediaType.TEXT_HTML)` |
 


{{%notice style="note" title="À surveiller"%}}
`$_GET`, `$_POST`, `$_REQUEST` et `$_COOKIE` contiennent des données **fournies par l'utilisateur**.
{{%/notice%}}