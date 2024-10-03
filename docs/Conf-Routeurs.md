# Le materiel

Pour fournir l'accés internet a notre infrastructure nous avons a notre disposition deux routeurs cisco 1921.<br>

- L'un fournira internet haut débit en <b>Fibre</b>, il sera notre connection primaire<br>

- L'autre fournira internet en <b>ADSL</b> il, sera notre connection secondaire.

Pour effectuer une priorité sur le trafic nous allons utiliser le protocole <b>HSRP</b>.


# Rénitialisation des routeurs

Pour avoir accées a notre routeur en port serie sur nos machines linux il faut installer et configurer le minicom avec les commandes suivantes :

        sudo apt install minicom 
        minicom sudo minicom - s
        /dev/tty
Nous pouvons maintenant nous connecter a notre routeur grace au port serie et au logiciel <b>putty</b>

Une fois connecter nous allons aller dans le <b>ROMmon</b> avec <b>ctrl pause</b> 

Le ROMon s'ouvre et nous pouvons y entrer la commande :

        confreg 0x2142
        reset

# Configuration du <b>SSH</b>

Afin de mettre en place notre <b>SSH</b> nous devons d'abord mettre en place notre <b>VLAN</b> de management en créant une interface virtuelle sur notre 



## NAT

permettre à plusieurs dispositifs d'une réseau local (IP inside) de partager une seule adresse IP publique (IP outside) pour accéder à Internet ou d'autres réseaux externes.

## access list sur les routeurs