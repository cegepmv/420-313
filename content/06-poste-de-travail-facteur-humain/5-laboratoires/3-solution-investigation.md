+++
title = "Solution de l'investigation"
weight = '653'
draft = false
+++
--------------

Ce solutionnaire présente une réponse très détaillée. D'autres sont acceptables si elles s'appuient sur des **indices vérifiables** du dossier et distinguent bien les **faits** des **hypothèses**.

##### 1. Examiner les indices

**Le courriel (`01_Courriel_suspect.txt`)**

1. **Adresse de l'expéditeur :** 
    + Le courriel provient de `securite@cegepvl-soutien.com`, alors que le domaine officiel est `cegepvl.qc.ca`. 
    + Le domaine est **différent** : il reprend le nom du cégep pour paraître légitime, mais il appartient à n'importe qui (`.com` au lieu de `.qc.ca`). 
    + Le nom affiché « Service informatique » ne prouve rien : n'importe qui peut choisir ce nom.

2. **Lien :** Non. Le texte affiché est `https://portail.cegepvl.qc.ca/securite`, mais le lien réel mène à `https://cegepvl-soutien.com/verification`, soit le **même domaine frauduleux** que l'expéditeur. Il s'agit très probablement d'une fausse page de connexion conçue pour voler le mot de passe.

