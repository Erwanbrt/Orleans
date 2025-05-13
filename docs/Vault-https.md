
# Configuration HTTPS avec un certificat auto-signé
## 1. Installation d'OpenSSL

    sudo apt install openssl -y

## 2. Génération d'un certificat SSL auto-signé

    mkdir -p /opt/vaultwarden/ssl
    cd /opt/vaultwarden/ssl

> Stockage du certificat et de la clé privée

    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout vaultwarden.key -out vaultwarden.crt

> Rentrer les infos demandées

## 3. Modification de docker-compose.yml pour activer HTTPS

Dans le fichier `/opt/vaultwarden/docker-compose.yml` :

> Ajouter dans la section environment :
     
     
     - ROCKET_TLS={certs="/ssl/vaultwarden.crt",key="/ssl/vaultwarden.key"}

> Modifier la section volumes :
    
    
    volumes :
      - ./data:/data
      - ./ssl:/ssl

## 4. Redémarrer Vaultwarden
    sudo docker-compose down
    sudo docker-compose up -d


## 5. Accès et configuration

### 5.1 Accès a Vaultwarden

>HTTP : http://192.168.45.16:8080

>HTTPS : https://192.168.45.16:8080 (avec avertissement SSL) pour le wifi employé 

>HTTP : http://10.10.10.27:8080

>HTTP : http://10.10.10.27:8080 en management

(avec avertissement SSL)

### 5.2 Accès à l'interface d'administration

> URL : https://10.10.10.27:8080/admin

> Se connecter avec ADMIN_TOKEN donc”xxxx”










