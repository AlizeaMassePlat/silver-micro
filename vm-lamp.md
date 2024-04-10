
# VM LAMP Setup

## Création de la VM

Pour créer une VM et installer Debian, suivez les instructions détaillées dans le tutoriel disponible ici : [Installer Debian Sous VM](https://dautrylimoges.scenari-community.org/1NSI/Linux_shell_et_commandes_principales/res/IntallerDebianSousVM.pdf).

## Installation de la Stack LAMP

Une fois Debian installé, procédez à l'installation de la stack LAMP (Linux, Apache, MariaDB, PHP) en utilisant les commandes suivantes :

```bash
# Mettre à jour les paquets
sudo apt-get update

# Installer Apache
sudo apt-get install -y apache2

# Assurer que Apache démarre automatiquement
sudo systemctl enable apache2

# Activer le module de réécriture pour Apache
sudo a2enmod rewrite

# Installer PHP
sudo apt-get install -y php

# Installer MariaDB
sudo apt-get install -y mariadb-server

# Sécuriser l'installation de MariaDB
sudo mariadb-secure-installation
```

L'adresse IP du serveur Apache sur Debian est `192.168.136.128`.

