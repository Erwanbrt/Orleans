
# Configuration HTTPS avec un certificat auto-signé
## 1. Installation d'OpenSSL

    sudo apt install openssl -y

## 2. Génération d'un certificat SSL auto-signé

    mkdir -p /opt/vaultwarden/ssl
    cd /opt/vaultwarden/ssl
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout vaultwarden.key -out vaultwarden.crt

## 3. Modification de docker-compose.yml pour activer HTTPS
Ajouter sous environment :

     - ROCKET_TLS={certs="/ssl/vaultwarden.crt",key="/ssl/vaultwarden.key"}

Modifier la section volumes :
   
      - ./data:/data
      - ./ssl:/ssl

## 4. Redémarrer Vaultwarden
    sudo docker-compose down
    sudo docker-compose up -d


## 5. Accès et configuration

HTTP : http://192.168.10.31:8080 

-> HTTPS : https://192.168.10.31:8080 

(avec avertissement SSL)








