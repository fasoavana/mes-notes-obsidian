
## 3.1. Introduction  
Après avoir mis en place l’environnement virtuel (Chapitre 2), il est maintenant possible de déployer une solution de cloud open source. Dans le cadre de ce projet, le choix s’est porté sur **Nextcloud**, un logiciel libre qui permet la gestion et le partage de fichiers, la synchronisation multi-appareils, ainsi que l’intégration de fonctionnalités collaboratives (agenda, contacts, messagerie).  
L’objectif de ce chapitre est de présenter les différentes étapes de l’installation et de la configuration de Nextcloud sur une machine virtuelle Ubuntu, afin d’obtenir un mini-cloud fonctionnel et accessible en réseau local.  

## 3.2. Mise à jour du système  
```bash
sudo apt update && sudo apt upgrade -y
```

## 3.3. Installation des dépendances  
```bash
sudo apt install apache2 mariadb-server \
libapache2-mod-php php php-mysql php-zip php-xml \
php-curl php-gd php-mbstring php-intl php-bcmath \
php-gmp php-imagick php-cli unzip -y
```

## 3.4. Configuration de la base de données  
```bash
sudo mysql -u root -p
```
Puis exécuter :  
```sql
CREATE DATABASE nextcloud;
CREATE USER 'nc_user'@'localhost' IDENTIFIED BY 'motdepassefort';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nc_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 3.5. Téléchargement et installation de Nextcloud  
```bash
cd /tmp
wget https://download.nextcloud.com/server/releases/nextcloud-29.0.5.zip
unzip nextcloud-29.0.5.zip
sudo mv nextcloud /var/www/html/
```
Ajuster les permissions :  
```bash
sudo chown -R www-data:www-data /var/www/html/nextcloud
sudo chmod -R 755 /var/www/html/nextcloud
```

## 3.6. Configuration d’Apache  
Créer un fichier :  
```bash
sudo nano /etc/apache2/sites-available/nextcloud.conf
```
Contenu :  
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
Activer le site et modules :  
```bash
sudo a2ensite nextcloud.conf
sudo a2enmod rewrite headers env dir mime
sudo systemctl reload apache2
```

## 3.7. Lancement et accès à l’interface web  
Accéder à :  
```
http://IP_de_la_VM/nextcloud
```
ou  
```
http://nextcloud.local
```
Configurer ensuite :  
- Compte administrateur  
- Base de données (**nextcloud**, **nc_user**, **motdepassefort**)  

## 3.8. Création d’utilisateurs et partage de fichiers  
Depuis l’onglet **Utilisateurs**, ajouter :  
- **etudiant1** → accès au dossier “Cours Cloud”  
- **professeur** → droits d’écriture sur le dossier partagé  

## 3.9. Optionnel : Déploiement via Docker  
```bash
docker run -d -p 8080:80 nextcloud
```

## 3.10. Conclusion partielle  
Le déploiement de Nextcloud a permis de mettre en place un mini-cloud fonctionnel, hébergé localement dans une machine virtuelle.  
Les étapes ont couvert l’installation du serveur web, la configuration de la base de données, la mise en place de l’application et la gestion des utilisateurs.  
Cette solution constitue une première expérience concrète de cloud privé et servira de base au **Chapitre 4 – Sécurité et accès**.
