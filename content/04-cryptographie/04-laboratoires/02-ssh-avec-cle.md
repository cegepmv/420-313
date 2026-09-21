+++
title = '2- SSH avec paire de clés'
weight = '443'
draft = false
+++
----------------

## Objectifs

- générer une paire de clés SSH;
- distinguer une clé privée d'une clé publique;
- utiliser une clé publique pour s'authentifier auprès d'un serveur;
- configurer SSH pour utiliser l'authentification par clé;
- désactiver l'authentification par mot de passe;
- comprendre une application concrète de la cryptographie à clé publique.


## Architecture du laboratoire

![Architecture du laboratoire 2](/images/04-lab2-architecture.png)

+ Deux machines virtuelles vous sont fournies: 
  - **VM1** sera la machine cliente;
  - **VM2** sera le serveur SSH;
+ vous utiliserez VM1 pour vous connecter à VM2;
+ VM2 ne devra plus accepter l'authentification SSH par mot de passe.

{{%notice style="info" title="Important"%}}
Ne désactivez pas l'authentification par mot de passe avant d'avoir vérifié que l'authentification par clé fonctionne correctement.
{{%/notice%}}

## Étapes du laboratoire

##### 1. Identifier les machines

Votre enseignant vous a communiqué les adresses IP de vos deux machines virtuelles :

```text
VM1 : <IP_VM1>
VM2 : <IP_VM2>
```

Vous vous connecterez aux deux machines avec l'utilisateur :

```text
root
```

Dans la suite du laboratoire, remplacez `<IP_VM1>` et `<IP_VM2>` par les adresses IP qui vous ont été fournies.

Vous pouvez confirmer que vous êtes bien sur la bonne machine avec :

```bash
hostname
ip a
```

| Information | VM1 | VM2 |
|---|---|---|
| Nom d'hôte | | |
| Adresse IP | `<IP_VM1>` (fournie) | `<IP_VM2>` (fournie) |
| Utilisateur | root | root |

##### 2. Tester l'authentification actuelle

Depuis VM1, connectez-vous à VM2 avec SSH :

```bash
ssh root@<IP_VM2>
```

Vous devriez actuellement pouvoir vous connecter avec le mot de passe du compte.

Une fois connecté, vérifiez que vous êtes bien sur VM2 :

```bash
hostname
```

Puis quittez la connexion :

```bash
exit
```

##### 3. Générer une paire de clés SSH

Sur **VM1**, générez une paire de clés avec *Ed25519* :

```bash
ssh-keygen -t ed25519
```

Lorsque le programme demande :

```text
Enter file in which to save the key:
```

Appuyez sur **Entrée** pour utiliser l'emplacement proposé.

Vous pouvez ensuite définir une phrase secrète (*passphrase*) pour protéger votre clé privée.


{{%notice style="tip" title="À retenir"%}}

La commande crée généralement deux fichiers :

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

La clé privée :

```text
~/.ssh/id_ed25519
```

La clé publique :

```text
~/.ssh/id_ed25519.pub
```
{{%/notice%}}

##### 4. Observer les deux clés

Affichez les permissions :

```bash
ls -l ~/.ssh/id_ed25519*
```

Affichez votre clé publique :

```bash
cat ~/.ssh/id_ed25519.pub
```

Vous pouvez afficher la clé publique, mais **ne partagez jamais votre clé privée**.

Pour observer les caractéristiques de la clé publique :

```bash
ssh-keygen -lf ~/.ssh/id_ed25519.pub
```

###### **Questions**
1. Quel fichier correspond à la clé privée ?
2. Quel fichier correspond à la clé publique ?
3. Pourquoi la clé privée ne doit-elle jamais être copiée sur le serveur ?
4. Quel algorithme est utilisé par cette paire de clés ?
5. À quoi sert la *passphrase* ?

##### 5. Installer la clé publique sur VM2

Depuis VM1 :

```bash
ssh-copy-id <UTILISATEUR>@<IP_VM2>
```

