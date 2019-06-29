# Linux Server Configuration

IP Address and SSH Port:

  - IP Address: 54.93.246.90
  - SSH Port: 2200
  - URL: http://54.93.246.90.xip.io/
  - grader's ssh key is located in /home/grader/.ssh

# Software Installed:

  - Apache
  - PostgreSQL
  - GIT


# Steps:
  - # Update Packages:
```
  - sudo apt-get update 
    sudo apt-get upgrade
```
  - # Create grader account and give it root access:
  ```
  - sudo adduser grader
  -  sudo visudo
  -  grader ALL = (ALL:ALL) ALL
  ```
  -  # Set SSH Key Login:
  ```
    -Generate SSH key locally using ssh-keygen
    -Login in as the grader user 
     su - grader
    -Make .ssh directory
     mkdir .ssh
    -Create and edit authorized_keys file inside .ssh dierctory and add the generated key.
    touch .ssh/authorized_keys
    nano .ssh/authorized_keys
    -Change file permissions:
    chmod 700 .ssh
    chmod 644 .ssh/authorized_keys
```
  -  # Configure SSH Port:
  ```
  Sudo nano /etc/ssh/sshd_config and change port to 2200
```

  -  # Configure Firewall:
  ```
  Sudo ufw allow ntp
  Sudo ufw allow www
  Sudo ufw allow 2200/tcp
  Sudo ufw enable
```
 -  # Install Apache:
 ```
  sudo apt-get install apache2
```
 -  # Install mod_wsgi:
```
  sudo apt-get install libapache2-mod-wsgi
```  
 -  # Install Git and Clone Item Catalog Project:
 ```
 sudo apt-get install git
 cd /var/www/
 mkdir ItemCatalog
 cd ./ItemCatalog
 git clone https://github.com/omnia-kamal/Item-Catalog
 sudo mv project.py __init__.py
 ```
 -  # Install PostgreSQL:
```
  Install PostgreSQL `sudo apt-get install postgresql`
```
 -  # Setting Up User and Database:
 ```
 CREATE DATABASE catalog;
 CREATE USER catalog;
 ALTER ROLE catalog WITH PASSWORD 'catalog';
 GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
 ```
  -  # Configuring Virtual Host:
```
sudo nano /etc/apache2/sites-available/ItemCatalog.conf
<VirtualHost *:80>
		ServerName 54.93.246.90
		ServerAdmin test@test.com
		WSGIScriptAlias / /var/www/catalog/catalog.wsgi
		<Directory /var/www/catalog/catalog/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/catalog/catalog/static
		<Directory /var/www/catalog/catalog/static/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

  -  # Create the .wsgi file:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/ItemCatalog/")

from catalog import app as application
application.secret_key = 'secret key'
```
  -  # Restart Apache:
```
sudo service apache2 restart 
```


# Refrences:
```
Udacity FSND
StackOverFlow
Github
