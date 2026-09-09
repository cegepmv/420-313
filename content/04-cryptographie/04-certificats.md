<!-- 
## 4.6 - Certificats et signature numérique

### Fonctions de hachage

Une **fonction de hachage** transforme un message de taille quelconque en une empreinte (*hash*) de taille fixe, avec les propriétés suivantes :

- **Irréversibilité** : impossible de retrouver le message d'origine à partir de l'empreinte.
- **Effet avalanche (pseudo-aléatoires)** : un changement minime du message produit une empreinte complètement différente.
- **Résistance aux collisions** : il doit être extrêmement difficile de trouver deux messages différents produisant la même empreinte.

Applications : vérification de l'intégrité d'un fichier téléchargé, stockage sécurisé de mots de passe (jamais en clair), base des signatures numériques. Algorithme couramment utilisé aujourd'hui : **SHA-256** (la famille *MD5*/*SHA-1*, plus ancienne, est aujourd'hui considérée affaiblie pour un usage sécuritaire).

### Signature numérique

Une **signature numérique** combine le hachage et le chiffrement asymétrique : on calcule d'abord l'empreinte du message, puis on chiffre cette empreinte avec la clé **privée** de l'expéditeur. Le destinataire recalcule l'empreinte du message reçu et la compare à celle obtenue en déchiffrant la signature avec la clé **publique** de l'expéditeur. Une correspondance garantit à la fois l'**intégrité** du message et son **authenticité** (non-répudiation).

> 📊 *Illustration suggérée : schéma en deux colonnes (expéditeur / destinataire) montrant le processus complet de signature puis de vérification.*

### Certificats numériques et infrastructure à clé publique

Un **certificat numérique** associe une clé publique à l'identité de son détenteur (une personne, une organisation, un serveur web) et est signé par une **autorité de certification (CA)** de confiance. C'est ce mécanisme qui permet, par exemple, à un navigateur web de faire confiance à la clé publique présentée par un site HTTPS — on approfondira le fonctionnement complet de l'**infrastructure à clé publique (PKI)** au chapitre 5. -->

