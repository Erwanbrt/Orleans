# Reverse Proxy
1. Créer un fichier de configuration pour le site   

Navigue vers le répertoire `sites-available` de Nginx et crée un nouveau fichier de configuration : 

sudo nano /etc/nginx/sites-available/site-orleans-sportludique 

Il faudra faire pareil pour l’autre site mais avec un autre intitulé  

2. Configuration du Reverse Proxy

   Dans ce fichier, ajoute la configuration suivante pour configurer le reverse proxy du site 1 et du site 2 avec une différence pour chacun que j’ai indiqué ci dessous : 

   server { 

       listen 80;  

        index index.html index.htm index.nginx-debian.html; POUR LE SITE2 

       server_name www.orleans.sportludique.fr; 

        return 301 https://$host$request_uri; 

} 

server { 

        listen 443 ssl; 

        server_name www.orleans.sportludique.fr; 

 

        ssl_certificate /etc/ssl/certificats-sitesweb/certificat_siteweb.crt; 

        ssl_certificate_key /etc/ssl/certificats-sitesweb/privatekey.key; 

 

        location / { 

        #proxy_pass http://192.168.45.9; 

        proxy_pass http://backend; 

        include /etc/nginx/proxy_params;  

        proxy_set_header Host $host; 

        proxy_set_header X-Real-IP $remote_addr; 

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 

        proxy_set_header X-Forwarded-Proto $scheme; 

        } 

} 

3. Activer le site  

Crée un lien symbolique dans le répertoire `sites-enabled` pour activer la configuration: 

sudo ln -s /etc/nginx/sites-available/site-orleans-sportludique /etc/nginx/sites-enabled/nomduliensymbolique 

4. Tester la configuration

S’assurer que la configuration est correcte : 

  sudo nginx -t 

5. Redémarrer Nginx 

Si la configuration est correcte, redémarre Nginx pour appliquer les changements : 

 sudo systemctl restart nginx 

6. Vérifier le fonctionnement du Reverse Proxy

Vérifier en accédant à l'IP de ton serveur Nginx depuis un navigateur. On devrait être redirigé vers le serveur configuré dans le `proxy_pass`.

