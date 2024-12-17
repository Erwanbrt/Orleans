# Serveur VPN OpenVPN


## Installation basique OpenVPN. `

        sudo apt-get install openvpn
        sudo apt-get install easy-rsa
        make-cadir openvpn-ca && cd  ~/openvpn-ca
        sudo nano vars 


## Modification du fichier vars en ajoutant ces lignes :

        set_var EASYRSA_REQ_COUNTRY     "Fr"
        set_var EASYRSA_REQ_PROVINCE    "Idf"
        set_var EASYRSA_REQ_CITY        "Orleans"
        set_var EASYRSA_REQ_ORG         "orleans.sportludique.fr"
        set_var EASYRSA_REQ_EMAIL       "orleans.orleans.sportludique.fr"
        set_var EASYRSA_REQ_OU          "labo"
        set_var EASYRSA_ALGO            rsa 
        set_var EASYRSA_CURVE           secp384r1
        set_var EASYRSA_DIGEST          "sha256"
        set_var EASYRSA_CA_EXPIRE       3650
        set_var EASYRSA_CERT_EXPIRE     825
        set_var EASYRSA_CRL_DAYS        180

#### nous avons enregistré puis entré la commande afin de créer le fichier PKI :

        ./easyrsa init-pki

#### Nous allons ensuite générer le fichier ca.crt :

        ./easyrsa build-ca nopass

#### Nous allons également générer les fichiers server.key et server.crt :

        ./easyrsa build-server-full server nopass

#### Génération du fichier CRL 

        ./easyrsa gen-crl

#### Génération du fichier ta.key :

        sudo openvpn --genkey secret ta.key
        Déplacement des fichier :
        sudo cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn
        sudo cp /openvpn-ca/pki/issued/server.crt /etc/openvpn/server/
        sudo cp /openvpn-ca/pki/private/server.key /etc/openvpn/server/
        sudo cp /openvpn-ca/pki/ca.crt /etc/openvpn/server/
        sudo cp /openvpn-ca/ta.key /etc/openvpn/server/
        sudo cp /openvpn-ca/pki/crl.pem /etc/openvpn/server/

#### Modification du fichier server.conf :

    local 192.168.34.15 
    port 1194 
    ;proto tcp 
    proto udp 
    ;dev tap 
    dev tun  
    ;dev-node MyTap 
    ca server/ca.crt 
    cert server/server.crt 
    key server/server.key  # This file should be kept secret 
    dh none 
    topology subnet 
    server 10.8.0.0 255.255.255.0 
    ifconfig-pool-persist /var/log/openvpn/ipp.txt   
    ;server-bridge 10.8.0.4 255.255.255.0 10.8.0.50 10.8.0.100 
    ;server-bridge 
    ;push "route 192.168.10.0 255.255.255.0" 
    ;push "route 192.168.20.0 255.255.255.0" 
    push "route 121.183.90.200 255.255.255.248" 
    push "route 183.44.0.0 255.255.0.0" 
    push "route 221.87.0.0 255.255.0.0" 
    push "route 10.10.10.0 255.255.255.0" 
    ;client-config-dir ccd 
    ;route 192.168.40.128 255.255.255.248 
    ;client-config-dir ccd 
    ;route 10.9.0.0 255.255.255.252 
    ;learn-address ./script 
    push "redirect-gateway def1 bypass-dhcp" 
    ;push "dhcp-option DNS 208.67.222.222" 
    ;push "dhcp-option DNS 208.67.220.220" 
    push "dhcp-option DNS 121.183.90.205" 
    ;client-to-client 
    ;duplicate-cn 
    keepalive 10 120 
    tls-auth server/ta.key 0 # This file is secret 
    cipher AES-256-GCM 
    auth SHA256
    ;compress lz4-v2 
    ;push "compress lz4-v2"  
    ;comp-lzo 
    ;max-clients 100 
    user root 
    group root 
    persist-key 
    persist-tun 
    status /var/log/openvpn/openvpn-status.log 
    ;log /var/log/openvpn/openvpn.log 
    ;mute 20 
    explicit-exit-notify 1 
    mode server 
    crl-verify server/crl.pem 
    remote-cert-tls client 
    tls-version-min 1.2 

#### Redémmarage du service et création des client avec les commandes :
        
        cd ~/openvpn-ca
        ./easyrsa build-client-full elouenn nopass

#### Récuperation des fichiers clients grace à FileZilla :

### Création du fichier OVPN : 

    # Nous somme client et l'on peut recupérer des éléments poussé par le serveur 
    client
    # Mettre comme sur le fichier server.conf 
    dev tun
    # Protocol pour communiquer avec le serveur
    proto udp
    # Liste de vos serveur VPN et port (Mettre le domaine ou l'IP publique) 
    remote 183.44.45.1 1194
    # Si plusieurs serveur sont défini on en choisi 1 au hasard.
    #remote-random
    # Aucune idée de ce que ça fait
    resolv-retry infinite
    # N'utilise pas de port spécifique pour repondre
    nobind
    # On garde des état à travers les restart
    persist-key
    persist-tun
    # On rajoute un controle pour s'assurer que le serveur de destination
    # possède bien un certificat de type serveur
    remote-cert-tls server
    # On configure les même cipher que le serveur
    cipher AES-256-GCM
    auth SHA256
    # Le niveau de log
    verb 3
    # Pour une configuration inline des certificats
    key-direction 1
    ca ca.crt
    cert elouenn.crt
    key elouenn.key
    tls-auth ta.key 1

#### Tentative de connexion 

Pour la premiére tentative nous avons essayé de nous connecter en local et au debut nous avions cette erreur : 

     2024-12-17 11:06:26 us=114951 Authenticate/Decrypt packet error: packet HMAC authentication failed
     2024-12-17 11:06:26 us=115088 TLS Error: incoming packet authentication failed from [AF_INET]10.10.10.17:39727 erreur possible

Nous avons donc modifié le fichier client.OVPN pour le mettre comme il est ci-dessus.


Puis nous avons retesté la connexion en local avec succès.

nous pouvons donc testé depuis l'infrastructure de Tours succes mais sans possibilité de joindre les équipements liés au réseau local management.

Solution : Créer des règles de NAT sur le serveur 

Nous avons donc accès au réseaux management de notre entreprise depuis le réseau local de tours.


#### Étape suivant mettre le VPN accessible depuis le vrai WEB 


