+++
title = 'Débordement de tampon'
weight = '740'
draft = true
+++
---------

Un **débordement de tampon** se produit lorsqu'un programme écrit des données **au-delà de l'espace mémoire qui leur est alloué**. Il concerne surtout les langages qui gèrent la mémoire manuellement (**C**, **C++**) : systèmes d'exploitation, navigateurs, serveurs, objets connectés.

```c
void saluer(char *nom) {
    char tampon[16];       // 16 octets prévus
    strcpy(tampon, nom);   // ❌ copie sans vérifier la taille
}
```

Si `nom` contient 100 caractères, les octets excédentaires écrasent la mémoire voisine, notamment l'**adresse de retour** de la fonction. Un attaquant peut alors faire planter le programme (**disponibilité**) ou détourner son exécution vers son propre code.

{{< youtube-short 4yjZV1Xt_OU >}}

<!-- 📊 Illustration suggérée : la pile avant et après le débordement, avec l'adresse de retour écrasée. -->

{{%notice style="warning" title="Exemple réel : Heartbleed (2014)"%}}
Une erreur de vérification de longueur dans **OpenSSL** (l'outil utilisé au chapitre 4) permettait de lire la mémoire des serveurs HTTPS : mots de passe, **cookies de session** et même **clés privées**. Une cryptographie solide ne protège rien si son **implémentation** contient une erreur : c'est le principe du maillon le plus faible (chapitre 2).
{{%/notice%}}

**Contre-mesures** : fonctions qui limitent la taille (`snprintf`, `fgets` plutôt que `strcpy`, `gets`), langages à mémoire sécurisée (Rust, Java, C#, Python) et **application des correctifs** (chapitre 6).

<!-- protections du système (**ASLR**, pile non exécutable, canaris) -->