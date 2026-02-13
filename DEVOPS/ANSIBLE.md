pte Utilisation de playbookk

 1.  Sécurité
 2. Tâche lourde : automatisation
 3. Réutilisation
 4. Pull images avec condition

lancer l'image, soit transformer en dockerfile soit , lancer un conteneur et rattacher le volume
serveur ssh : creer user et paswword


## Récupération de l'image qu'on va utliser comme cible
```
docker pull debian:stable-slim
```

## Lancer avec montage du dossier courant
```
docker run -it --name mon_conteneur_ctf -v "$(pwd):/app" debian:stable-slim /bin/bash
```
## Pour relancer le processus du conteneur
```
docker exec -it mon_conteneur_ctf /bin/bash
```
## Installation de serveur ssh sur les machine cible 
```
apt update && apt install -y openssh-server sudo

```

**Créer l'utlisateur**
```
useradd -m -s /bin/bash ansible_user
echo "ansible_user:password" | chpasswd
```
ou on va utliser directement l'utlisateur  **root** mais on va definir un mot de passe pour lui
```
echo "root:password" | chpasswd
```

Et comme sur la plupart des distribution linux l'accès ssh sur l'utilisateur root est desactivé par défaut donc on va l'activer


```
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
```
Redémmarer le service pour appliquer les changements

```
service ssh restart
```
# Redémarrer le service pour appliquer les changements
service ssh restart

**Installer les outils réseaux**

```
apt update
apt install -y iproute2 net-tools
```
**Demmarer le service ssh**
```
service ssh start
```
**Pour voir l'adresse ip du container**
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' vibrant_solomon
```
**Connecter en ssh**
```
ssh root@172.17.0.2
```

************************************************************

# Objetif
- Utiliser Ansible pour piloter le conteneur comme s'il s'agissait d'un serveur distant
- Pour que cela fonctionne, Ansible doit pouvoir "parler" au conteneur via SSH sans taper de mot de passe
##### 1. Préparer l'inventaire (`hosts.ini`)
   Le  fichier `hosts.ini` est la "liste de contacts" d'Ansible.
```
   nano hosts.ini
```

```
[containers]
debian_target ansible_host=172.17.0.2 ansible_user=root
```
##### 2. Configurer l'accès SSH (La clé publique)

Ansible utilise la clé SSH privée pour se connecter. On doit donc donner la **clé publique** au conteneur.

**Créer la clé public si n'existe pas**
```
ssh-keygen -t rsa -b 4096
```
**Afficher la clé public sur le pc**


```
cat ~/.ssh/id_rsa.pub
```
**Dans le conteneur Debian**, crée le dossier et colle la clé :

```
mkdir -p /root/.ssh
echo "TA_CLE_PUBLIQUE_COPIEE" > /root/.ssh/authorized_keys
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```

Une fois que c'est fait ,on va tester la liason sur notre Pc , si cela reusi, notre configuration est pret
```
ansible containers -i hosts.ini -m ping
```
Maintenant on va pouvoir plannifier des taches à partir de **playbook.yml**

Voici une code minimal



---
```
- hosts: containers
  become: yes
  tasks:
    - name: Installation git
      apt:
        name: git
        state: present

    - name: Installation de Nginx
      apt:
        name: nginx
        state: present

    - name: S'assurer que Nginx est démarré
      service:
        name: nginx
        state: started
```

Pour lancer la taches , on execute cette commande

```
ansible-playbook -i hosts.ini playbook.yml
```

On va meme utiliser un deuxième conteneur pour finir la test de fonciotnnement, on va choisir **fedora**

```
docker run -it --name fedora_target -v "$(pwd):/app" fedora:latest /bin/bash
```


Preparer fedora

# On utilise dnf au lieu de apt
dnf install -y openssh-server python3 cracklib-dicts

# Création des clés d'hôte SSH (obligatoire sur Fedora/RedHat pour lancer le service)
ssh-keygen -A

# Lancer le service SSH
/usr/sbin/sshd -D &

**Ajouter la clé public**  

**Maintenant on va modifier le playbook pour s'adapter à chaque package**

---
```
- hosts: containers
  become: yes
  tasks:
    - name: Installation git (automatique selon l'OS)
      package:
        name: git
        state: present

    - name: Installation de Nginx
      package:
        name: nginx
        state: present
```

Autoriser le login root
```
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
```