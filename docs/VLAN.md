# VLAN 

1. VLAN Management: 230

En premier temps nous avons stacker les deux switchs pour avoir plus d’interfaces, ensuite nous avons diviser le réseau en /25 en deux afin configurer le vlan de management 230 sur l’interface gigabit 1/0/1 avec l’ip 172.28.63.129/25 Nous avons créé une connexion ssh pour pouvoir se connecter au switch à distance à l’aide de notre user adminorl et l’adresse ip que nous lui avons attribué.  

<table>
    <thead>
        <tr>
            <th>Nom</th> 
            <th>Vlan</th>
            <th>@ résau</th>
            <th>Masque</th>
        <tr>
            <th>Management</th>
            <th>230</th>
            <th>10.10.10.0</th>
            <th>/24</th>
        </tr>
            <th>DMZ Publique</th>
            <th>231</th>
            <th>192.168.45.1</th>
            <th>/24</th>
        </tr>
            <th>Serveur</th>
            <th>232</th>
            <th>172.28.32.0</th>
            <th>/24</th>
        </tr>
            <th>DMZ Privée</th>
            <th>233</th>
            <th>192.168.145.0</th>
            <th>/24
        </tr>
            <th>R&D</th>
            <th>235</th>
            <th>172.28.35.0</th>
            <th>/24</th>
        </tr>
            <th>WiFI visiteurs</th>
            <th>236</th>
            <th>172.28.36.0</th>
            <th>/24</th>
        </tr>
            <th>WiFi employées</th>
            <th>237</th>
            <th>172.28.37.0</th>
            <th>/24</th>
        </tr>
            <th>Interco SN210 Coeur</th>
            <th>238</th>
            <th>192.168.20.0</th>
            <th>/24</th>
        </tr>
            <th>Interco Routeurs SN310</th>
            <th>239</th>
            <th>192.168.10.0/24</th>
            <th>/24</th>
        </tr>
    </body>
    </body>
</table>
    

