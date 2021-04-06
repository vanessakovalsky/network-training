# Exercice - Découverte du service DNS

## Objectif : 
* Découvrir le service DNS

## Découvrir le DNS 
* Afficher le contenu du fichier /etc/hosts 
* Dans quel ordre se fera la résolution de noms sur votre machine ? (la réponse se trouve dans le fichier /etc/host.conf)
* Quelle est l'adresse IP du serveur de noms DNS que le résolveur interrogera ? (/etc/resolv.conf)
* Modifier le fichier de configuration de la résolution des noms pour qu'il interroge les serveurs DNS suivants : 
```
OpenDNS :
DNS Primaire : 208.67.222.222
DNS Secondaire : 208.67.220.220 
```

## Utiliser Dig pour obtenir plus d'information
* Déterminer l'adresse de kovalibre.com
* Installer dig si nécessaire
* Avec dig déterminer si la réponse du serveur DNS supporte la récursivité et si sa réponse fait autorité : 
```
dig kovalibre.com
```
* Remarque: les enregistrements NS (name server) permettent de spécifier les serveurs de noms ayant autorité sur le domaine. Chaque fichier de zone comporte en général un tel enregistrement. Dans la zone org, les record NS suivants créent le sous­domaine kovalibre et délèguent celui­ci vers lesserveurs indiqués. L'ordre des serveurs est quelconque. Tous les serveurs indiqués doivent faire autoritépour le domaine.
* Déterminer le serveur a utiliser pour obtenir une réponse  "authoritative". Ce serveur supporte t'il la récursivité ?
```
host ­-v ­-t ns kovalibre.com
dig @<serveur_dns> kovalibre.com
```
* Quelle réponse vous donne un serveur DNS lorsqu’il ne supporte pas la récursivité et qu’il ne connaît pas la réponse à votre question ? 
* Visualisez, avec l’option +trace la suite des serveurs contactés pourtrouver l’adresse IP de www.kovalibre.com
*  Quels sont les domaines traversés et les serveurs de nomsinterrogés ? La requête est­ elle récursive ?
* Recherchez plusieurs fois l’adresse www.kovalibre.com Que remarquez­ vous ?
* Qui est en charge de la zone com ?
* Comment déterminer la durée de validité d’une adresse ('A') ?
* Quelle est la durée de vie de l’adresse www.dyndns.org et celle destation­-stchamas.dyndns.org ?
* Effectuez plusieurs requêtes successivement. Que remarquez ­vous ?
* Déterminez le nom de la machine d’adresse 192.0.32.7 et le serveurde noms qui gère cette résolution inverse.
* www.google.fr est ­il le nom canonique ou un alias?
* Déterminez le ou les serveur(s) d’échange de courrier pour le domaine kovalibre.com
```
dig -mx kovalibre.com
```

## Cas concret - Identifier une adresse à partir d'une IP 

* On «ping» un site hébergé par free.fr, donc en Europe, pourobtenir son adresse IP
```
ping ­-c 1 tvaira.free.fr
```
* On interroge RIPE (pour l'Europe) pour en savoir plus sur cetteadresse :
```
whois -­h whois.ripe.net 212.27.63.132 | grep ­-E ­-e "inetnum|route"
```
* On obtient l'adresse CIDR donné à l'ISP Free SA par RIPE soit 212.27.32.0/19. On peut vérifier :
```
whois ­-h whois.ripe.net 212.27.32.0/19 | grep ­-E ­-e "inetnum|route|descr"
```
* On "trace" la route pour atteindre l'adresse IP du site 
```
traceroute ­-nI 212.27.63.132
```
Les routeurs 10 et 11 sont bien des routeurs de chez Free dans des sous-réseaux différents (les 2 adresses sont dans la plage 212.27.32.0 ­ 212.27.63.255 de chez Free)
* On vérifie sur le site www.iana.org à qui appartient l'adresse 212.x.x.x. 
```
wget ­-q ­-U "" -­O ­http://www.iana.org/assignments/ipv4­address­space/ipv4­address­space.xml | grep -­A 4 "212"
```
L'IANA a attribué cette adresse 212/8 à RIPE NCC (Réseaux IP Européens Network Coordination Center) Europe depuis octobre 1997. RIPE l'a ensuite découpée en plusieurs adresses (RIPE a la charge du netmask)pour l'attribuer à différents ISP ou sites.

* À l'étape 1, on a directement «pingé» le nom du site pour obtenir son adresse IP. C'est une méthode que l'on peut qualifier de "bruyante" (on a enquelque sorte alerté la cible). Il est possible d'utiliser une méthode plus "silencieuse".
* On recherche les serveurs DNS de free.fr en interrogeant NIC 
```
whois -­h whois.nic.fr free.fr | grep ­-E -­e "domain|address|nserver"
```
* On interroge maintenant le premier serveur DNS pour savoir s'il connaît le site xxx (si oui on obtiendra son adresse IP) 
```
host -­v ­-a tvaira.free.fr 212.27.60.19
```
* Pour terminer, on localise géographique l'adresse IP trouvée en utilisant les services de http://ipinfodb.com/

