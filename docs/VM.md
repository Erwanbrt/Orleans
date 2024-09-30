# VM
Nous avons besoins de deux VM


# vm windows serv 
Utilisation de Nutanix pour créer la VM Windows Server pour l'AD afin qu'il y est un controleur de domaine. <br>

Ensuite mit serveur DNS primaire, DHCP<br>

Creation d'un domaine en orleans.lan<br>

Creation des 3 sessions administrateur 


* la partie DCHP de erwan 



# vm debian 

Utilisation de Nutanix pour créer la VM Ddebian 12 afin de

creer les users sudo adduser "nom-user"<br>

- commande config DNS : /etc/resolv.conf 

- commande config du depot :  sudo nano /etc/apt/sources.list

donc on avait erreur pour faire apt update donc modif à l'aide de ce depot : 
`deb http://deb.debian.org/debian/ bookworm main non-free-firmware contrib non-free`

- configuration ssh : sudo apt install openssh-server







