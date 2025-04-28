# VLAN 

Afin de segmenter et différencier les différents pôles de notre organisation au sein de notre réseau, nous avons adopté la technologie des **VLANs** (Virtual Local Area Networks). Cette approche nous permet d’isoler les différents sous-réseaux de manière logique, facilitant ainsi la gestion du trafic, l’amélioration de la sécurité, et l’optimisation des performances réseau en attribuant des ressources dédiées à chaque pôle.

Notre plage d’adresses IP est **172.28.32.0/19**, ce qui nous offre une capacité d’adressage suffisante pour couvrir les besoins de chaque segment de réseau. Afin d’assurer une gestion efficace de ces adresses et de maintenir une vue d’ensemble actualisée de notre infrastructure, nous avons mis en place un système de **IPAM** (IP Address Management). Cet outil nous permet de référencer l’ensemble des adresses IP, d’attribuer les sous-réseaux à chaque VLAN de manière structurée, et de garantir une mise à jour continue de la configuration réseau, notamment en cas de modifications ou d’expansions. L’IPAM joue un rôle crucial dans l’optimisation de l’allocation des adresses et dans la prévention des conflits d’adressage.

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
            <th>Serveurs</th>
            <th>232</th>
            <th>172.28.32.0</th>
            <th>/24</th>
        </tr>
            <th>LIBRE</th>
            <th>233</th>
            <th>192.168.33.0</th>
            <th>/24
        </tr>
            <th>R&D</th>
            <th>235</th>
            <th>172.28.35.0</th>
            <th>/24</th>
        </tr>
            <th>WiFI visiteurs</th>
            <th>236</th>
            <th>192.168.28.0</th>
            <th>/24</th>
        </tr>
            <th>LIBRE</th>
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
    

