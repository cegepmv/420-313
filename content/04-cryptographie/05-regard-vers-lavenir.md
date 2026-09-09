<!-- 
## 4.8 - Regard vers l'avenir : l'informatique quantique et la cryptographie post-quantique

Toute la cryptographie à clé publique vue dans ce chapitre (RSA, Diffie-Hellman) repose sur des problèmes mathématiques réputés difficiles à résoudre pour un **ordinateur classique** — principalement la *factorisation de grands nombres* et le *calcul de logarithme discret*.

### La menace de l'algorithme de Shor

En 1994, le mathématicien **Peter Shor** a démontré qu'un **ordinateur quantique** suffisamment puissant pourrait résoudre ces deux problèmes de façon efficace, rendant potentiellement obsolètes RSA et Diffie-Hellman tels qu'utilisés aujourd'hui.

**Nuance importante** : cette menace concerne spécifiquement le chiffrement **asymétrique**. Le chiffrement **symétrique** (AES) et les fonctions de **hachage** demeurent globalement robustes face aux ordinateurs quantiques, moyennant au besoin une augmentation de la taille des clés ou des empreintes (l'algorithme de *Grover* offre un gain quadratique, non exponentiel, contre ces primitives).

### Où en est-on réellement ?

Le développement d'un ordinateur quantique capable de casser RSA en pratique nécessite un nombre de **qubits logiques** (après correction d'erreurs) largement supérieur à ce qui est démontré publiquement à ce jour. Il est important de distinguer la *recherche théorique*, bien établie, de la *menace opérationnelle* à court terme, qui reste incertaine et dépend de percées technologiques encore à venir.

### « Harvest now, decrypt later »

Une stratégie déjà documentée consiste, pour un attaquant, à **intercepter et conserver** aujourd'hui des communications chiffrées, dans l'attente qu'un futur ordinateur quantique permette de les déchiffrer rétroactivement. Cette stratégie représente un risque concret pour toute donnée dont la confidentialité doit être garantie sur le long terme (dossiers médicaux, secrets industriels, documents gouvernementaux), même si la menace quantique elle-même n'est pas encore opérationnelle.

> 📊 *Illustration suggérée : ligne du temps « aujourd'hui → interception → futur déchiffrement quantique » pour visualiser le concept.*

### Les algorithmes « quantum-resistant » (post-quantiques)

Une nouvelle génération d'algorithmes à clé publique est en développement, conçue pour résister à la fois aux ordinateurs classiques et quantiques. Plutôt que de reposer sur la factorisation ou le logarithme discret, ces algorithmes s'appuient sur d'autres problèmes mathématiques réputés difficiles même pour un ordinateur quantique, notamment :

- les **réseaux euclidiens** (*lattices*) ;
- les **codes correcteurs d'erreurs** ;
- certaines constructions basées sur le **hachage**.

Le **NIST** (National Institute of Standards and Technology, États-Unis) mène depuis plusieurs années un processus de standardisation de ces algorithmes, ayant mené à la sélection de premiers standards (par exemple *CRYSTALS-Kyber* pour l'échange de clés, *CRYSTALS-Dilithium* pour la signature numérique). *Le domaine évolue rapidement : ces repères doivent être validés et mis à jour au moment d'enseigner cette section.*

### Approches de transition

Dans l'attente d'une adoption complète des algorithmes post-quantiques, plusieurs organisations déploient déjà des solutions **hybrides**, combinant un algorithme classique (RSA ou ECC) et un algorithme post-quantique pour un même échange de clé, afin de rester protégées même si l'un des deux algorithmes venait à être cassé.

{{% notice tip "Discussion" %}}
Qu'est-ce que la menace quantique implique concrètement pour une organisation aujourd'hui ? (inventaire cryptographique, agilité cryptographique, priorisation des données à longue durée de vie) — un pont naturel vers la gouvernance de la sécurité (chapitre 10).
{{% /notice %}} -->