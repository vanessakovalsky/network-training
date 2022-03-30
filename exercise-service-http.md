# Exercice - Installer et sécuriser un serveur apache

## Objectifs : 
* Savoir installer un serveur web
* Sécuriser l'accès à un serveur web en mettant en place un pare-feu

## Installation du serveur web

* Pour installer le serveur web, commençons par installer Apache
``` 
apt install apache2 #debian
yum install httpd #red hat
```
* Démarrer le service : 
```
service apache2 start #debian
service httpd start #redhat
```
* Vérifier sur l'adresse locale : 
```
wget localhost 
``` 
-> Vous devriez obtenir une page html

## Sécurisation (d'un point de vue réseau)

* La sécurisation va se faire au travers de deux outils : iptables pour le pare-feu et fail2ban pour filtrer les adresse IP

### Mettre en place un pare-feu
* Installation de iptables : 
```
apt install -y iptables #debian
yum install -y iptables #redhat
```
* Activation des règles pour autoriser le trafic entrant sur le port 80 (HTTP) et le port 22 (SSH)
```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -I INPUT ! -i lo -p tcp --dport 80 -j DROP
```
* Pour tester, depuis une autre machine, essayer d'atteindre cette machine sur un autre port que le 80

### Mettre en place une protection contre les intrusions en filtrant les adresses IP

* Installation de fail2ban :
```
apt install -y fail2ban #debian
yum install -y fail2ban # redhat
```
* Démarrage du service 
```
service fail2ban start
```
* La configuration de fail2ban est détaillée ici : https://doc.ubuntu-fr.org/fail2ban#configuration 


-> Vous savez maintenant installer un service HTTP via un serveur web Apache et le sécuriser d'un point de vue réseau 
/!\ Aucune couche de sécurité est infaillible et ne doit être considéré comme une protection absolue, il est utile par exemple en plus, de gérer les comptes utilisateurs et les droits associés aux différents fichiers.    
