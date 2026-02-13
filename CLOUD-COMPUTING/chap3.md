
# Chapitre 3 – Déploiement du mini-cloud  

## 3.1. Installation des dépendances  
```bash
sudo apt install apache2 mariadb-server \
libapache2-mod-php php php-mysql php-zip php-xml \
php-curl php-gd php-mbstring php-intl php-bcmath \
php-gmp php-imagick php-cli unzip -y
```

![[CLOUD-COMPUTING/cloud1.png]]
## 3.2. Configuration de la base de données  
```bash
sudo mysql -u root -p
```
```sql
CREATE DATABASE nextcloud;
CREATE USER 'nc_user'@'localhost' IDENTIFIED BY 'motdepassefort';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nc_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
![[CLOUD-COMPUTING/cloud2.png]]
## 3.3. Installation de Nextcloud  
```bash
cd /tmp
wget https://download.nextcloud.com/server/releases/nextcloud-29.0.5.zip
unzip nextcloud-29.0.5.zip
sudo mv nextcloud /var/www/html/
sudo chown -R www-data:www-data /var/www/html/nextcloud
sudo chmod -R 755 /var/www/html/nextcloud
```
![[CLOUD-COMPUTING/cloud3.png]]
## 3.4. Configuration Apache  
```bash
sudo nano /etc/apache2/sites-available/nextcloud.conf
```
```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html/nextcloud
    ServerName nextcloud.local

    <Directory /var/www/html/nextcloud>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews
    </Directory>
</VirtualHost>
```
![[CLOUD-COMPUTING/cloud4.png]]

Activation :  
```bash
sudo a2ensite nextcloud.conf
sudo a2enmod rewrite headers env dir mime
sudo systemctl reload apache2
```
![[CLOUD-COMPUTING/cloud5.png]]
## 3.5. Accès via navigateur  
```
http://IP_VM/nextcloud
```
Création du compte admin et configuration de la base de données.  
![[CLOUD-COMPUTING/cloud6.png]]
## 3.6. Création d’utilisateurs  
Depuis l’interface :  
- **etudiant1** → accès lecture  
- **professeur** → droits d’édition  
![[CLOUD-COMPUTING/cloud7.png]]
---
![[CLOUD-COMPUTING/cloud8.png]]
# Chapitre 4 – Sécurité et accès  

## 4.1. Sécuriser l’accès SSH  
```bash
sudo apt install openssh-server -y
```
Connexion :  
```bash
ssh user@IP_VM
```

## 4.2. Pare-feu UFW  
```bash
sudo apt install ufw -y
sudo ufw allow OpenSSH
sudo ufw allow 'Apache Full'
sudo ufw enable
```
![[CLOUD-COMPUTING/cloud9.png]]
## 4.3. HTTPS avec Certbot  
```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
```

![[cloud10 1.png]]
---
![[cloud11 1.png]]
![[cloud12 1.png]]


---
