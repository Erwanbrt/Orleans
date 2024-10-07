# DNS Redirrecteur

### Activer le service DNS redirrecteur 

#### Dans le Gestionnaire de serveur

        Dans le menu "Outils" selectionner "DNS"
        Selectionner le dossier "Redirecteurs" puis "Propriétés"
        Clique sur "Modifier" 
        Puis dans la fenétre qui s'ouvre entrer l'addresse IP de votre redirrecteur

        Dans le menu "Intergfaces"
        Selectionner l'IP du serveur configurer précédement
        Puis valider 

Le DNS redirrecteur va permettre de communique avec le DNS qui a la propriété sur le domaine SportLudique.fr
Il permettra de resoudre les noms de domaines

### Dans les réglages réseau du Serveur 

        Dans le "Centre Réseau et Partage"
        Selectionner "Propriétés"
        Puis "IPv4"
        Dans le paramétre Serveur DNS Préféré entrer l'adresse IP du serveur DNS local du serveur

### nslookup

Le <strong>nslookup</strong> il permet de voir la resolution <strong>DNS</strong> 