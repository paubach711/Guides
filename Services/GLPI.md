# GLPI Installation

This guide is for installing the GLPI service. The OS used in this guide is Debian 13. Everything in this guide is based on official and unofficial documentation and forums.

## Updating the OS and installing dependencies

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

GLPI is a web-based service, so it needs some important web packages, such as Apache, PHP, and MariaDB.

## MariaDB and basic database configuration

GLPI uses a database to store information. In this case, MariaDB will be used to store information about users, computers, and other data.

```
systemctl enable --now apache2 mariadb
mariadb
```

Inside the MariaDB database, we can start configuring everything GLPI will need:

```
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'CAMBIA_ESTA_PASSWORD';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
```

## GLPI Installation

This part explains how to install GLPI using the official GitHub repository of [glpi-project](https://github.com/glpi-project).

```
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/X.X/glpi-X.X.tgz
tar -xzf glpi-*.tgz
mv glpi /var/www/glpi
```

Apache uses its own permissions, so we need to change them:

```
chown -R www-data:www-data /var/www/glpi
find /var/www/glpi -type d -exec chmod 755 {} \;
find /var/www/glpi -type f -exec chmod 644 {} \;
```

## Apache configuration

```
a2enmod rewrite
systemctl restart apache2
```

```
nano /etc/apache2/sites-available/glpi.conf
```

```
<VirtualHost *:80> 
	ServerName glpi-scc 
	
	DocumentRoot /var/www/glpi/public 
	
	<Directory /var/www/glpi/public>
		Require all granted 
		RewriteEngine On 
		RewriteCond %{REQUEST_FILENAME} !-f 
		RewriteRule ^(.*)$ index.php [QSA,L] 
	</Directory> 
	
	ErrorLog ${APACHE_LOG_DIR}/glpi_error.log 
	CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined 
</VirtualHost>
```

Enable the GLPI site and disable the default Apache site:

```
a2ensite glpi.conf
a2dissite 000-default.conf
apache2ctl configtest
systemctl reload apache2
```

After that, you can open GLPI in your browser:

```
http://<IP-SERVER>
```

And proceed with the installation from there.

---

Copyright © 2026 Pau Ubach

This document is licensed under the GNU Free Documentation License v1.3 or later.

SPDX-License-Identifier: GFDL-1.3-or-later
