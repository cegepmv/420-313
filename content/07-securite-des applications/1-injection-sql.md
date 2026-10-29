+++
title = 'Injection SQL'
weight = '710'
draft = false
+++
---------

Une **injection** survient lorsque des données fournies par l'utilisateur sont transmises à un **interpréteur** (base de données, shell, navigateur) **mélangées aux instructions**. L'attaquant fournit alors des « données » qui sont interprétées comme du **code**.

Une **injection SQL** survient lorsqu'une application intègre directement une entrée utilisateur dans une requête SQL, permettant de **modifier la logique de la requête**.

### Exemple : contournement d'une authentification

```python
# Code VULNÉRABLE : concaténation

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
# Python
curseur.execute("SELECT * FROM comptes WHERE nom = ?", (utilisateur,))
```
 
```php
// PHP (PDO)
$stmt = $pdo->prepare("SELECT * FROM comptes WHERE nom = :nom");
$stmt->execute(['nom' => $utilisateur]);
```
 
La structure de la requête est **fixée à l'avance** ; la valeur est transmise séparément et ne peut jamais être interprétée comme du SQL. `admin' --` devient un simple texte.
 
{{%notice style="tip" title="Défense en profondeur"%}}
- **Moindre privilège** (chapitre 5) : le compte de base de données de l'application n'a que les droits nécessaires (pas de `DROP TABLE` pour une page qui lit des produits).
- **Messages d'erreur génériques** : ne jamais afficher l'erreur SQL à l'utilisateur.
{{%/notice%}}
<!-- {{%notice style="info" title="Même principe : l'injection de commandes"%}}
Une page qui exécute `os.system("ping " + hote)` permet à un attaquant de saisir `8.8.8.8; cat /etc/passwd` pour exécuter une seconde commande. Solution : éviter le shell et passer les arguments séparément (`subprocess.run(["ping", hote])`), valider l'entrée, et ne jamais exécuter le service Web en `root`.
{{%/notice%}} -->