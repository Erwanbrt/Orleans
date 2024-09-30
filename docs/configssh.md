# Configuration SSH 

connexion ssh : ssh adminorl@10.10.10.10 et c’est en /24 

Doc : it-connect  

Nous avons eu problème : pas de mdp enable lors de la connexion ssh  

Nous sommes mis en mode console, en mode conf nous avons taper la commande : enable secret password <azerty> afin de pouvoir chiffrer le mdp lors de notre connexion  