Le programme vous demandera le mot de passe du compte sur VM2.

Cette étape copie la clé publique sur VM2 dans le fichier :

```text
~/.ssh/authorized_keys
```

##### 6. Vérifier la clé sur VM2

Connectez-vous à VM2 :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Puis vérifiez :

```bash
cat ~/.ssh/authorized_keys
```

Vous devriez retrouver votre clé publique.

##### 7. Tester l'authentification par clé

Depuis VM1 :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Cette fois, SSH devrait utiliser votre clé.

Si vous avez défini une *passphrase*, SSH peut vous demander :

```text
Enter passphrase for key ...
```

Il s'agit de la **phrase secrète protégeant votre clé privée**, et non du mot de passe du compte sur VM2.

##### 8. Vérifier que la clé est utilisée

Utilisez le mode détaillé :

```bash
ssh -v <UTILISATEUR>@<IP_VM2>
```

Repérez dans la sortie les messages indiquant qu'une clé publique est utilisée.

Vous pouvez également utiliser :

```bash
ssh -vv <UTILISATEUR>@<IP_VM2>
```

###### **Questions**

6. Quelle différence y a-t-il entre le mot de passe du compte et la passphrase de la clé privée ?
7. Où la clé publique est-elle enregistrée sur VM2 ?
8. Pourquoi est-il possible de placer la clé publique sur le serveur sans compromettre la clé privée ?
9. Quelle clé reste sur VM1 ?
10. Quelle clé est copiée sur VM2 ?

##### 9. Désactiver l'authentification par mot de passe

Une fois que vous avez confirmé que l'authentification par clé fonctionne, connectez-vous à VM2 et modifiez :

```bash
sudo nano /etc/ssh/sshd_config
```

Vérifiez que les paramètres suivants sont présents :

```text
PubkeyAuthentication yes
PasswordAuthentication no
```

##### 10. Vérifier la configuration SSH

Avant de redémarrer SSH :

```bash
sudo sshd -t
```

Si aucune erreur n'est affichée, la syntaxe est correcte.

Redémarrez ensuite le service :

```bash
sudo systemctl restart ssh
```

##### 11. Tester l'interdiction du mot de passe

Depuis VM1, votre connexion par clé doit toujours fonctionner :

```bash
ssh <UTILISATEUR>@<IP_VM2>
```

Pour tester que l'authentification par mot de passe est bien désactivée, vous pouvez forcer temporairement SSH à ne pas utiliser de clé :

```bash
ssh -o PubkeyAuthentication=no <UTILISATEUR>@<IP_VM2>
```

La connexion devrait être refusée.

{{%notice style="warning" title="Attention"%}}
Ne fermez pas votre dernière session SSH fonctionnelle avant d'avoir vérifié que la nouvelle configuration fonctionne.
{{%/notice%}}

<!-- ## 12. Limiter SSH à VM1

L'authentification par clé empêche les utilisateurs ne possédant pas la clé privée de s'authentifier.

Cependant, si vous souhaitez que **VM2 n'accepte les connexions SSH que depuis VM1**, vous pouvez également utiliser le pare-feu.

Sur VM2 :

```bash
sudo ufw status
```

Si UFW est utilisé dans votre environnement, vous pouvez autoriser SSH uniquement depuis VM1 :

```bash
sudo ufw allow from <IP_VM1> to any port 22 proto tcp
```

Puis :

```bash
sudo ufw enable
```

Vérifiez :

```bash
sudo ufw status
```

La règle devrait permettre les connexions SSH provenant de VM1.

--- -->

### Questions de synthèse

11. Pourquoi peut-on considérer VM1 comme un *jump server* dans cette architecture ?
12. Quelles sont les deux mesures utilisées pour protéger l'accès SSH à VM2 ?
13. Quelle serait la conséquence si quelqu'un obtenait une copie de votre clé privée ?
14. Pourquoi une clé privée protégée par une passphrase est-elle plus sécuritaire qu'une clé privée sans protection ?
