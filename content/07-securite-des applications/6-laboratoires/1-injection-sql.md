+++
title = 'Injection SQL'
weight = '761'
draft = false
+++
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