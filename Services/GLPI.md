# GLPI Installation 

This guide is for installing GLPI service, the SO I'm gonna use is Debian 13, everything in this guide is based on oficial and non oficial documentation and forums.

## Updating SO and installing dependencies

```
apt update
apt upgrade -y
```

```
apt install -y \
  apache2 mariadb-server \
  php php-cli php-common libapache2-mod-php \
  php-mysql php-gd php-intl php-curl php-mbstring php-xml \
  php-zip php-bz2 php-ldap php-apcu \
  php-opcache php-readline \
  unzip wget curl tar
```

GLPI being a web based service needs to install some important packages for web, like: apache,php,mariadb

## mariadb and basic database configuration

GLPI uses databases to store info in this case mariadb will be used for storing info about users,computers and other stuff

```
systemctl enable --now apache2 mariadb
mariadb
```
Inside the mariadb database we can start to configure everything GLPI will need:
```
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'CAMBIA_ESTA_PASSWORD';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
```

## GLPI Installation

This part will explain how to install GLPI using the official github of [glpi-project](https://github.com/glpi-project) 

```
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/X.X/glpi-X.X.tgz
```

