# Serveur DHCP

#### Mise en place du serveur DHCP pour les différents VLANS 


<table>
    <thead>
        <tr>
            <th>Nom</th>
            <th>VLAN</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>R&D</th>
            <th>235</th>
        </tr>
        <tr>
            <th>WiFi visiteurs</th>
            <th>236</th>
        </tr>
        <tr>
            <th>WiFi employées</th>
            <th>237</th>
        </tr>
    </body>
</table>


Pour cela nous avons créer le serveur DHCP sur notre Windows Serveur.

### Dans le gestionnaire de Serveur

      - Outils
      - DHCP
      - Selection du dommaine concerné ainsi que le type d'IP
      - Cliquer sur "action" puis "nouvelle etendue"

  * L'assistant de Nouvelle étendue s'ouvre. 

### Sur l'assistant : 

      - Nommer l'étendue
      - Ajouter les addresse IP de début et de fin
      - (il est possible d'effectuer une exculusion pour une plage d'IP)
      - On definit la durée du bail
      - et enfin on valide les paramétres

