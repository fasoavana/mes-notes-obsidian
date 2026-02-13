# Exegol — Commandes essentielles & Guide d’audit Pentest

Ce document est un **guide complet et pédagogique** listant :

- les commandes **Exegol**
    
- les commandes **Docker utiles au CTF**
    
- les **étapes d’un audit/pentest**
    
- les **outils clés** (nmap, nikto, sqlmap, etc.)
    
- une **définition simple pour chaque étape**
    

---

## 1️⃣ Commandes essentielles Exegol

### 🔹 Informations & gestion

```bash
exegol info
```

➡️ Affiche la configuration Exegol et Docker

```bash
exegol images
```

➡️ Liste les images Exegol disponibles

```bash
exegol start master full-04-12-2025
```

➡️ Crée et démarre un conteneur Exegol nommé `master`

```bash
exegol stop master
```

➡️ Arrête le conteneur

```bash
exegol restart master
```

➡️ Redémarre le conteneur

```bash
exegol remove master
```

➡️ Supprime le conteneur

```bash
exegol exec master
```

➡️ Entre dans un conteneur déjà lancé

📌 **Rôle d’Exegol** : poste de l’attaquant

---

## 2️⃣ Commandes Docker utiles (diagnostic)

```bash
docker images
```

➡️ Liste les images Docker

```bash
docker ps
```

➡️ Conteneurs en cours d’exécution

```bash
docker ps -a
```

➡️ Tous les conteneurs

```bash
docker load -i image.tar.gz
```

➡️ Charge une image Exegol locale

---

## 3️⃣ Méthodologie générale d’un audit Pentest

### 🧭 Étapes classiques

1. Reconnaissance
    
2. Scan & enumeration
    
3. Identification des vulnérabilités
    
4. Exploitation
    
5. Post-exploitation
    
6. Rapport
    

---

## 4️⃣ Reconnaissance (Recon)

### 📘 Définition

➡️ Collecter un maximum d’informations **sans attaquer directement**.

### 🔹 Outils & commandes

#### 🔍 Ping

```bash
ping -c 4 192.168.1.10
```

➡️ Vérifier si la machine est en ligne

#### 🔍 Whois

```bash
whois example.com
```

➡️ Infos sur le domaine

#### 🔍 DNS / OSINT

```bash
nslookup example.com
```

```bash
theHarvester -d example.com -b all
```

---

## 5️⃣ Scan réseau & ports

### 📘 Définition

➡️ Identifier les ports ouverts et services exposés.

### 🔹 Nmap (outil clé)

#### Scan rapide

```bash
nmap 192.168.1.10
```

#### Scan services + versions

```bash
nmap -sC -sV 192.168.1.10
```

#### Scan complet

```bash
nmap -p- -A 192.168.1.10
```

#### Scan furtif

```bash
nmap -sS 192.168.1.10
```

---

## 6️⃣ Enumeration (approfondissement)

### 📘 Définition

➡️ Explorer en détail les services détectés.

### 🔹 HTTP / Web

```bash
nikto -h http://192.168.1.10
```

➡️ Scan vulnérabilités web connues

```bash
gobuster dir -u http://192.168.1.10 -w /usr/share/wordlists/dirb/common.txt
```

➡️ Découverte de répertoires cachés

```bash
ffuf -u http://192.168.1.10/FUZZ -w wordlist.txt
```

➡️ Fuzzing

---

## 7️⃣ Audit SQL & Injection

### 📘 Définition

➡️ Tester les entrées utilisateur pour une injection SQL.

### 🔹 sqlmap

#### Test basique

```bash
sqlmap -u "http://site.com/page.php?id=1"
```

#### Détection automatique

```bash
sqlmap -u "http://site.com/page.php?id=1" --batch
```

#### Extraction de bases

```bash
sqlmap -u "http://site.com/page.php?id=1" --dbs
```

#### Extraction de tables

```bash
sqlmap -u "http://site.com/page.php?id=1" -D dbname --tables
```

---

## 8️⃣ Brute force & mots de passe

### 📘 Définition

➡️ Tester la robustesse des authentifications.

### 🔹 Hydra

```bash
hydra -l admin -P rockyou.txt ssh://192.168.1.10
```

```bash
hydra -L users.txt -P pass.txt ftp://192.168.1.10
```

---

## 9️⃣ Exploitation

### 📘 Définition

➡️ Exploiter une vulnérabilité pour obtenir un accès.

### 🔹 Metasploit

```bash
msfconsole
```

```bash
search exploit smb
```

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
```

---

## 🔟 Post-exploitation

### 📘 Définition

➡️ Étendre l’accès et collecter des preuves.

### 🔹 Linux

```bash
linpeas.sh
```

### 🔹 Windows

```bash
winpeas.exe
```

```bash
whoami
```

```bash
id
```

---

## 1️⃣1️⃣ Capture vidéo (obligatoire CTF)

### 🎥 Vokoscreen

- Format : MP4
    
- Codec : H.264
    
- FPS : faible (10–15)
    
- Sauvegarde toutes les 30 min
    

### 🎬 OBS

- Format : MKV
    
- Remux MP4 après
    
- Qualité moyenne
    

---

## 1️⃣2️⃣ Résumé pédagogique (à retenir)

> Exegol = environnement de l’attaquant  
> Recon → Scan → Enum → Exploit → Rapport

Ce guide couvre **90% des besoins d’un CTF étudiant**.