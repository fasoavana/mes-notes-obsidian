# Exercice 1 : Segmentation Réseau (Marketing & DSI)

**Objectif :** Diviser le réseau `192.168.20.0/24` en deux sous-réseaux via un routeur et des VLANs.

### 1. Configuration du Routeur (Router0)

Bash

```
Router> enable
Router# configure terminal

# --- Configuration Interface Marketing (Haut) ---
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip address 192.168.20.1 255.255.255.128
Router(config-if)# no shutdown
Router(config-if)# exit

# --- Configuration Interface DSI (Bas) ---
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.20.129 255.255.255.128
Router(config-if)# no shutdown
Router(config-if)# exit
```

### 2. Configuration du Switch Marketing (Switch0)

Bash

```
Switch> enable
Switch# configure terminal

# --- Création du VLAN 10 ---
Switch(config)# vlan 10
Switch(config-vlan)# name Marketing
Switch(config-vlan)# exit

# --- Assignation des ports (PC, Imprimante et Liaison Routeur) ---
Switch(config)# interface range fa0/1 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
```

### 3. Configuration du Switch DSI (Switch1)

Bash

```
Switch> enable
Switch# configure terminal

# --- Création du VLAN 20 ---
Switch(config)# vlan 20
Switch(config-vlan)# name DSI
Switch(config-vlan)# exit

# --- Assignation des ports (PC, Imprimante et Liaison Routeur) ---
Switch(config)# interface range fa0/1 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
```

---

# Exercice 2 : Infrastructure Sans-Fil (WLC & AP)

**Objectif :** Déployer un réseau Wi-Fi centralisé avec gestion des VLANs et DHCP sur un switch multicouche.

### 1. Configuration du Switch Multicouche (3650)

_Note : Pense à ajouter l'alimentation (Power Supply) dans l'onglet Physical avant._

Bash

```
Switch> enable
Switch# configure terminal

# --- Activation du routage Inter-VLAN ---
Switch(config)# ip routing

# --- Création des VLANs de l'infrastructure ---
Switch(config)# vlan 10
Switch(config-vlan)# name Management
Switch(config)# vlan 100
Switch(config-vlan)# name Internal
Switch(config)# vlan 200
Switch(config-vlan)# name Guest
Switch(config-vlan)# exit

# --- Configuration des passerelles (SVI) ---
Switch(config)# interface vlan 10
Switch(config-if)# ip address 172.16.1.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 100
Switch(config-if)# ip address 10.0.0.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 200
Switch(config-if)# ip address 10.1.0.1 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

# --- Configuration du Serveur DHCP ---
# Pool pour les Antennes (AP)
Switch(config)# ip dhcp pool AP_MGMT
Switch(dhcp-config)# network 172.16.1.0 255.255.255.0
Switch(dhcp-config)# default-router 172.16.1.1
Switch(dhcp-config)# exit

# Pool pour le Wi-Fi Interne
Switch(config)# ip dhcp pool INTERNAL_WIFI
Switch(dhcp-config)# network 10.0.0.0 255.255.255.0
Switch(dhcp-config)# default-router 10.0.0.1
Switch(dhcp-config)# exit

# --- Configuration des ports physiques ---
# Ports vers AP et WLC (Mode Trunk)
Switch(config)# interface range gig1/0/1, gig1/0/3, gig1/0/4
Switch(config-if-range)# switchport trunk encapsulation dot1q
Switch(config-if-range)# switchport mode trunk
Switch(config-if-range)# switchport trunk native vlan 10

# Port vers PC0 (Accès Management)
Switch(config)# interface gigabitEthernet 1/0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

### 2. Rappel de configuration statique du WLC

Ces paramètres sont à entrer dans l'onglet **Config > Management** du WLC0 :

- **IP Address :** `172.16.1.5`
    
- **Subnet Mask :** `255.255.255.0`
    
- **Default Gateway :** `172.16.1.1`