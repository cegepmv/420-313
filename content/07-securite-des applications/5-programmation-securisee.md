+++
title = 'Programmation sécurisé et références'
weight = '750'
draft = true
+++
---------


## OWASP Top 10
L'**OWASP** (*Open Worldwide Application Security Project*) est une fondation qui produit des ressources libres sur la sécurité des applications. Son **Top 10** est une référence de l'industrie qui recense les **catégories de risques les plus critiques** des applications Web. La version actuelle date de **2025** :

| Rang | Catégorie | Dans ce cours |
|---|---|---|
| A01 | Contrôle d'accès défaillant | 7.5, 7.7, ch. 5 |
| A02 | Mauvaise configuration de sécurité | 7.10 |
| A03 | Défaillances de la chaîne d'approvisionnement logicielle | 7.10 |
| A04 | Défaillances cryptographiques | ch. 4 |
| A05 | Injection (SQL, commandes, XSS) | 7.3, 7.4 |
| A06 | Conception non sécurisée | ch. 3 |
| A07 | Défaillances d'authentification | 7.5, ch. 5 |
| A08 | Défaillances d'intégrité des logiciels ou des données | ch. 4 (signatures) |
| A09 | Défaillances de journalisation et d'alerte | ch. 6 |
| A10 | Mauvaise gestion des conditions exceptionnelles | 7.10 |

On remarque que le premier risque n'est pas une attaque sophistiquée, mais le plus souvent un simple **oubli de vérification des permissions**.

## Principes de programmation sécurisée

1. **Valider toutes les entrées côté serveur**, de préférence avec une liste de ce qui est **accepté** (type, longueur, format).
2. **Séparer le code des données** : requêtes préparées, encodage des sorties.
3. **Gérer les erreurs sans révéler d'information** : message générique pour l'utilisateur, détails dans les journaux. En cas d'erreur, **refuser** l'accès plutôt que l'accorder.
4. **Moindre privilège** (chapitre 5) pour le code, les services et les comptes de base de données.
5. **Secrets jamais en clair dans le code source** : variables d'environnement, fichier `.env` exclu du dépôt (`.gitignore`), gestionnaire de secrets. Un secret publié par erreur sur GitHub doit être **révoqué**, pas seulement supprimé (il reste dans l'historique).
6. **Maintenir les dépendances à jour** (`npm audit`, `pip-audit`, Dependabot) et n'installer que des bibliothèques connues.
7. **Configurer le serveur de façon sécuritaire** : pas de comptes par défaut ni de mode débogage en production, en-têtes de sécurité (`Strict-Transport-Security`, `Content-Security-Policy`, `X-Frame-Options`).
8. **Journaliser** les échecs d'authentification et les refus d'accès (chapitre 6), sans jamais journaliser de mots de passe.
9. **Se méfier des macros et scripts intégrés** (suites bureautiques) : désactivés par défaut, autorisés seulement s'ils sont signés.


{{%notice style="tip" title="À retenir"%}}

| Attaque | Contre-mesure principale | Lien |
|---|---|---|
| Injection SQL | Requêtes préparées | Moindre privilège (ch. 5) |
| XSS | Encodage des sorties, CSP | `HttpOnly` |
| Vol de session | HTTPS, `Secure`, `HttpOnly` | TLS (ch. 4), sessions (ch. 5) |
| Fixation de session | Régénérer l'identifiant à la connexion | Sessions (ch. 5) |
| Manipulation de cookie | Données côté serveur ou signées | Signatures (ch. 4) |
| CSRF / XSRF | Jeton anti-CSRF, `SameSite` | Sessions (ch. 5) |
| IDOR | Vérifier l'autorisation côté serveur | RBAC (ch. 5) |
| Débordement de tampon | Vérifier les tailles, langages sécuritaires | Correctifs (ch. 6) |

{{%/notice%}}

## Références

+ [OWASP Top 10 : 2025](https://owasp.org/Top10/2025/)
+ [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
+ [MDN — Cookies HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Cookies)
+ [DVWA](https://github.com/digininja/DVWA)