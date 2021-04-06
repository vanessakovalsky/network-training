# Exercice - Netstat pour obtenir des statistiques réseaux 

## Objectifs :
* Être capable d'afficher et d'exploiter les statistiques réseaux d'une machine.
* Être capable de mettre en oeuvre une surveillance de l'activité réseau de sa machine

## Afficher et exploiter les statistiques

* Afficher les statistiques avec la commande netstat -i
* A l'aide du man et d'internet trouvé à quoi correspond la colonne Flg 
* Déterminer combien de ping ont été émis à partir de votre machine ? 
* Exécuter la commande suivante et vérifier quelles sont les statistiques affectées par son exécution : 
```
ping www.ibm.fr ­s 1500 ­M do ­c 1
```
* Donner les options qui permettent d'afficher le nom et le PID desprocessus propriétaires des sockets TCP
* Qu'est­ ce qu'une socket «unix»?
* Démarrer un serveur TCP sur le port 5000 (avec netcat) et observer dans quel état se trouve la socket?
* Connecter un client TCP (avec telnet ou netcat) vers le port duserveur et observer les différents états avec la commande suivante.Commenter
```
netstat ­nta | grep ­E ":5000|Address"
```
* Mettre fin au client et observer à nouveau les états. Commenter

## Surveiller sa machine

Voici quelques commandes qui permettent d'exploiter les statistiques fournies par netstat afin de surveiller l'activité réseau de sa machine :

* Compter les états des sockets passives et actives
```
$ netstat ­nat | awk '{print $6}' | sort | uniq ­c | sort ­n
```
* Compter les états des sockets pour une adresse IP en particulier
```
netstat ­nat |grep {IP­address} | awk '{print $6}' | sort | uniq ­c |sort ­n
```
* Afficher la liste des adresses IP connectés
```
netstat ­nat | sed 1d | sed 1d | awk '{ print $5}' | cut ­d: ­f1 | sed­e '/^$/d' | uniq
```
* Afficher le nombre d'adresses IP connectés 
```
netstat ­nat | sed 1d | sed 1d | awk '{ print $5}' | cut ­d: ­f1 | sed­e '/^$/d' | uniq | wc ­l
```
* Compter le nombre de sockets connectés par adresse IP
```
netstat ­atun | sed 1d | sed 1d | awk '{print $5}' | cut ­d: ­f1 | sed­e '/^$/d' |sort | uniq ­c | sort ­n
netstat ­atun | awk '{print $5}' | sed ­n ­e '/[0­9]\{1,3\}\.[0­9]\{1,3\}\.[0­9]\{1,3\}\.[0­9]\{1,3\}/p' | sed 's/::ffff://' | cut ­d: ­f1| sort | uniq ­c | sort ­n
```
* Compter le nombre de sockets dans l'état ESTABLISHED par adresse IP
```
netstat ­atun | grep ESTABLISHED | awk '{print $5}' | cut ­d: ­f1 |sed ­e '/^$/d' |sort | uniq ­c | sort ­n
```


Nous allons maintenant créer un script de surveillance de notre service web HTTP qui effectuera les actions suivantes. 
* S'assurer que le service HTTP est bien démarré sur la machine
* Vérifier que le nombre d'adresse IP connecté n'est pas supérieur à 100 
* Vérifier que le nombre de socket connecté par adresse IP n'est pas supérieur à 3 
* Dans les cas où les seuils seraient dépassé ajouter un message d'erreur dans le fichier syslog

Une fois le script crée, le faire tourner toutes les 10 minutes à l'aide de crontab
