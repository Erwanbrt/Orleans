# Configuration

## L'équipement
 
Nous avons a notre disposition un switch CISCO Catalyst 3750-X Serie 24 ports stacker ce qui nous fais donc un coeur de réseau 48 ports.

* Le stacking : 

Pour effectuer le stacking nous avons du brancher les switch avec le cable satck et il ont fusionnée automatiquement.

Il est organiser comme suit : Les port du premier switch sont en 1/0/1 - 1/0/24 et les port du second sont en 2/0/1 - 2/0/24.

## La configuration 

#### Reset du switch

Afin  de commencer notre infrastructure sur un équipement propre nous l'avons reenitialiser en effexctuant un appuy long sur le boutton "Mode" en facade du switch. 

#### Configuration basique  

Afin de pouvoir securiser notre switch nous avons créer un nouvel utilisateur admin grace au commandes: 

    Switch> enable
    Switch# conf t
    Switch(config)# username <nomutilisateur> privilege <15> secret <motdepasse>
    Switch(config)# line vty 0 15
    Switch(config-line)# login local
    Switch(config-line)# exit
    Orleans(config)# exit
    Orleans> write

Une fois le nouvel utilisateur créer nous pouvons renomer le switch avec cette commande :

    Switch> enable
    Switch# conf t
    Switch(config)# hostname Orleans
    Orleans(config)# exit
    Orleans> write

#### Configurer le SSH 

Pour configurer le SSH nous allons initialiser le VLAN de management 230 (10.10.10.0/24) L'ip du switch sera 10.10.10.1 il faudra donc dans le switch entrer les commandes suivantes:

    Orleans> enable
    Orleans# conf t
    Orleans(config)# interface vlan 230
    Orleans(config-if)# description Management
    Orleans(config-if)# ip address 10.10.10.1 255.255.255.0
    Orleans(config-if)# exit
    Orleans(config)# Interface GigabitEthernet1/0/1
    Orleans(config-if)# switchport mode access
    Orleans(config-if)# Switchport access vlan 230
    Orleans(config-if)# exit
    Orleans(config)# exit
    Orleans> write

Nous pouvons maintenant configurer le SSH :

    Orleans> enable
    Orleans# conf t
    Orleans(config)# ip domain-name orleans.sportludique.fr
    Orleans(config)# crypto key generate rsa 2048
    Orleans(config)# ip ssh version 2
    Orleans(config)# ip ssh time-out 60
    Orleans(config)# ip ssh authentication-retries 2
    Orleans(config)# enable secret password <mot de passe>
    Orleans(config)# exit
    Orleans> write mem
<span class="underline">
#### Création des VLANS
</span>

    Orleans> enable
    Orleans# conf t
    Orleans(config)# interface vlan231
    Orleans(config-if)# description DMZ
    Orleans(config-if)# no ip address
    Orleans(config-if)#exit
    Orleans(config)# interface vlan232
    Orleans(config-if)# description Serveur
    Orleans(config-if)# ip address 172.28.32.254 255.255.255.0
    Orleans(config-if)#exit
    Orleans(config)# interface vlan235
    Orleans(config-if)# description R&D
    Orleans(config-if)# ip address 172.28.35.254 255.255.255.0
    Orleans(config-if)#exit
    Orleans(config)# interface vlan236
    Orleans(config-if)# description WiFiVisiteurs
    Orleans(config-if)# ip address 172.28.36.254 255.255.255.0
    Orleans(config-if)#exit
    Orleans(config)# interface vlan237
    Orleans(config-if)# description WiFiEmployes
    Orleans(config-if)# ip address 172.28.37.254 255.255.255.0
    Orleans(config-if)#exit
    Orleans(config)# interface vlan238
    Orleans(config-if)# description Intercom_coeur_SN210
    Orleans(config-if)# ip address 192.168.20.1 255.255.255.0
    Orleans(config-if)#exit
    Orleans(config)# interface vlan239
    Orleans(config-if)# description Intercom
    Orleans(config-if)# no ip address
    Orleans(config-if)#exit
    Orleans(config)# exit
    Orleans> write mem

#### Ajout de la passerelle par défault

    Orleans> enable
    Orleans# conf t
    Orleans(config)# ip default-gateway 192.168.20.254
    Orleans(config)# ip classeless
    Orleans(config)# ip route 0.0.0.0 192.168.20.254
    Orleans(config)# exit
    Orleans> write mem




    




