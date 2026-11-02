+++
title = 'XSS et vol de session'
weight = '762'
draft = false
+++
---------------
## Objectifs



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
3. Dans les outils de développement de cette fenêtre, remplacez `PHPSESSID` par la valeur copiée (ajoutez au besoin le cookie `security` = `low`), puis ouvrez `http://<IP_VM2>:4280/index.php`.

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

