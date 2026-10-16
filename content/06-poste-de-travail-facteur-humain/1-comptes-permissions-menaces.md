+++
title = 'Comptes, permissions et menaces courantes'
weight = '610'
draft = false
+++

----------------

## Comptes et permissions

La gestion des comptes et des permissions a été abordée au chapitre 5. On retiendra notamment les principes suivants :

- **Principe du moindre privilège** : chaque utilisateur ne devrait disposer que des droits nécessaires à ses tâches.
- **Séparation des rôles (RBAC)** : les opérations administratives devraient être réalisées avec un compte adapté, distinct du compte utilisé pour les activités courantes.
- **Verrouillage de session** : une session inutilisée devrait être verrouillée afin d'empêcher une personne non autorisée d'accéder au poste.

Ces mesures limitent les conséquences d'une erreur humaine ou de la compromission d'un compte.

Par exemple, si un utilisateur travaille avec un compte standard plutôt qu'un compte administrateur, un logiciel malveillant exécuté sous ce compte dispose généralement de moins de permissions pour modifier le système.

## Menaces et vulnérabilités

Une **menace** est un élément susceptible de causer un dommage à un système informatique. Une **vulnérabilité** est une faiblesse qui peut être exploitée pour compromettre ce système.

Par exemple, un logiciel qui n'a pas reçu un correctif de sécurité peut présenter une vulnérabilité exploitable par un attaquant.

Les principales menaces visant les postes de travail sont les suivantes :

### Logiciels malveillants (*malware*)

Un logiciel malveillant est un programme conçu pour nuire à un système, voler des informations ou effectuer des actions non autorisées.

Les principales catégories sont :

- **Virus** : infecte des fichiers ou des programmes et peut se propager lorsqu'ils sont utilisés ou partagés.
- **Ver informatique (*worm*)** : se propage automatiquement entre des systèmes, notamment par le réseau.
- **Cheval de Troie (*Trojan horse*)** : se présente comme un programme légitime, mais dissimule des fonctionnalités malveillantes.
- **Rançongiciel (*ransomware*)** : chiffre des fichiers ou bloque l'accès à un système afin d'exiger une rançon.
- **Logiciel espion (*spyware*)** : collecte des informations sur l'utilisateur à son insu.

Un même logiciel malveillant peut combiner plusieurs de ces comportements. Par exemple, un rançongiciel peut voler des données avant de chiffrer les fichiers de sa victime.

{{% notice style="info" title="Comment un poste peut-il être infecté ?" %}}

Un logiciel malveillant peut exploiter une vulnérabilité technique, mais il peut également être exécuté à la suite d'une action de l'utilisateur.

Par exemple, un utilisateur peut télécharger un faux logiciel ou ouvrir une pièce jointe malveillante reçue par courriel.

{{% /notice %}}

### Keyloggers et vol d'informations

Un **keylogger** est un logiciel ou un dispositif matériel qui enregistre les frappes effectuées au clavier à l'insu de l'utilisateur.

Un attaquant peut ainsi chercher à récupérer des mots de passe, des messages confidentiels ou d'autres informations sensibles.

D'autres logiciels malveillants peuvent également capturer des écrans, surveiller le presse-papiers ou récupérer des fichiers.

### Autres vecteurs de compromission

Un poste peut également être compromis par :

- **Pièces jointes malveillantes** : documents ou programmes contenant du code dangereux.
- **Téléchargements douteux** : logiciels provenant de sources non fiables.
- **Supports amovibles inconnus** : clés USB ou disques externes contenant éventuellement des fichiers malveillants.
- **Vulnérabilités non corrigées** : failles dans un système d'exploitation ou une application.
- **Intrusions physiques** : accès à un poste laissé sans surveillance ou à des équipements insuffisamment protégés.

La sécurité d'un poste dépend donc de sa configuration, des logiciels utilisés et des comportements de ses utilisateurs.


