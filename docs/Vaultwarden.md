# Installation de Docker et Docker Compose pour Vaultwarden
 


Un serveur Debian<br>Mettre à jour le système : sudo apt update && sudo apt upgrade -y


## 1. Installation de Docker
Ajout des dépôts officiels et installation :

    sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    sudo apt update

    sudo apt install -y docker-ce docker-ce-cli containerd.io

Activation et vérification de Docker :

    sudo systemctl enable --now docker docker --version

## 2. Installation de Docker Compose
    sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/

    sudo docker-compose

    sudo chmod +x /usr/local/bin/docker-compose

    sudo docker-compose --version


# 3. Installation et configuration de Vaultwarden

## 3.1 Création du dossier Vaultwarden
    mkdir -p /opt/vaultwarden && cd /opt/vaultwarden

## 3.2 Création du fichier docker-compose.yml
Créer et éditer le fichier docker-compose.yml :

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

Si on veut changer le mot de passe il suffit de le changer dans <b>docker.compose.yml</b>

-> Redémarrer Vaultwarden pour appliquer la modification avec les commandes suivantes : 

    sudo docker-compose up -d

-> pour vérifier que le conteneur tourne :

    sudo docker ps


## 3.3 Créer compte utilisateur 
On peut gérer les utilisateurs via : https://192.168.45.16:8080/admin



Pour créer un compte il y a trois possibilités : 

- Interface admin : cliquer sur Vault en haut dans le menu ce qui va nous amener sur la page web et créer un compte
- Interface admin avec “enter mail”
- URL : https://192.168.45.16:8080/#/login


On peut soit choisir de créer un compte (qui sera par la suite ajouter à l’interface en admin vu au-dessus) ou alors se connecter directement si un mot de passe à déjà été défini. 
 


![ ](images/schémavault.png)

