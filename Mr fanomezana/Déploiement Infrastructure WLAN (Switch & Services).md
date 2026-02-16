

> [!ABSTRACT] Objectif Configurer un Switch Multicouche pour gérer le routage de 3 VLANs, distribuer des IPs via DHCP (incluant l'option 43 pour le WLC) et sécuriser l'accès en SSH.

---

## 🟦 Étape 1 : Fondations des VLANs

_On crée les "tuyaux" pour séparer les flux._

- [ ] **Créer les VLANs :**
    

Bash

```
conf t
vlan 10
 name Management
vlan 100
 name Internal
vlan 200
 name Guest
exit
```

- [ ] **Configurer les adresses de secours (Passerelles) :**
    

Bash

```
interface vlan 10
 ip address 192.168.1.1 255.255.255.0
interface vlan 100
 ip address 10.0.0.1 255.255.255.0
interface vlan 200
 ip address 10.1.0.1 255.255.255.0
exit
```

---

## 🟨 Étape 2 : Intelligence & Adressage (DHCP)

_Le Switch va distribuer les adresses IP automatiquement aux bornes et aux clients._

- [ ] **Activer le moteur de routage :**
    

Bash

```
ip routing
```

- [ ] **Configurer le Pool pour les Points d'Accès (VLAN 10) :**
    

> [!IMPORTANT] L'Option 43 Elle permet aux bornes (LAP) de trouver l'adresse du contrôleur (WLC) `192.168.1.100`.

Bash

```
ip dhcp pool VLAN10
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 option 43 ip 192.168.1.100
```

---

## 🟩 Étape 3 : Connexion Physique (EtherChannel & Ports)

_On regroupe les câbles vers le WLC pour plus de vitesse et de sécurité._

- [ ] **Lien vers le WLC (Trunk Aggregé) :**
    

Bash

```
interface range f0/1, f0/4
 channel-group 1 mode on
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,100,200
```

- [ ] **Lien vers les bornes WiFi (Ports d'accès) :**
    

Bash

```
interface range f0/2 - 3
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
```

---

## 🔒 Étape 4 : Sécurisation (SSH)

_On ferme la porte au Telnet pour utiliser SSH._

- [ ] **Générer l'identité du switch :**
    

Bash

```
hostname SW1
ip domain-name lab.local
crypto key generate rsa
# Choisir 2048
```

- [ ] **Créer l'accès Admin :**
    

Bash

```
username admin privilege 15 secret cisco123
line vty 0 4
 login local
 transport input ssh
```

---

## 🏁 Étape 5 : Vérification du succès

_Copie ces commandes pour vérifier que tout est au "vert"._

|Commande|Ce qu'il faut vérifier|
|---|---|
|`show ip dhcp binding`|Est-ce que mes bornes ont une IP ?|
|`show etherchannel summary`|Est-ce que le lien WLC est `(SU)` ?|
|`show ip interface brief`|Est-ce que les VLANs sont `up/up` ?|
