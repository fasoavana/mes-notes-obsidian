

Le routage inter-VLAN est le processus consistant à acheminer le trafic réseau d'un VLAN à un autre. Par défaut, les commutateurs (switches) de couche 2 ne peuvent pas faire cela. Il nous faut un routeur.

## I. Le Concept de l'Encapsulation 802.1Q

Lorsqu'un PC du **VLAN 10** envoie un paquet à un PC du **VLAN 30**, le switch doit envoyer ce paquet au routeur. Pour que le routeur sache d'où vient le paquet, le switch ajoute une "étiquette" (Tag) à la trame Ethernet.

- **Le Tag 802.1Q :** C'est un champ de 4 octets inséré dans l'en-tête Ethernet qui contient le **VLAN ID**.
    
- **Le Trunk :** C'est le lien capable de transporter ces trames "étiquetées".
    

---

## II. Configuration détaillée du Switch (L2)

### 1. Création et Nommage (Base de données VLAN)

Il est crucial de nommer les VLANs pour s'y retrouver dans les grands réseaux.

Bash

```
(config)# vlan 10
(config-vlan)# name Engineering
(config-vlan)# exit
```

### 2. Ports d'Accès vs Ports Trunk

- **Access :** Pour les terminaux (PC). La trame arrive "normale" (untagged).
    
- **Trunk :** Pour les liaisons Switch-Switch ou Switch-Routeur. La trame circule avec un tag.
    

**Commande de sécurité (Port Access) :**

Bash

```
(config)# interface fa0/1
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# spanning-tree portfast  # Optionnel : accélère la connexion du PC
```

**Commande Trunk (Le lien vital) :**

Bash

```
(config)# interface fa0/5
(config-if)# switchport mode trunk
(config-if)# switchport trunk native vlan 99  # Change le VLAN par défaut (sécurité)
(config-if)# switchport trunk allowed vlan 10,20,30 # Elague le trafic inutile
```

---

## III. Configuration détaillée du Routeur (L3)

Le routeur 2911 n'a qu'une interface physique connectée au switch, mais il doit gérer 3 réseaux différents. On crée donc des **interfaces virtuelles (sous-interfaces)**.

### 1. L'interface parente

Elle doit être allumée, mais elle ne porte généralement **pas** d'adresse IP elle-même.

Bash

```
(config)# interface g0/0
(config-if)# no shutdown
```

### 2. Les sous-interfaces (Sub-interfaces)

Le numéro après le point (`.10`) est arbitraire, mais par convention, on utilise le numéro du VLAN.

Bash

```
(config)# interface g0/0.10
(config-subif)# encapsulation dot1Q 10  # INDISPENSABLE : définit le protocole et le VLAN
(config-subif)# ip address 192.168.1.62 255.255.255.192
(config-subif)# description Passerelle_VLAN_10
```

---

## IV. Calcul des Sous-réseaux (Subnetting)

Dans ton exemple, tu utilises un masque **/26** (`255.255.255.192`). Voici comment ton réseau est découpé :

|VLAN|Plage d'adresses réseau|Plage d'IP utilisables|Passerelle (Router)|
|---|---|---|---|
|**10**|`192.168.1.0/26`|`.1` à `.62`|`192.168.1.62`|
|**20**|`192.168.1.64/26`|`.65` à `.126`|`192.168.1.126`|
|**30**|`192.168.1.128/26`|`.129` à `.190`|`192.168.1.190`|

Export to Sheets

---

## V. Troubleshooting (Dépannage) - Check-list

1. **Vérifier le Trunk :** `show interfaces trunk`
    
    - _Erreur commune :_ Le port du switch vers le routeur est resté en `mode access`.
        
2. **Vérifier l'encapsulation :** `show vlans` sur le routeur.
    
    - _Erreur commune :_ L'ID dans `encapsulation dot1Q` ne correspond pas au VLAN du switch.
        
3. **Vérifier la Passerelle (Gateway) :** Sur le PC, fais un `ipconfig`.
    
    - _Erreur commune :_ L'IP de la gateway sur le PC ne correspond pas à l'IP de la sous-interface du routeur.
        
4. **Vérifier le VLAN natif :** Si tu vois `%CDP-4-NATIVE_VLAN_MISMATCH`, vérifie que le `native vlan` est identique des deux côtés du câble.