# **PROJET : DÉPLOIEMENT D'UNE PLATEFORME DE SUPERVISION RÉSEAU AVEC CACTI**

## **1. CONTEXTE ET OBJECTIFS**

L'objectif est de mettre en place un laboratoire de supervision capable de collecter, stocker et visualiser les données de performance d'un parc informatique. Ce projet démontre la capacité à surveiller des environnements hétérogènes (Linux et Windows) tout en optimisant les ressources matérielles limitées.

### **Objectifs spécifiques :**

- Mesurer la charge CPU et l'utilisation RAM en temps réel.
    
- Visualiser le trafic réseau entrant et sortant.
    
- Générer des graphiques historiques pour l'analyse de tendances.
    

---

## **2. ARCHITECTURE TECHNIQUE DU LABORATOIRE**

Pour ce projet, nous utilisons la virtualisation sur **VMware** afin de simuler une infrastructure réseau réelle sur une seule machine physique.

### **A. Dimensionnement des machines (Total 12 Go RAM)**

| Machine           | Système d'Exploitation  | RAM Allouée  | Rôle                            |
| ----------------- | ----------------------- | ------------ | ------------------------------- |
| **Hôte Physique** | Windows / Linux (Asus)  | 5 Go (Libre) | Hyperviseur & Consultation      |
| **Serveur Cacti** | Ubuntu Server 22.04 LTS | 3 Go         | Collecte, Base de données & Web |
| **Cible Client**  | Windows 10              | 4 Go         | Poste supervisé via SNMP        |

Export to Sheets

### **B. Architecture Logicielle (La pile LAMP)**

Le serveur Cacti repose sur une structure en 4 couches :

1. **L**inux (Ubuntu) : Système d'exploitation stable et léger.
    
2. **A**pache : Serveur Web pour l'interface de gestion.
    
3. **M**ariaDB : Stockage des configurations et des seuils.
    
4. **P**HP : Moteur d'exécution du "Poller" (récupérateur de données).
    

---

## **3. PLAN DE RÉALISATION ÉTAPE PAR ÉTAPE**

Le projet se déroulera en **5 phases clés** pour assurer une mise en service progressive.

### **Phase 1 : Préparation de l'environnement virtuel**

- Création des deux VMs dans VMware.
    
- Configuration du réseau en mode **NAT** ou **Bridge** pour que les machines puissent communiquer.
    
- Test de connectivité simple (`ping`) entre Ubuntu et Windows 10.
    

### **Phase 2 : Installation du Serveur (Ubuntu)**

- Mise à jour du système : `sudo apt update && sudo apt upgrade`.
    
- Installation des composants LAMP : `apache2`, `mariadb-server`, `php`.
    
- Installation de **RRDTool** (moteur graphique) et du service **SNMPd**.
    
- Installation du paquet `cacti` et configuration de sa base de données.
    

### **Phase 3 : Configuration de la Cible (Windows 10)**

- Activation de la fonctionnalité "Protocole SNMP" dans les paramètres Windows.
    
- Configuration du service SNMP :
    
    - **Communauté** : `ProjetCacti` (en lecture seule).
        
    - **Sécurité** : Autoriser uniquement l'adresse IP du serveur Ubuntu.
        

### **Phase 4 : Mise en place de la Supervision**

- Connexion à l'interface Web Cacti depuis le navigateur de l'hôte.
    
- Création d'un nouveau "Device" pour la VM Windows 10.
    
- Sélection des "Data Queries" : _SNMP - Interface Statistics_ et _Host MIB - CPU Utilization_.
    
- Création des graphiques associés.
    

### **Phase 5 : Tests et Optimisation**

- Vérification de la réception des données (attendre 5 à 10 minutes).
    
- Optimisation du Poller pour réduire la charge sur le processeur 4ème génération.
    
- Vérification de l'espace disque consommé par les fichiers `.rrd`.
    

---

## **4. SÉCURITÉ ET BONNES PRATIQUES**

- **Isolation** : Les flux SNMP sont limités aux deux machines du labo.
    
- **Performance** : Utilisation d'Ubuntu **Server** (sans interface graphique) pour libérer de la RAM pour Windows 10.
    
- **Persistance** : Configuration de RRDTool pour conserver les données sur 1 mois avec une haute précision.
    

---

## **CONCLUSION**

Ce plan permet de réaliser un projet de supervision complet. Il offre une visibilité totale sur une machine Windows 10 tout en respectant les limites de ton matériel Asus. C'est une base solide qui peut être étendue plus tard par l'ajout de plugins comme **Weathermap**.