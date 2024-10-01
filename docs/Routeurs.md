# Routeurs

- 1. Rénitialisation du routeur
installer : sudo apt install minicom 
faire minicom sudo minicom - s
configuration du port série avec /dev/tty
aller dans le ROMmon avec ctrl pause 
commande confreg 0x2142
reset

- 2. création du Vlan de management 



- 3. ssh 

 


- 4. configuration interfaces virtuelles 

- 5. 



## NAT

permettre à plusieurs dispositifs d'une réseau local (IP inside) de partager une seule adresse IP publique (IP outside) pour accéder à Internet ou d'autres réseaux externes.

## HSRP

Permet d'asurer la redondance des passerelles en créant une adresse IP virtuelle partagée dans notre cas 192.168.10.254. 

Un routeur actif gère le trafic, tandis qu’un routeur en veille prend leCoeur de réseau relais en cas de panne. Les routeurs échangent des paquets pour surveiller leur état, garantissant ainsi une haute disponibilité du réseau.

Dans ce cas avec le routeur Fibre et celui de L'ADSL nous devons les configurer les interfaces définir une priorité qui sera plus élévés pour l'un dans ce cas celui de l'ADSL (priorité de 110).

Grace à cette configuration le routeur ADSL sera le routeur actif tant qu'il est disponible. En cas de panne du routeur ADSL ou de sa connexion, le routeur de la fibre prendra automatiquement le relais en tant que routeur actif. Coeur de réseau


## access list sur les routeurs