# Chapitre 2 – Mise en place de l’environnement

## 2.1. Introduction  
Avant de déployer un mini-cloud, il est essentiel de préparer l’environnement de virtualisation. Le choix retenu pour ce projet est **VMware Workstation** comme hyperviseur, avec une machine virtuelle sous **Ubuntu Linux**. Afin de bénéficier de fonctionnalités avancées, nous ajoutons également la prise en charge de la **virtualisation KVM**. Le réseau est configuré en mode **Bridge** afin que la VM soit accessible directement sur le même réseau que la machine hôte.  

## 2.2. Installation de VMware Workstation  
1. Télécharger VMware Workstation depuis le site officiel.  
2. Installer VMware sur la machine hôte (Windows ou Linux).  
3. Créer une nouvelle machine virtuelle avec les caractéristiques suivantes :  
   - **Système invité** : Ubuntu 22.04 LTS (64 bits)  
   - **RAM** : 2 Go minimum  
   - **CPU** : 2 cœurs  
   - **Disque dur** : 20 Go (disque virtuel au format VMDK)  
   - **Carte réseau** : mode **Bridge** (la VM utilise le réseau local comme si c’était un poste physique).  

## 2.3. Installation d’Ubuntu sur la VM  
1. Télécharger l’image ISO d’Ubuntu depuis le site officiel (ubuntu.com).  
2. Monter l’ISO dans VMware et démarrer la VM.  
3. Suivre les étapes d’installation :  
   - Choisir la langue et la disposition du clavier.  
   - Installer Ubuntu Desktop ou Server selon les besoins.  
   - Créer un utilisateur administrateur.  
4. Après installation, mettre à jour le système :  
```bash
sudo apt update && sudo apt upgrade -y
```

## 2.4. Installation et activation de KVM  
Bien que VMware soit l’hyperviseur principal, KVM peut être utilisé comme solution native de virtualisation sous Ubuntu, pour lancer des conteneurs ou tester d’autres VMs.  

Installation de KVM et des outils associés :  
```bash
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y
```

Vérifier que KVM est actif :  
```bash
sudo kvm-ok
```

Si la commande retourne *“KVM acceleration can be used”*, l’installation est correcte.  

## 2.5. Configuration réseau en mode Bridge  
Dans VMware, la carte réseau de la VM est réglée sur **Bridged Adapter**.  
Cela permet à la VM d’obtenir une adresse IP sur le même réseau que la machine hôte.  

Vérifier l’adresse IP attribuée :  
```bash
ip a
```

Exemple de résultat attendu :  
- Hôte : 192.168.1.10  
- VM Ubuntu : 192.168.1.20  

Les deux machines peuvent ainsi communiquer directement, et la VM sera accessible depuis d’autres postes du réseau local.  

## 2.6. Conclusion partielle  
À ce stade, l’environnement de base est opérationnel : VMware héberge une VM Ubuntu fonctionnelle, KVM est installé pour des usages avancés, et la configuration réseau en mode Bridge rend le système accessible depuis le réseau local. Nous pouvons désormais passer au **Chapitre 3 : Déploiement du mini-cloud**.  

---

# Chapitre 3 – Déploiement du mini-cloud

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