3. **Pièce jointe :** Le fichier `Avis_mise_a_jour.pdf.exe` est une **application (exécutable)** et non un document PDF. Il utilise une **double extension**. Comme Windows **masque les extensions connues** sur ce poste, l'utilisatrice voyait seulement `Avis_mise_a_jour.pdf`, accompagné d'une icône de PDF (un exécutable peut afficher l'icône de son choix). Le fichier est aussi placé dans une archive `.zip`, ce qui aide souvent à contourner les filtres de courriel.

4. **Leviers psychologiques.**

   | Levier | Passage du message |
   |---|---|
   | Urgence | *"URGENT – Mise à jour de sécurité obligatoire avant 10 h"* |
   | Peur | *"votre compte sera DÉSACTIVÉ et vous perdrez l'accès à vos courriels et à vos fichiers"* |
   | Autorité | Signature *"Le Service informatique"* », mention d'une *"faille de sécurité critique"* |
   | Empêcher la vérification | *"Inutile d'appeler le centre d'assistance, il est débordé ce matin"* |

   Le dernier passage est particulièrement révélateur : il décourage la victime de **vérifier la demande par un canal indépendant**, ce qui est justement le bon réflexe.
5. **Technique.** Il s'agit d'**hameçonnage (*phishing*)** avec **usurpation d'identité** du service informatique. Le message combine deux vecteurs : une **pièce jointe malveillante** et un **lien de vol d'identifiants**. On peut aussi parler de **harponnage (*spear phishing*)** visant l'organisation, puisque le message imite précisément le cégep (nom de domaine ressemblant, service interne), même s'il n'est pas personnalisé pour Julie.

**Le journal Windows (`02_Extrait_journal_securite.csv`)**

1. **Échecs de 8 h 44 :** Non, ils ne sont pas inquiétants. Il y a seulement **deux** échecs, à 11 secondes d'intervalle, suivis d'une **ouverture de session réussie au clavier du poste** 16 secondes plus tard, à l'heure d'arrivée au travail. C'est typique d'une faute de frappe dans le mot de passe. Une attaque par force brute produirait de nombreux échecs rapprochés, souvent par le réseau. De plus, ces événements ont lieu **avant** la réception du courriel.
2. **Compte `svc_inventaire`:** Oui, ces connexions sont normales. Ce compte de service correspond à l'agent d'inventaire installé par le service informatique. Les connexions proviennent du **serveur d'inventaire `10.20.0.15`**, comme prévu, et reviennent à intervalle régulier (9 h 02 et 9 h 31), **avant et après** l'incident.
3. **Événements anormaux :**
   - 9 h 23 min 05 s (ID **4720**) : création d'un compte local nommé `support_it` ;
   - 9 h 23 min 07 s (ID **4732**) : ajout de `support_it` au groupe **Administrateurs**.

   Un compte administrateur a été créé, puis élevé, en **2 secondes**, moins de 2 minutes après l'exécution de la pièce jointe. Le nom `support_it` est choisi pour passer inaperçu : même si le logiciel malveillant est supprimé ou si Julie change son mot de passe, l'attaquant conserve un accès administrateur au poste.
4. **Le compte `jtremblay` est indiqué :** Non, cela ne prouve pas que Julie a fait ces actions elle-même. Le journal indique le **compte** sous lequel l'action s'est exécutée, pas la personne devant l'écran. Un programme lancé par Julie s'exécute avec ses droits ; comme elle est **administratrice** et qu'elle a accepté la demande d'autorisation, le logiciel malveillant disposait des droits nécessaires pour créer un compte. 
<!-- Le délai de 2 secondes entre les deux actions suggère une **action automatisée**, et Julie ne mentionne rien de tel dans son témoignage. -->

**Les processus (`03_Liste_processus.txt`)**

1. **Processus suspect :** `svchost.exe`, **PID 6612**.
2. **Comparaison avec les autres `svchost.exe` :**

   | Critère | Vrais `svchost.exe` (PID 892, 1012, 1488) | `svchost.exe` (PID 6612) |
   |---|---|---|
   | Emplacement | `C:\Windows\System32\` | `C:\Users\jtremblay\AppData\Roaming\Microsoft\` |
   | Utilisateur | Comptes système (SYSTEM, SERVICE RÉSEAU, SERVICE LOCAL) | `jtremblay` (compte d'une personne) |
   | Signature | Microsoft Windows | **Non signé** |
   | Processeur | 0,1 % à 0,3 % | **62,3 %** |

   S'ajoute un cinquième indice : il a démarré à **9 h 21 min 39 s**, juste après l'ouverture de la pièce jointe. Le programme **usurpe le nom** d'un processus système légitime pour se fondre dans la liste. Son utilisation élevée du processeur explique probablement la lenteur signalée.
3. **`Spotify.exe`.** Non, il n'est pas suspect. Spotify s'installe **normalement** dans `AppData\Roaming\Spotify` (installation par utilisateur). Son nom correspond à son emplacement, il est **signé par Spotify AB** et il utilise peu le processeur (0,4 %). L'emplacement `AppData` n'est donc pas, à lui seul, un critère suffisant. 
  {{%notice style="note" title="À se demander"%}}
  On pourrait se demander si ce logiciel est **autorisé** par la politique du cégep sur un poste des finances, mais c'est une question de conformité, pas un indice de compromission.
  {{%/notice%}}

**Les connexions réseau (`04_Connexions_reseau.csv`)**

1. **Analyse de chaque connexion.**

   | Programme | Destination | Normale? | Justification |
   |---|---|---|---|
   | `AgentInventaire.exe` | `10.20.0.15:9525` | Oui | Serveur d'inventaire interne prévu dans l'environnement |
   | `OUTLOOK.EXE` | `198.51.100.20:443` | Oui | Messagerie en HTTPS |
   | `msedge.exe` | `198.51.100.35:443` | Probablement | Navigation HTTPS ordinaire, mais l'adresse seule ne dit pas **quel site** est consulté (à vérifier, voir partie 2, question 3) |
   | `Spotify.exe` | `198.51.100.120:4070` | Oui | Programme légitime et signé ; le port 4070 est utilisé par Spotify |
   | `MsMpEng.exe` | `198.51.100.140:443` | Oui | Antivirus Defender |
   | `svchost.exe (6612)` | `203.0.113.45:8443` | **Non** | Faux processus, adresse publique inconnue, port inhabituel |
   | `System (4)` | `10.20.0.10:445` | Oui | Partage de fichiers (SMB) vers le serveur de fichiers interne |

2. **Connexion à vérifier :** La connexion du **faux `svchost.exe` (PID 6612)** vers `203.0.113.45:8443`. C'est **le même processus** que celui repéré à la partie 1.3 : un programme non signé, lancé depuis `AppData`, qui communique avec un serveur externe inconnu. Il s'agit probablement d'un **serveur de commande et de contrôle (C&C)** permettant à l'attaquant de piloter le poste, ou d'un canal d'**exfiltration de données**.
  {{%notice title="À noter" style="info"%}}
  La connexion vers le serveur de fichiers est normale en soi, mais elle montre que le poste (et donc le logiciel malveillant, qui a les droits de Julie) **a accès aux fichiers partagés** du cégep.
  {{%/notice%}}

**Le rapport antivirus (`05_Rapport_antivirus.txt`)**

1. **Détection.** À **9 h 21 min 41 s**, Defender a détecté `Trojan:Win32/Dropper` (comportement suspect) dans `C:\Users\jtremblay\AppData\Local\Temp\Avis_mise_a_jour.pdf.exe`, soit la pièce jointe extraite de l'archive. Il l'a **mise en quarantaine** avec succès.
2. **Analyse de 9 h 50.** Non, le poste n'est pas sain. Le faux `svchost.exe` a démarré à **9 h 21 min 39 s**, soit **2 secondes avant** la mise en quarantaine : Le logiciel malveillant était déjà exécuté et actif. La preuve en est qu'à 9 h 57 et 9 h 58, le faux `svchost.exe` est toujours en cours d'exécution et connecté à `203.0.113.45`, et que le compte `support_it` a été créé à 9 h 23. L'absence d'alerte ne prouve pas l'absence de menace.
3. **Configuration.** Les **signatures n'ont pas été mises à jour depuis 4 jours** (16 octobre). Une charge récente ou inconnue peut échapper à la détection par signature. On peut aussi mentionner qu'une **analyse rapide** ne vérifie que certains emplacements et ne remplace pas une analyse complète.

---

##### 2. Reconstituer la chronologie

| Heure | Ce qui s'est passé | Source (fichier) | Fait ou hypothèse? |
|---|---|---|---|
| 8 h 44 – 8 h 45 | Deux échecs d'ouverture de session, puis ouverture réussie au clavier (faute de frappe probable) | 02 | Fait (l'explication « faute de frappe » est une hypothèse) |
| 9 h 02 | Connexion réseau du compte `svc_inventaire` depuis le serveur d'inventaire (normal) | 02 | Fait |
| 9 h 12 | Réception du courriel frauduleux avec `Avis_mise_a_jour.zip` | 01 | Fait |
| ≈ 9 h 20 | Julie ouvre le « PDF » ; rien ne s'affiche ; elle accepte la demande d'autorisation Windows (UAC) | 01 (témoignage) | Fait déclaré (heure approximative) |
| 9 h 21 min 39 s | Démarrage du faux `svchost.exe` (PID 6612) dans `AppData\Roaming\Microsoft` | 03 | Fait |
| 9 h 21 min 41 s | Defender détecte `Avis_mise_a_jour.pdf.exe` et le met en quarantaine | 05 | Fait |
| — | `Avis_mise_a_jour.pdf.exe` a installé et lancé le faux `svchost.exe` avant d'être bloqué | 03 + 05 | Hypothèse |
| 9 h 23 min 05 s | Création du compte local `support_it` sous le compte `jtremblay` | 02 | Fait |
| 9 h 23 min 07 s | Ajout de `support_it` au groupe Administrateurs | 02 | Fait |
| — | Ces deux actions ont été effectuées automatiquement par le logiciel malveillant (porte dérobée) | 02 + 03 | Hypothèse |
| 9 h 31 | Nouvelle connexion de `svc_inventaire` (normal) | 02 | Fait |
| ≈ 9 h 45 | Le poste devient très lent | 01 (témoignage) | Fait déclaré |
| — | La lenteur est causée par le faux `svchost.exe` (62,3 % du processeur) | 01 + 03 | Hypothèse |
| 9 h 50 | Analyse rapide lancée par Julie : aucune menace détectée | 05 | Fait |
| 9 h 57 | Le faux `svchost.exe` est toujours actif (62,3 % du processeur) | 03 | Fait |
| 9 h 58 | Connexion établie du faux `svchost.exe` vers `203.0.113.45:8443` | 04 | Fait |
| — | `203.0.113.45` est un serveur contrôlé par l'attaquant (C2 ou exfiltration) | 04 | Hypothèse |
| ≈ 10 h | Julie appelle le soutien informatique (témoignage noté à 10 h 02) | 00, 01 | Fait |
| ? | Julie a-t-elle cliqué sur le lien et saisi son mot de passe? | 01 | **Inconnu** |

Le témoignage de Julie est un **fait déclaré** : on sait qu'elle l'a dit, mais ses heures sont approximatives et ses souvenirs peuvent être incomplets. Il est donc préférable de le confirmer par les autres fichiers (ce qui est le cas : 9 h 20 concorde avec 9 h 21 min 39 s).

**Questions**

1. **Origine :** L'**exécution de la pièce jointe** `Avis_mise_a_jour.pdf.exe` vers 9 h 20, avec **acceptation de la demande d'autorisation** (UAC), ce qui a donné au programme les droits administrateur. La réception du courriel (9 h 12) en est le point de départ, mais c'est l'exécution qui déclenche la compromission.
2. **Enchaînement rapproché :** 9 h 21 min 39 s (faux `svchost.exe` démarre) → 9 h 21 min 41 s (mise en quarantaine) → 9 h 23 min 05 s et 07 s (création de `support_it` et ajout aux Administrateurs). Tout s'est fait en moins de 2 minutes. Cet enchaînement suggère une **attaque automatisée**, et montre que l'antivirus a réagi **trop tard** : il a mis en quarantaine, mais le processus était déjà actif.
3. **Information manquante :** On ne sait pas si Julie a **cliqué sur le lien** et **saisi son mot de passe** sur `cegepvl-soutien.com`. Il faudrait consulter l'**historique du navigateur**, les **journaux DNS** (visite du domaine frauduleux?) et les **journaux de connexion du compte de Julie** (par exemple Microsoft 365) pour repérer des connexions depuis des adresses inhabituelles. Même si elle n'a rien saisi, son mot de passe doit être considéré comme **potentiellement compromis**, puisqu'un logiciel malveillant s'exécute dans sa session (il pourrait par exemple enregistrer ses frappes au clavier).

---

##### 3. Évaluer l'incident

1. **Le poste est-il compromis?** **Oui.** Indices provenant de fichiers différents :
   - **01** : un exécutable déguisé en PDF a été ouvert et l'autorisation administrateur a été accordée ;
   - **02** : un compte administrateur `support_it` a été créé moins de 2 minutes plus tard ;
   - **03** : un faux `svchost.exe` non signé, lancé depuis `AppData`, utilise 62 % du processeur et a démarré à 9 h 21 ;
   - **04** : ce même processus maintient une connexion vers une adresse externe inconnue (`203.0.113.45:8443`) ;
   - **05** : Defender a détecté l'exécution dangereuse de `Avis_mise_a_jour.pdf.exe` et l'a mis en quarantaine.
2. **Scénario le plus probable.** Julie a reçu un courriel d'hameçonnage usurpant le service informatique. Croyant ouvrir un PDF, elle a lancé un exécutable et lui a accordé les droits administrateur. L'exécutable a installé un logiciel malveillant déguisé en `svchost.exe` dans son profil, juste avant que l'antivirus ne le mette en quarantaine. Le logiciel malveillant a ensuite créé un compte administrateur caché (`support_it`) pour conserver l'accès, puis établi une connexion vers un serveur externe contrôlé par l'attaquant. Il était toujours actif au moment de l'intervention.
3. **Principaux risques.**
   - **Pour Julie** : vol de ses identifiants (et de tout autre compte utilisant le même mot de passe) ; usurpation de son compte de courriel pour tromper ses collègues ou des fournisseurs ; atteinte à ses renseignements personnels.
   - **Pour le cégep** :
     - **Confidentialité** : exfiltration de données financières et de renseignements personnels (employés, étudiants, fournisseurs), accessibles depuis le poste ou le serveur de fichiers. 
     {{%notice style="tip" title="À prendre en compte"%}}
     Un tel incident pourrait constituer un **incident de confidentialité** au sens de la **Loi 25**, avec l'obligation de l'évaluer, de l'inscrire au registre et, en cas de risque de préjudice sérieux, d'aviser la Commission d'accès à l'information et les personnes concernées.
     {{%/notice%}}
     - **Intégrité et fraude** : le Service des finances est une cible de choix pour la fraude (fausses factures, détournement de paiements depuis un compte légitime).
     - **Propagation** : l'attaquant dispose d'un accès administrateur persistant et pourrait se déplacer vers d'autres postes ou vers le serveur de fichiers.
     - **Disponibilité** : risque de rançongiciel sur les partages réseau ; lenteur actuelle du poste.
     - **Réputation** et coûts de la remise en état.
4. **Faiblesses ayant facilité l'attaque.**
   - **Droits administrateur** pour l'utilisatrice (non-respect du moindre privilège) : un simple clic sur « Oui » a suffi pour créer un compte administrateur.
   - **Extensions de fichiers masquées** : la double extension `.pdf.exe` est passée inaperçue.
   - **Signatures antivirus non à jour** depuis 4 jours.
   - **Filtrage de courriel insuffisant** : une archive contenant un `.exe`, envoyée depuis un domaine imitant celui du cégep, a atteint la boîte de réception.
   - **Manque de sensibilisation** : l'utilisatrice a suivi les instructions sans vérifier la demande par un canal indépendant, malgré plusieurs signaux d'alerte.
   - **Absence de surveillance centralisée** : la détection de 9 h 21 et la création d'un compte administrateur à 9 h 23 n'ont déclenché **aucune réaction** du service informatique ; c'est l'utilisatrice qui a signalé le problème 40 minutes plus tard.

---

##### 4. Proposer un plan d'intervention

| Étape | Actions proposées | Justification |
|---|---|---|
| **1. Contenir** | Isoler le poste du réseau (débrancher le câble, couper le Wi-Fi ou isoler le poste avec l'EDR) **sans l'éteindre**. Bloquer `203.0.113.45` et le domaine `cegepvl-soutien.com` au pare-feu. **Désactiver** (sans supprimer) le compte `support_it`. Réinitialiser le mot de passe de Julie **depuis un autre appareil fiable** et fermer ses sessions actives. Retirer le courriel des autres boîtes de réception et avertir le personnel. | Couper la communication avec l'attaquant et empêcher l'exfiltration ou la propagation vers le serveur de fichiers. Éteindre le poste effacerait la mémoire vive (processus, connexions). Changer le mot de passe depuis le poste compromis pourrait permettre au logiciel malveillant de capturer le nouveau. Désactiver plutôt que supprimer conserve les traces. |
| **2. Préserver les preuves** | Exporter les journaux Windows (Sécurité, Système, Defender). Conserver le courriel original avec ses en-têtes, le fichier en quarantaine et le faux `svchost.exe`. Faire une image du disque. Documenter chaque action (qui, quoi, quand). | Ces éléments permettent de comprendre l'attaque, d'en mesurer l'ampleur et de rechercher d'autres victimes. La documentation est nécessaire si l'incident a des suites légales ou doit être déclaré. |
| **3. Investiguer** | Vérifier si Julie a visité le faux portail et saisi son mot de passe (historique du navigateur, journaux DNS, connexions inhabituelles à son compte). Rechercher les autres destinataires du courriel et les autres postes communiquant avec `203.0.113.45`. Vérifier si `support_it` a été utilisé. Mesurer le volume de données envoyées à `203.0.113.45` et les fichiers consultés sur `10.20.0.10`. Chercher d'autres mécanismes de persistance (tâches planifiées, services, clés de démarrage). Évaluer si des renseignements personnels ont été compromis. | Déterminer ce qui a été volé, si d'autres postes ou comptes sont touchés et quelles obligations légales s'appliquent. Il ne faut pas supposer que l'incident se limite à ce poste. |
| **4. Rétablir** | **Réinstaller le poste** à partir d'une image fiable plutôt que de tenter de le « nettoyer ». Supprimer le compte `support_it` (une fois les preuves conservées). Restaurer les fichiers de Julie à partir d'une **sauvegarde antérieure à 9 h 20**, vérifiée saine. Appliquer toutes les mises à jour (système et signatures). Changer les mots de passe de tous les comptes utilisés sur le poste. Surveiller le poste de près après sa remise en service. | Un attaquant ayant eu des droits administrateur a pu modifier le système de façon difficile à détecter : on ne peut plus faire confiance au poste. La sauvegarde garantit qu'on ne restaure pas des fichiers compromis. |
| **5. Prévenir** | Retirer Julie du groupe Administrateurs (compte standard ; compte administrateur distinct géré par le service informatique). **Afficher les extensions** de fichiers. Automatiser la mise à jour des signatures. Renforcer le filtrage de courriel (bloquer les exécutables, même dans une archive ; bannière « expéditeur externe » ; détection des domaines ressemblants). Activer l'**authentification multifacteur**.  Former le personnel et rappeler la procédure : le service informatique ne demande jamais un mot de passe, et toute demande inhabituelle se vérifie par un canal indépendant. | Chaque mesure corrige une faiblesse relevée à la partie 3.4. Combinées, elles appliquent le principe de **défense en profondeur** : si une mesure échoue (par exemple, l'utilisateur clique), les autres limitent les conséquences. La MFA rend un mot de passe volé beaucoup moins utile. |

<!-- Prévenir : Centraliser les alertes (EDR, SIEM) et alerter sur les événements 4720 et 4732. -->

{{%notice style="tip" title="À retenir"%}}
**À retenir** : l'ordre des actions compte. On **contient** d'abord, on **préserve les preuves** avant toute action irréversible (éteindre, supprimer des fichiers ou des comptes), et on change un mot de passe compromis **depuis un appareil fiable**.
{{%/notice%}}
