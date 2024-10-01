# Windows Serveur

Utilisation de Nutanix pour créer la VM Windows Server pour l'AD afin qu'il y est un controleur de domaine. <br>

Nous avons ensuite mit serveur DNS primaire, DHCP.<br>

Créer le domaine en orleans.lan.<br>

Et création des 3 sessions administrateur 


## Mise en place du serveur DHCP pour les différents VLANS.
    
Afin de pouvoir obtenir une addresse automatiquement sur nos VLANS.<br>
    - R&D (235)<br>
    - WiFi Visiteurs (236)<br>
    - WiFi Employés (237)<br>

Pour cela nous avons créer notre serveur DHCP sur notre Windows Serveur. 

   Dans le gestionnaire de Serveur: 
      
    - Outils.
    - DHCP.
    - On selectionne notre domaine puis IPv4.
    - Dans le dossier IPv4 on clique sur "Action" puis "nouvelle étendue"
    - Il suffit ensuite de suivre assistant de création d'étendue.
        - On la nomme.
        - On définit l'addresse de début, de fin et le masque.
        - On peut égallement effectuer une exclusion pour une plage d'IP réserver.
        - On définit la durée du bail (Temps d'attribution d'une adresse pour un equipement).
        - Et enfin on valide les paramétres.
    