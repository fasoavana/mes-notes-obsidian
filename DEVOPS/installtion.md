Installer jenkins
relien m doker hub sy github


**Installation du jenkis

# Démarrer un nouveau conteneur avec les ports exposés
docker run -d \
  --name jenkins-new \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  myjenkins-blueocean:2.528.3-1


###### Afficher le mot de passe par defaut

8f7b7dff037c4c1c8845d17db751dccc


###### Infrastructe

teraform: infrastructure qui permet d'automatiser dans cloud


**On utlise l'environnement cloud 
 - Teraform (Iac=Infrastructure as Code)
 - Ansible (configuration management)
 - Jenkins (CI/CD avanc)
 - Github (Versionning)
 - Trivy (Scan vulnerabilité image docker)
 - Dockerhub (registry)

Dossier

APP front ----+ dockerfile
APP back -----+ dockerfile
  docker-comppose
  Jenkinsfile
  main.tf(teraform)
  playbook.yml(ansible) ----
     On peut faire de l'installation simultanné , quelque soit le nombre de la machine à partir de fichier **inventory.ini** , c'est à dire que playbook est la responsable de l'installation


////La semaine prochaine on va installer docker avec cloud à partir de ansible
creer  insfrastructure avec teraform




teraform : Deployer C2
ansible(installer avec terminal) : pull image debian ou ubuntu avec docker
	ansible --version

Installation Ansible

`Sudo apt update`
`sudo apt install software-properties-common`
`sudo add-apt-repository --yes --update ppa:ansible/ansible`
`sudo apt install ansible -y`

Créer un fichier  **hosts.ini**

[serveur_c2]
IP_DE_TON_SERVEUR ansible_user=root

Créer un fichier **docker_deploy.yml**

---
- name: Installation de Docker et Image C2
  hosts: serveur_c2
  become: yes
  tasks:
    - name: Installer Docker
      apt:
        name: docker.io
        state: present
        update_cache: yes

    - name: S'assurer que Docker est démarré
      service:
        name: docker
        state: started

    - name: Pull l'image Debian (Base pour le C2)
      community.docker.docker_image:
        name: debian
        source: pull


Installer Dépendance docker pour Ansible

```
ansible-galaxy collection install community.docker
```
## Mise en place de TERAFORM



avant tout ça, il faut une preparation pour AWS : il faut installer un outis pour qu'ansible puisse parler à AWS
```
sudo apt install awscli -yd
```

On va installer **TERAFORM**


## 1. Ajoute la clé GPG de HashiCorp

```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```
## 2. Ajoute le dépôt officiel aux sources

```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```
## 3. Met à jour et installe

```
sudo apt update && sudo apt install terraform -y
```

On va maintenant creer  **main.tf** , ce petite script va demander à AWS une petite machine gratuite **t2.micro**

```
`provider "aws" {`
  `region     = "us-east-1" # Tu peux changer pour eu-west-3 (Paris)`
  # `Il faudra configurer tes accès via 'aws configure'`
`}`

`resource "aws_instance" "c2_server" {`
  `ami           = "ami-0c7217cdde317cfec" # ID pour Ubuntu 22.04 en us-east-1`
  `instance_type = "t2.micro"             # Type de machine (éligible au free tier)`

  `tags = {`
    `Name = "C2-Deployer"`
  `}`
`}`

`output "instance_ip" {`
  `value = aws_instance.c2_server.public_ip`
`}`
```


### Initialisation de TERRAFORM

```
terraform init
```
### Vérification et Construction

Pour voir ce que l'ingénieur compte faire sans rien payer encore :
```

terraform plan
```

