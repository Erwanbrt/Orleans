# SSL

Nous allons configurer et activer via notre serveur Apache le protocole de chiffrement SSL/TLS afin que les requetes circulent de façon confidentielle sur internet. Lorsque la communication est effectuée via cette couche de transport chiffrée, un « s » est ajouté au nom du protocole : : http://www.orleans.sportludique.fr  devient https://www.orleans.sportludique.fr . 

Ce certificat est nécessaire pour transférer la clé publique du serveur ainsi que son identité afin d'initier une communication via TLS.  

Vérification de l’installation d’OpenSSL 

OpenSSL est un ensemble de bibliothèques et d'outils comprenant tout ce qui est nécessaire à l'utilisation de la cryptographie forte et des protocoles SSL (Secure Sockets Layer), TLS (Transport Layer Security). 

Dans ce cas nous sommes notre propre autorité de certification. 

openssl version METTRE DANS CADRE  

Configuration openSSL 

Il est important de configurer le fichier /etc/ssl/openssl.cnf  

METTRE PHOTO DU FICHIER CONF  

Organisation 

Pour plus de compréhension j’ai installé « tree » 

METTRE PHOTO DU TREE 

Génération de la clé privée du serveur web 

openssl genrsa 2048 > siteweb/keys/privatekey.key METTRE DANS CADRE  

commende pour vérifier le contenu : 

openssl ec -in privatekey.key -text -noout 

Génération d'une demande de certificat pour le serveur web. 

openssl req -new -key siteweb/keys/privatekey.key > siteweb/requests_certificats/demande.csr -config /etc/ssl/openssl.cnf 

rappeler les champs a mettre avec toutes la liste 

Si on veut vérifier notre demande on peut faire la commande :  

openssl req -in siteweb/requests_certificats/demande.csr -noout -text 

Création du certificat de l'autorité de certification 

  =>  la création de la clé privée :  

openssl genrsa -des3 2048 > [a vous de préciser le chemin, soyez cohérent SVP] private_ca.key 

une pass phrase sera demandé  

Création du certificat pour la durée  

openssl req -new -x509 -days 365 -key private_ca.key > autorite/certificats/ca.crt 

rappeler les champs a mettre avec toutes la liste 

Signer la demande de certificat  

openssl x509 -req -in [te_plante_pas].csr -out [a_adapter].crt -CA [certif_autorite].crt -CAkey [privatekey_ca].key -CAcreateserial -CAserial ca.srl -extfile /etc/ssl/openssl.cnf -extensions v3_req 

Vérifier son contenu  

openssl x509 -in siteweb/certificats/siteweb.crt -noout -text 

METTRE PHOTO  

Configuration du serveur web  

 

Copier le fichier /etc/apache2/sites-available/default-ssl.conf dans son dossier d'origine mais avec le nom de votre site web donc www.orleans .conf 

(en conservant l'extension .conf pour la commande a2ensite) 

METTRE PHOTO www.orleans.confpart1-2 

Adapter : DocumentRoot et ServerName, SSLCertificateFile et SSLCertificatekeyfile, rediriger le port 443. 

Activer le module SSL, activer le site, et redémarrer le service comme demandé : 

a2enmod ssl 

Accéder au site et accepter le certificat et afficher ses informations 

 

Rajouter certificat dans le navigateur  