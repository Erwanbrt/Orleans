# Installation et configuration de Nginx sur Debian/Ubuntu
     
## 1. Mise à jour du système

    sudo apt update && sudo apt upgrade -y


## 2. Installation de Nginx

Installez Nginx avec la commande suivante :

```bash
sudo apt install nginx 
```
## 3.  Vérification du service Nginx

Vérifiez si le service Nginx s’est bien lancé après l’installation :

```bash
sudo systemctl status nginx
```

Si nécessaire, démarrez-le manuellement :


    sudo systemctl start nginx


## 4. Vérification depuis un navigateur

Accédez à l’adresse IP `http://192.168.45.14` dans le navigateur.

Vous devriez voir la page par défaut de Nginx : **"Welcome to nginx!"**

---

## 5. Commandes utiles pour gérer Nginx

| Action                     | Commande                       |
| -------------------------- | ------------------------------ |
| Redémarrer Nginx           | `sudo systemctl restart nginx` |
| Recharger la configuration | `sudo systemctl reload nginx`  |
| Activer Nginx au démarrage | `sudo systemctl enable nginx`  |
| Désactiver au démarrage    | `sudo systemctl disable nginx` |

---


## 6. Arborescence des fichiers Nginx

* Fichier principal de config : `/etc/nginx/nginx.conf`
* Répertoires de configuration des sites :

    * Sites disponibles : `/etc/nginx/sites-available/`
    * Sites activés : `/etc/nginx/sites-enabled/`

* Répertoire racine du site par défaut : `/var/www/html`

---

## 7. Tester la configuration

Avant de redémarrer Nginx après des modifications 

>Tester la configuration :

```bash
sudo nginx -t
```

Si la commande retourne "syntax is ok" et "test is successful", 

> Pour recharger la configuration :

```bash
sudo systemctl reload nginx
```
