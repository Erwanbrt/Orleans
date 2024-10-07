# HSRP

### Définition : 

<b>(Hot Standby Router Protocol)</b> est un protocole de redondance développé par <b>CISCO.</b> 

Il permet d’assurer une haute disponibilité des passerelles par défaut (ou routeurs) dans un réseau local. Il est conçu pour éviter un point de défaillance unique dans le réseau en offrant une solution de basculement automatique lorsque la passerelle principale devient indisponible. 

Il permet à plusieurs routeurs ou passerelles de collaborer afin de présenter une seule passerelle virtuelle aux hôtes du réseau. Cela garantit que si le routeur actif (qui achemine réellement le trafic) échoue, un routeur de secours prend automatiquement le relais sans que les hôtes ne s’en aperçoivent.

Dans notre cas nous avons définit en <b>routeur</b> prioritaire la <b>FIBRE</b> et la passerelle par défaut sera <b>192.168.10.254</b>

## Configuration du <b>HSRP</b>

dans les deux routeurs la configuration est diffférente, nous allons alors commencer par le routeur <b>FIBRE</b> (Primaire) grace au commandes suivantes :

        RTR-ORLEANS-FIBRE>enable
        RTR-ORLEANS-FIBRE#conf t
        RTR-ORLEANS-FIBRE#(config)interface GigabitEthernet 0/1.239
        RTR-ORLEANS-FIBRE#(config-if) standby 1 ip 192.168.10.254
        RTR-ORLEANS-FIBRE#(config-if) standby 1 priority 110
        RTR-ORLEANS-FIBRE#(config-if) standby 1 preempt
        RTR-ORLEANS-FIBRE#(config-if) exit
        RTR-ORLEANS-FIBRE#(config) exit
        RTR-ORLEANS-FIBRE# write mem

Configuration dans le routeur <b>ADSL</B> (Secondaire) :

        RTR-ORLEANS-ADSL>enable
        RTR-ORLEANS-ADSL#conf t
        RTR-ORLEANS-ADSL#(config)interface GigabiEethernet 0/1.239
        RTR-ORLEANS-ADSL#(config-if)standby 1 ip 192.168.10.254
        RTR-ORLEANS-ADSL#(config-if)standby 1 preempt
        RTR-ORLEANS-ADSL#(config-if)exit
        RTR-ORLEANS-ADSL#(config)exit
        RTR-ORLEANS-ADSL#write mem



