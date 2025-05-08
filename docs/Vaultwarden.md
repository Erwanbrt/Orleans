# Vaultwarden gestionnaire de mot de passe
Documentation d'installation de Bitwarden (Vaultwarden) sur Debian
Vaultwarden est une version allégée de Bitwarden et auto-hébergée. 


# 1. Prérequis

Un serveur Debian<br>Mettre à jour le système : sudo apt update && sudo apt upgrade -y


# 2. Installation de Docker et Docker Compose
## 2.1 Installation de Docker
Ajout des dépôts officiels et installation :
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

Activation et vérification de Docker :
sudo systemctl enable --now docker
docker --version

## 2.2 Installation de Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/

sudo docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo docker-compose --version


# 3. Installation et configuration de Vaultwarden
# 3.1 Création du dossier Vaultwarden
mkdir -p /opt/vaultwarden && cd /opt/vaultwarden

## 3.2 Création du fichier docker-compose.yml
Créer et éditer le fichier docker-compose.yml :
nano docker-compose.yml

    Ajouter le contenu suivant :
        version: "3"
        services:
            vaultwarden:
            image: vaultwarden/server:latest
            container_name: vaultwarden
            restart: unless-stopped
        ports:
            - "8080:80"
            - "3012:3012"
        volumes:
            - ./data:/data
        environment:
            - WEBSOCKET_ENABLED=true
            - SIGNUPS_ALLOWED=true #ce qui permet l’activation pour créer un compte si on veut éviter que des utilisateurs se créer un compte changer par “false”
            - ADMIN_TOKEN=”xxxxxxx”

Si on veut changer le mot de passe il suffit de le changer dans docker.compose.yml

Une fois la modification effectuée, enregistrer et fermer le fichier (CTRL + X, puis Y, puis Entrée).
Puis, redémarrer Vaultwarden pour appliquer la modification avec les commandes suivantes : 
sudo docker-compose up -d
pour vérifier que le conteneur tourne :
sudo docker ps


5. Configuration HTTPS avec un certificat auto-signé
5.1 Installation d'OpenSSL
sudo apt install openssl -y

5.2 Génération d'un certificat SSL auto-signé
mkdir -p /opt/vaultwarden/ssl
cd /opt/vaultwarden/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout vaultwarden.key -out vaultwarden.crt

5.3 Modification de docker-compose.yml pour activer HTTPS
Ajouter sous environment :
     - ROCKET_TLS={certs="/ssl/vaultwarden.crt",key="/ssl/vaultwarden.key"}

Modifier la section volumes :
   volumes:
      - ./data:/data
      - ./ssl:/ssl

5.4 Redémarrer Vaultwarden
docker-compose down
docker-compose up -d


6. Accès et configuration
Accès à Vaultwarden


HTTP : http://192.168.10.31:8080
HTTPS : https://192.168.10.31:8080 (avec avertissement SSL)
Première inscription


La première inscription créera un compte administrateur.
Accès à l'interface d'administration


URL : https://192.168.10.31:8080/admin
Se connecter avec ADMIN_TOKEN donc”xxxx”

7. Créer compte utilisateur 
On peut gérer les utilisateurs de cette interface : 
Supprimer l’utilisateur à accéder   
Retirer le droit à l’utilisateur 
Déconnecter l’utilisateur 

Pour créer un compte il y a trois possibilités : 
soit dans l’interface admin : cliquer sur Vault en haut dans le menu ce qui va nous amener sur la page web et créer un compte
soit directement dans l’interface admin avec “enter mail”

soit via l’URL : https://192.168.10.31:8080/#/login


On peut soit choisir de créer un compte (qui sera par la suite ajouter à l’interface en admin vu au-dessus) ou alors se connecter directement si un mot de passe à déjà été défini. 
 
