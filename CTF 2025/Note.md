
# ÉTAPE 1 —  DOCUMENTATION

le **fichier Excel** listant tous les outils inclus dans Exegol

https://docs.google.com/spreadsheets/d/1gvcZZC2RZj3Q3Oy3kSLeDn0PYqIiFcI5jZRO17pQdcg/edit?usp=sharing

# CTF2025 – Guide officiel (du commencement à la pratique)


---

## 📅 Planning général

### 🔹 15/06/2025 — Information & préparation

- Présentation du CTF
    
- Explication des objectifs (attaque, défense, rapport)
    
- Liste des outils à installer
    
- Choix du système d’exploitation
    

---

### 🔹 16/06/2025 — Installation des outils (obligatoire)

Chaque groupe doit installer et tester les outils suivants :

- Docker
    
- Exegol
    
- Outil de capture vidéo (Vokoscreen **ou** OBS Studio)
    

> 🎯 Objectif : avoir un environnement **fonctionnel avant le jour du CTF**.

---

### 🔹 17/07/2025 — Journée CTF

#### 🕘 AM — White Box (accès aux machines)

- Accès direct aux machines vulnérables
    
- Tous les participants **doivent utiliser Docker + Exegol**
    
- Les machines vulnérables seront communiquées le jour même
    

#### 🕑 PM — ProLab CTF _Attack & Defend_ (Black Box)

- Aucun accès aux machines vulnérables
    
- Les participants :
    
    - attaquent des machines
        
    - protègent leurs propres services
        

🔧 Outils autorisés :

- Exegol
    
- Kali Linux
    
- Parrot OS
    
- BlackUbuntu
    
- SnoopGod
    
- Autres outils de pentest
    

📹 **Obligatoire : Capture vidéo**

- Logiciel : Vokoscreen ou OBS Studio
    
- Qualité moyenne
    
- Framerate faible
    
- Sauvegarde **toutes les 30 minutes** (pour éviter des fichiers trop lourds)
    

---

### 🔹 18/07/2025 — Rédaction & préparation

#### 📄 Rapport Word

- Vulnérabilités exploitées
    
- Impacts de sécurité
    
- Captures d’écran issues des vidéos du 17/07/2025
    

#### 📊 Présentation PPT (5 minutes max)

- Vulnérabilités **critiques et intéressantes**
    
- Contenu compréhensible pour un public non expert
    

---

### 🔹 19/07/2025 — Dépôt & présentation

- Dépôt Word + PPT **avant 06h00**
    
- Présentation orale (sans démonstration live)
    
- Possibilité de montrer un extrait vidéo
    
- Durée maximale : **5 minutes**
    

---

## 💻 Choix du système d’exploitation

### ✔ Option 1 : Linux (recommandé)

- Linux avec Docker installé
    

### ✔ Option 2 : Windows

- VMware ou VirtualBox
    
- Machine Linux légère :
    
    - Lubuntu
        
    - Xubuntu
        

> 📦 Tous les ISO et outils seront disponibles sur le **serveur local INSI à partir du 16/06/2025**  
> (pour éviter le téléchargement de plus de 45 Go)

---

## 🛡️ Exegol — Présentation

**Exegol** est un environnement de pentest basé sur Docker.  
C’est une **surcouche Python (wrapper)** qui simplifie l’utilisation de conteneurs spécialisés CTF.

---

## 🧭 ÉTAPE 1 — Lire la documentation (obligatoire)

- 📘 Documentation officielle : [https://docs.exegol.com](https://docs.exegol.com)
    
- 🎥 Vidéos de prise en main (Document 1 à 15)
    

📌 **Important** :

- Rechercher le **fichier Excel** listant tous les outils inclus dans Exegol
    

---

## ⚙️ ÉTAPE 2 — Installation des dépendances (Linux)

```bash
sudo apt update
sudo apt install -y docker.io vokoscreen pipx
```

```bash
pipx ensurepath
pipx install exegol
pipx upgrade exegol
```

🔁 **Redémarrer le PC** après installation

---

## 📦 ÉTAPE 3 — Chargement des images Exegol

- Plusieurs images existent : FREE, FULL, OSINT, AD, WEB, LIGHT
    
- ⚠️ **Ne pas utiliser l’image FREE** (outils insuffisants)
    

📌 Image utilisée pour le CTF :

- `FULL – LAST – 04/12/2025`
    

---

## ⬇️ ÉTAPE 4 — Charger l’image Exegol

```bash
docker load -i nwodtuhs_exegol_full_04_12_2025.tar.gz
```

```bash
docker images
```

---

## 🧪 ÉTAPE 5 — Tester Exegol

```bash
exegol info
```

Créer et démarrer un conteneur :

```bash
exegol start master full-04-12-2025
```

Dans un autre terminal :

```bash
docker ps
```

📌 Astuces dans Exegol :

- ↑ ↓ : historique des commandes
    
- → : validation des suggestions
    
- `export` pour gérer les paramètres
    

---

## 🧨 ÉTAPE 6 — Démo : File Upload avec DVWA

- Une démonstration DVWA sera fournie sur le serveur INSI
    
- Objectif :
    
    - exploitation
        
    - capture vidéo
        
    - preuve pour le rapport final
        

📺 Vidéo de référence : Document 1

---

## ✅ Conclusion

Ce guide vous accompagne :

- avant le CTF
    
- pendant les attaques/défenses
    
- après, pour la rédaction et la présentation
    

🎯 **Objectif final : comprendre, pratiquer et expliquer la cybersécurité**