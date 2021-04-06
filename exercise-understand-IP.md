# Exercice - Comprendre les IP et le routage

## Objectifs
* analyser l’adressage IP dans un environnement réseau privé et public
* découvrir la notion de routes sur un réseau

## Connaitre et comprendre son adresse IP : 

* Rechercher l’adresse IP de son interface réseau.
* L’adresse IP de votre interface est-elle privée ou publique ? Justifier.
* Vérifier en utilisant la commande whois
* L’adresse IP de votre interface est-elle statique ou dynamique ? Justifier. 
* Rechercher son adresse IP publique
* * A partir de la ligne de commande :
```
wget -O - http://www.monip.org | grep -Eo "([0-9]+\.){3}[0-9]+"
wget -O - http://www.monip.org | grep -Eo "([0-9]{1,3}\.){3}[0-9]{1,3}"#lynx --source www.monip.org | grep -Eo "([0-9]{1,3}\.){3}[0-9]{1,3}"90.29.215.198
```
* * A partir de votre navigateur préféré :Liste de sites consultables : http://www.whatismyip.com/, http://monip.org/, http://www.connaitre-son-ip.com/, http://www.mon-ip.com/, http://www.adresseip.com/ ou encore http://www.monip.biz/

* Vérifier l’état de connexion vers votre adresse publique

* A-t-elle un nom de domaine associé ?

## Découvrir le routage 

* Nous allons travailler avec l'utilitaire traceroute, il est donc nécessaire de l'installer si celui-ci n'est pas présent sur votre système.
* Nous cherchons à définir le routing vers l'url www.kovalibre.com 
* Utiliser traceroute et ces options pour obtenir le nombre de routeurs traversés 
* Identifier avec la commande whois à qui appartiennent ces différents réseaux 
* Identifier l'adresse IP du serveur web www.kovalibre.com
* Déterminer l'adressage IP réseau de ce serveur. 
* Pour obtenir des informations sur le réseau, installer l'utilitaire whatmask. 
* Utiliser le sur l'adresse IP du réseau obtenu. 