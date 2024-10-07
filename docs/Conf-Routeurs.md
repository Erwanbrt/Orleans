# Le materiel

Pour fournir l'accés internet a notre infrastructure nous avons a notre disposition deux routeurs cisco 1921.<br>

- L'un fournira internet haut débit en <b>Fibre</b>, il sera notre connection primaire<br>

- L'autre fournira internet en <b>ADSL</b> il, sera notre connection secondaire.

Pour effectuer une priorité sur le trafic nous allons utiliser le protocole <b>HSRP</b>.


# Rénitialisation des routeurs

Pour avoir accées a notre routeur en port serie sur nos machines linux il faut installer et configurer le minicom avec les commandes suivantes :

        sudo apt install minicom 
        sudo minicom - s
        /dev/tty
Nous pouvons maintenant nous connecter a notre routeur grace au port serie et au logiciel <b>putty</b>

Une fois connecter nous allons aller dans le <b>ROMmon</b> avec <b>ctrl pause</b> 

Le ROMon s'ouvre et nous pouvons y entrer la commande :

        confreg 0x2142
        reset

# Configuration du <b>SSH</b>

Afin de mettre en place notre <b>SSH</b> nous devons d'abord mettre en place notre <b>VLAN</b> de management en créant une interface virtuelle sur notre interface interne

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE# conf 
        RTR-ORLEANS-FIBRE(config)# interface GigabitEthernet 0/1.230
        RTR-ORLEANS-FIBRE(config-subif)# description VLAN Management
        RTR-ORLEANS-FIBRE(config-subif)# encapsulation dot1q 230
        RTR-ORLEANS-FIBRE(config-subif)# ip address 10.10.10.200 255.255.255.0
        RTR-ORLEANS-FIBRE(config-subif)# exit
        RTR-ORLEANS-FIBRE(config)#
        
        RTR-ORLEANS-FIBRE(config)# ip domain-name orleans.sportludique.fr
        RTR-ORLEANS-FIBRE(config)# crypto key generate rsa 2048
        RTR-ORLEANS-FIBRE(config)# ip ssh version 2
        RTR-ORLEANS-FIBRE(config)# ip ssh time-out 60
        RTR-ORLEANS-FIBRE(config)# ip ssh authentication-retries 2
        RTR-ORLEANS-FIBRE(config)# enable secret password <mot de passe>
        RTR-ORLEANS-FIBRE(config)# exit
        RTR-ORLEANS-FIBRE> write mem

## Configuration des interfaces

* Interface WAN (out):

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf
        RTR-ORLEANS-FIBRE#(config)interface GigabitEthernet 0/0
        RTR-ORLEANS-FIBRE#(config-if)description FIBRE
        RTR-ORLEANS-FIBRE#(config-if)ip addresse 183.44.45.1 255.255.255.252
        RTR-ORLEANS-FIBRE#(config-if)exit
        RTR-ORLEANS-FIBRE#(config)exit
        RTR-ORLEANS-FIBRE# write mem 
        

* Interface LAN (in): 

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf
        RTR-ORLEANS-FIBRE#(config)interface GigabitEthernet 0/1
        RTR-ORLEANS-FIBRE#(config-if)no ip address
        RTR-ORLEANS-FIBRE#(config-if)exit
        RTR-ORLEANS-FIBRE#(config)interface GigabitEthernet 0/1.239
        RTR-ORLEANS-FIBRE#(config-if) encapsulation dot1Q 239
        RTR-ORLEANS-FIBRE#(config-if) ip address 192.168.10.200 255.255.255.0
        RTR-ORLEANS-FIBRE#(config-if) exit
        RTR-ORLEANS-FIBRE#(config) exit
        RTR-ORLEANS-FIBRE# write mem

## Routage 

* Etablire les routes par défault
        
        RTR-ORLEANS-FIBRE#(config)ip route 0.0.0.0 0.0.0.0 221.87.145.1
        RTR-ORLEANS-FIBRE#(config)ip route 172.28.32.0 255.255.224.0 192.168.10.1
        RTR-ORLEANS-FIBRE#(config)ip route 192.168.45.0 255.255.255.0 192.168.10.1
        RTR-ORLEANS-FIBRE#(config)ip route 192.168.145.0.255.255.255.0 255.255.255.0 192.168.10.1

## NAT

Le <strong>NAT</strong> agit au niveau des <strong>routeurs</strong> ou des <strong>pare-feu.</strong> Lorsqu’un appareil du réseau local envoie une requête vers Internet, le routeur qui fait du <strong>NAT</strong> remplace l’adresse <strong>IP</strong> privée de l’appareil par une adresse <strong>IP</strong> publique avant de transmettre la requête. Lorsque la réponse revient de l’extérieur, le <strong>NAT</strong> rétablit l’adresse <strong>IP</strong> privée d’origine et redirige la réponse vers le bon appareil à l’intérieur du réseau local.

* Sur l'interface <strong>WAN</strong> (OUT):

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf
        RTR-ORLEANS-FIBRE#(config)interface GigabitEthernet 0/0
        RTR-ORLEANS-FIBRE#(config-if) ip nat outside
        RTR-ORLEANS-FIBRE#(config-if) exit
        RTR-ORLEANS-FIBRE#(config)ip nat source list 1 interface GigabitEthernet 0/0 overload
        RTR-ORLEANS-FIBRE#(config) exit
        RTR-ORLEANS-FIBRE# write mem

* Sur l'interface <strong>LAN</strong> (IN):

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf
        RTR-ORLEANS-FIBRE#(config)interface, GigabitEthernet 0/1.239
        RTR-ORLEANS-FIBRE#(config-if) ip nat inside
        RTR-ORLEANS-FIBRE#(config-if) exit
        RTR-ORLEANS-FIBRE#(config) exit
        RTR-ORLEANS-FIBRE# write mem



## ACL (Access List)

* Une Access Control List (<stong>ACL</strong>) est un ensemble de règles utilisées en informatique pour contrôler le trafic réseau. Elles définissent quelles connexions sont autorisées ou bloquées en fonction de critères comme l’adresse IP, le protocole <strong>(TCP, UDP)</strong>, ou les numéros de ports.

Les <Strong>ACL</strong>s sont couramment utilisées dans les <strong>routeurs</strong> et <stong>pare-feu</strong> pour sécuriser les réseaux en gérant quels paquets de données peuvent passer.

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf
        RTR-ORLEANS-FIBRE#(config)access-list 1 permit 172.28.32.0 0.0.31.255
        RTR-ORLEANS-FIBRE#(config)access-list 1 permit 192.168.45.0 0.0.0.255
        RTR-ORLEANS-FIBRE#(config)access-list 1 permit 192.168.145.0 0.0.0.255
        RTR-ORLEANS-FIBRE#(config)exit
        RTR-ORLEANS-FIBRE#write mem


         

