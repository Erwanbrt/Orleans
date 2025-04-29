# Serveur Apache

Afin d’avoir accès à notre site web nous avons d’eu le mettre sur un serveur apache. Son rôle est d'écouter les requêtes émises par les navigateurs (qui demandent des pages web), de chercher la page demandée et de la renvoyer.  

Forme 

1. Installation d'Apache2 

Installez Apache2 : 

sudo apt install apache2  

Vérifiez le statut du service Apache : 

sudo systemctl status apache2 

Testez l’installation : Rendez-vous sur l’adresse IP de votre VM dans un navigateur pour voir la page de test Apache par défaut (exemple : http://192.168.45.10). 

Nous devrions obtenir cette page : image 

Forme 

2. Configuration du site web 

Créer un répertoire pour le site web : 

Se placer dans le fichier /var/www/html/index.html  

Définir les permissions : 

sudo chown -R www-data:www-data /var/www/orleans.sportludique.fr 

sudo chmod -R 755 /var/www/orleans.sportludique.fr 

Créer un fichier de configuration Apache pour le site : 

sudo nano /etc/apache2/sites-available/www.orleans.conf 

Contenu du fichier : 

Les deux images 

Activer le site et recharger Apache : 

sudo a2ensite orleans.sportludique.fr 