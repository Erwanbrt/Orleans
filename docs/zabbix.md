# 📡 Installation et Configuration de Zabbix pour la Supervision VPN & SNMP

## 🔧 1. Installation de Zabbix sur Debian 12

### 🗄️ Installation de MariaDB

```bash
sudo apt install mariadb-server mariadb-client -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation
```

![Installation MariaDB](URL_IMAGE_INSTALL_MARIADB)

---

### 📦 Dépôt Zabbix

```bash
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.2+debian12_all.deb
sudo apt update
```

![Dépôt Zabbix](URL_IMAGE_DEPOT_ZABBIX)

---

### 🚀 Installation de Zabbix et dépendances

```bash
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

---

### 🗃️ Configuration de la base de données

```bash
sudo mysql -u root -p
```

```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'VotreMotDePasseZabbix';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
SET GLOBAL log_bin_trust_function_creators = 1;
EXIT;
```

```bash
sudo zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

![Configuration DB](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix2.png)

---

### ▶️ Lancer les services

```bash
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```

![Zabbix Web Interface](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix3.png)

---

## 🖧 2. Configuration des Agents SNMP

- SNMP permet de superviser des équipements réseau (routeurs, switches, etc.)
- Pour SNMPv1 ou v2c, il suffit d’indiquer la communauté (exemple : `zabbix`)

![Configuration SNMP](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix4.png)

---

## ⚙️ 3. Problème avec `fping` ?

```bash
sudo apt-get install fping
sudo rm -f /usr/sbin/fping6
sudo ln -s /usr/bin/fping /usr/sbin/fping
sudo systemctl restart zabbix-server
```

---

## 🔐 4. Supervision des Connexions VPN

### 📜 Script de comptage des clients OpenVPN

```bash
sudo nano /usr/local/bin/get_openvpn_clients.sh
```

```bash
#!/bin/bash

STATUS_FILE="/var/log/openvpn/openvpn-status.log"

if [ ! -f "$STATUS_FILE" ]; then
  echo "0"
  exit 1
fi

grep -E "10.8.0" "$STATUS_FILE" | wc -l
```

```bash
sudo chmod +x /usr/local/bin/get_openvpn_clients.sh
```

Modifier l’agent Zabbix :

```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
```

Ajouter :

```
UserParameter=vpn.connections,/usr/local/bin/get_openvpn_clients.sh
```

```bash
sudo systemctl restart zabbix-agent
```

![Item VPN Zabbix](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix4.png)

---

## 📥 5. Gestion des SNMP Traps

### 📂 Installation du receiver

```bash
sudo curl -o /usr/bin/zabbix_trap_receiver.pl https://git.zabbix.com/projects/ZBX/repos/zabbix/raw/misc/snmptrap/zabbix_trap_receiver.pl
sudo chmod +x /usr/bin/zabbix_trap_receiver.pl
```

Modifier la ligne :

```perl
$SNMPTrapperFile = '/var/log/snmptrap/snmptrap.log';
```

Créer le répertoire :

```bash
sudo mkdir /var/log/snmptrap
sudo chmod 777 /var/log/snmptrap
```

![Trap Receiver](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix6.png)

---

### 📜 Fichier de configuration SNMP

```bash
sudo nano /etc/snmp/snmptrapd.conf
```

Ajouter :

```
authCommunity execute zabbix
perl do "/usr/bin/zabbix_trap_receiver.pl";
traphandle default /usr/bin/zabbix_trap_receiver.pl
```

---

### 🔁 Rotation des logs

```bash
sudo nano /etc/logrotate.d/snmptrap
```

Contenu :

```
/var/log/snmptrap/snmptrap.log {
    weekly
    rotate 12
    compress
    delaycompress
    missingok
    notifempty
}
```

---

### ▶️ Démarrage du service SNMP

```bash
sudo systemctl stop snmptrapd snmptrapd.socket
sudo systemctl start snmptrapd
sudo systemctl enable snmptrapd
```

---

## 🧩 6. Configuration de Zabbix pour SNMP Traps

```bash
sudo nano /etc/zabbix/zabbix_server.conf
```

Configurer :

```
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log
StartSNMPTrapper=1
```

```bash
sudo systemctl restart zabbix-server
```

![Trap Result](https://raw.githubusercontent.com/Erwanbrt/Orleans/refs/heads/main/docs/images/zabbix7.png)

---

🎉 Vous êtes maintenant prêt à superviser vos connexions VPN et équipements SNMP via Zabbix !


