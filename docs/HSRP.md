# HSRP

### Définition : 

<b>(Hot Standby Router Protocol)</b> est un protocole de redondance développé par <b>CISCO.</b> 

Il permet d’assurer une haute disponibilité des passerelles par défaut (ou routeurs) dans un réseau local. Il est conçu pour éviter un point de défaillance unique dans le réseau en offrant une solution de basculement automatique lorsque la passerelle principale devient indisponible. 

Il permet à plusieurs routeurs ou passerelles de collaborer afin de présenter une seule passerelle virtuelle aux hôtes du réseau. Cela garantit que si le routeur actif (qui achemine réellement le trafic) échoue, un routeur de secours prend automatiquement le relais sans que les hôtes ne s’en aperçoivent.

Dans notre cas nous avons définit en <b>routeur</b> prioritaire la <b>FIBRE</b> et la passerelle par défaut sera <b>192.168.10.254</b>

## Configuration du <b>HSRP</b>

dans les deux routeurs la configuration est diffférente, nous allons alors commentcer par le routeur <b>FIBRE</b> (Primaire) grace au commandes suivantes :

        ****
        ****

Configuration dans le routeur <b>ADSL</B> (Secondaire) :

        *****
        *****

