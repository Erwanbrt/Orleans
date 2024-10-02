# HSRP

Permet d'asurer la redondance des passerelles en créant une adresse IP virtuelle partagée dans notre cas 192.168.10.254. 

Un routeur actif gère le trafic, tandis qu’un routeur en veille prend leCoeur de réseau relais en cas de panne. Les routeurs échangent des paquets pour surveiller leur état, garantissant ainsi une haute disponibilité du réseau.

Dans ce cas avec le routeur Fibre et celui de L'ADSL nous devons les configurer les interfaces définir une priorité qui sera plus élévés pour l'un dans ce cas celui de l'ADSL (priorité de 110).

Grace à cette configuration le routeur ADSL sera le routeur actif tant qu'il est disponible. En cas de panne du routeur ADSL ou de sa connexion, le routeur de la fibre prendra automatiquement le relais en tant que routeur actif. Coeur de réseau


