# transformation d'un poste en routeur

Avoir une connexion wifi ( ou un partage de connexion) 

net.ipv4.ip_forward = 1 

 

sudo iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE 


Et voilà le poste est devenu un routeur (il faut attendre un peu avant que la connexion arrive) 

