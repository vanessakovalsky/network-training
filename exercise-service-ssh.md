 # Exercice - Connexion SSH

 ## Objectifs 
 * Créer une clé SSH
 * Utiliser la clé SSH pour se connecter à un autre serveur

 ## Pré-requis 
 * Avoir au moins deux machines sous Linux afin de permettre de tester la connexion SSH

 ## Création de la clé SSH

 * Sur le poste `client` généré une clé SSH avec la commande ssh-keygen : 
```
 ssh-keygen -t rsa -b 4096
```
* La génération se fait en interactif, répondre aux questions posées en laissant les valeurs par défaut, et en n'oubliant pas de définir un mot de passe pour la clé 
* Explication des options utilisées :
* * -t rsa : tpe de clé à créé, ici RSA
* * -b 4096 : Nombre de bits dans la clé, ici 4096 

* La clé est alors créé sous la forme de deux fichiers présents dans le dossier ~/.ssh 
* * id_rsa : clé privé à laisser sur le client
* * id_rsa.pub : clé publique que l'on va déployer sur le serveur sur lequel on souhaite se connecter

## Déployer la clé et se connecter sur un serveur 
* Nous allons copier la clé générée sur le serveur sur lequel nous souhaitons nous connecter. Pour cela on uutilise la commande ssh-copy-id 
```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@serveur
```
* La commande ssh-copy-id permet de déployer une clé publique sur un serveur.
* * L'option -i permet de spécifier le fichier de clé publique à utiliser
* * Ensuite nous spécifions l'utilisateur qui doit se connecter et le nom ou l'adresse IP du serveur 

* Pour se connecter, il ne reste plus qu'à ouvrir une connexion : 
```
ssh user@serveur
```
* SSH vous demande alors la passphrase pour vous utiliser votre clé (c'est celle que vous avez défini lors de la génération)
* Vous devriez maintenant être connecté sur le serveur et pouvoir faire vos différentes actions

## Eviter de saisir sa passphrase à chaque fois que l'on se connecte
* Afin d'éviter de retaper la passphrase à chaque connexion il est possible en utilisant ssh-agent d'indiquer au systèmes quel clé utilisé et ainsi de ne pas resaisir sa passphrase à chaque connection :
```
eval "$(ssh-agent -s)"
```
* Une fois l'agent lancé, il suffit d'ajouter la clé privé concernée : 
```
ssh-add ~/.ssh/id_rsa
```