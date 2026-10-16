
## 6.4 - Facteur humain et ingénierie sociale

### Le facteur humain

Le **facteur humain** désigne l'influence des comportements, des décisions et des erreurs humaines sur la sécurité informatique.

Un utilisateur peut involontairement compromettre un système en ouvrant un fichier malveillant, en communiquant ses identifiants ou en laissant sa session ouverte.

Les attaquants cherchent donc parfois à manipuler les utilisateurs plutôt qu'à exploiter directement une vulnérabilité technique.

### Ingénierie sociale

L'**ingénierie sociale** consiste à exploiter la confiance, la politesse, la curiosité, la peur ou l'urgence perçue pour amener une personne à divulguer des informations sensibles ou à effectuer une action compromettante.

L'attaquant peut se faire passer pour un collègue, un gestionnaire, un technicien informatique ou un fournisseur.

Par exemple, une personne prétendant travailler au service informatique peut demander à un employé son mot de passe sous prétexte de résoudre un problème urgent.

### Principales techniques d'ingénierie sociale

- **Hameçonnage (*phishing*)** : message frauduleux incitant la victime à cliquer sur un lien, ouvrir une pièce jointe ou communiquer des informations.
- **Harponnage (*spear phishing*)** : hameçonnage ciblant une personne ou une organisation précise, à l'aide d'un message personnalisé.
- **Usurpation d'identité et prétexte (*pretexting*)** : attaquant qui invente un scénario pour se faire passer pour une personne de confiance et obtenir des informations ou un accès.
- **Appât (*baiting*)** : utilisation d'un élément attirant la curiosité de la victime, comme une clé USB inconnue.
- **Vishing et smishing** : hameçonnage par téléphone et par SMS.

Ces techniques exploitent souvent des réactions humaines normales. Un message peut être crédible et bien rédigé tout en étant frauduleux.

### Reconnaître une tentative d'ingénierie sociale

Certains indices doivent éveiller les soupçons :

- une demande inhabituelle ou urgente ;
- une demande de mot de passe ou de code MFA ;
- une adresse d'expéditeur ou un lien suspect ;
- une pièce jointe inattendue ;
- une demande de contourner une procédure habituelle.

Il ne faut pas se fier uniquement à l'apparence d'un message. L'identité de l'expéditeur et la légitimité de la demande doivent être vérifiées.

### Bonnes pratiques

Pour réduire les risques d'ingénierie sociale, les utilisateurs devraient :

1. Vérifier les demandes inhabituelles par un canal indépendant.
2. Ne jamais communiquer leur mot de passe ou un code MFA à une personne qui le demande.
3. Éviter d'ouvrir les liens et les pièces jointes suspects.
4. Ne pas connecter de supports amovibles inconnus.
5. Verrouiller leur poste lorsqu'ils s'en éloignent.
6. Signaler rapidement les messages suspects au service informatique.

### Que faire en cas de compromission suspectée ?

Si un utilisateur ouvre une pièce jointe suspecte ou communique ses identifiants par erreur, il doit rapidement prévenir le service informatique et suivre les procédures prévues.

Selon la situation, il peut être nécessaire d'isoler le poste du réseau, de modifier les identifiants compromis depuis un appareil fiable et de vérifier si d'autres comptes ont été affectés.

Il faut éviter de supprimer les fichiers ou les journaux qui pourraient aider à comprendre ce qui s'est passé.

### Sensibilisation et responsabilité partagée

La sensibilisation des utilisateurs constitue une mesure de sécurité à part entière. Elle permet de reconnaître les menaces, d'adopter de bonnes pratiques et de signaler rapidement les incidents.

Dans un **système de management de la sécurité de l'information (SMSI)**, la formation, les procédures et l'amélioration continue contribuent à intégrer la sécurité dans les activités quotidiennes.

{{% notice style="info" title="La sécurité repose sur plusieurs couches" %}}

Un incident peut résulter de plusieurs défaillances combinées : un message frauduleux, une erreur humaine, un poste insuffisamment protégé et une absence de surveillance.

La sécurité efficace repose sur une combinaison de protections techniques, de procédures organisationnelles et de comportements adaptés.

{{% /notice %}}