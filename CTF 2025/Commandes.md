# 🧰 Exegol – Commandes essentielles (A → H)

> 📌 Fiche **prête à copier-coller dans Obsidian**  
> 🎯 Objectif : comprendre **quoi fait chaque commande** et **quand l’utiliser** en CTF / Pentest

---

## 🟢 A. Commandes Linux de base

```bash
whoami
```

- 👉 Affiche l’utilisateur courant
    
- 🔎 Utile pour vérifier si tu es `root`, `www-data`, etc.
    

```bash
id
```

- 👉 Affiche UID, GID et groupes
    
- 🔥 Indispensable pour détecter des groupes dangereux (`sudo`, `docker`, `lxd`)
    

```bash
pwd
```

- 👉 Affiche le répertoire courant
    
- 📁 Utile pour savoir où tu travailles (workspace Exegol)
    

```bash
ls -la
```

- 👉 Liste fichiers + permissions + fichiers cachés
    
- 🔎 Très utile pour repérer des fichiers sensibles (`.ssh`, `.env`)
    

```bash
cd /chemin
```

- 👉 Changer de répertoire
    
- 📁 Navigation dans le système
    

```bash
cat fichier
```

- 👉 Affiche le contenu d’un fichier
    
- 🔎 Lire config, creds, scripts
    

```bash
less fichier
```

- 👉 Lire un fichier page par page
    
- ✔️ Plus pratique que `cat` pour gros fichiers
    

---

## 🟢 B. Réseau / VPN

```bash
ip a
```

- 👉 Affiche les interfaces réseau
    
- 🔑 Vérifier `tun0` (VPN actif)
    

```bash
ip route
```

- 👉 Affiche la table de routage
    
- 🔎 Vérifier que le trafic lab passe par `tun0`
    

```bash
ifconfig
```

- 👉 Ancienne commande réseau (encore utilisée)
    
- 🔎 Vérifier IP et interfaces
    

```bash
ss -tulpn
```

- 👉 Affiche ports ouverts et services actifs
    
- 🔥 Très utile en post-exploitation
    

```bash
netstat -tulpn
```

- 👉 Alternative à `ss`
    
- 📡 Voir services écoutant
    

---

## 🟢 C. Reconnaissance

```bash
nmap -sC -sV IP
```

- 👉 Scan classique (scripts + versions)
    
- 🎯 Première commande sur une cible
    

```bash
nmap -p- IP
```

- 👉 Scan TOUS les ports (1–65535)
    
- 🔥 Indispensable pour ne rien rater
    

```bash
arp -a
```

- 👉 Affiche les hôtes du réseau local
    
- 🔎 Utile en lab local / pivoting
    

---

## 🟢 D. Web

```bash
curl http://IP
```

- 👉 Requête HTTP simple
    
- 🔎 Tester rapidement un site ou une API
    

```bash
whatweb http://IP
```

- 👉 Identifie les technologies web
    
- 🔍 CMS, serveur, frameworks
    

```bash
gobuster dir -u http://IP -w /opt/resources/wordlists/seclists/Discovery/Web-Content/common.txt
```

- 👉 Bruteforce de répertoires
    
- 🔥 Trouver `/admin`, `/backup`, etc.
    

```bash
nikto -h http://IP
```

- 👉 Scan de vulnérabilités web
    
- ⚠️ Bruyant mais informatif
    

---

## 🟢 E. Exploitation

```bash
searchsploit nom_service
```

- 👉 Recherche exploits connus
    
- 🔎 Toujours après Nmap
    

```bash
msfconsole
```

- 👉 Lancer Metasploit
    
- 💥 Exploitation automatisée
    

```bash
python3 -m http.server 80
```

- 👉 Serveur HTTP local
    
- 🔁 Transfert de fichiers vers la cible
    

```bash
nc -lvnp 4444
```

- 👉 Listener Netcat
    
- 🔥 Réception de reverse shell
    

---

## 🟢 F. Privilege Escalation

```bash
sudo -l
```

- 👉 Liste les droits sudo
    
- 🔥 Commande N°1 pour devenir root
    

```bash
find / -perm -4000 2>/dev/null
```

- 👉 Trouve les binaires SUID
    
- 🔎 Failles privesc fréquentes
    

```bash
getcap -r / 2>/dev/null
```

- 👉 Liste les capabilities Linux
    
- 🔥 Alternative moderne au SUID
    

```bash
crontab -l
```

- 👉 Liste tâches planifiées
    
- 🔎 Scripts exécutés en root
    

```bash
uname -a
```

- 👉 Infos kernel
    
- 🔎 Recherche d’exploits kernel
    

```bash
./linpeas.sh
```

- 👉 Script automatique de privesc
    
- ⚡ Gain de temps énorme
    

---

## 🟢 G. Utilisateurs & permissions

```bash
cat /etc/passwd
```

- 👉 Liste des utilisateurs
    
- 🔎 Identifier comptes intéressants
    

```bash
cat /etc/shadow
```

- 👉 Hashs des mots de passe (root only)
    
- 🔥 Jackpot si accessible
    

```bash
groups
```

- 👉 Groupes de l’utilisateur courant
    
- 🔎 Détecter `docker`, `lxd`, `sudo`
    

---

## 🟢 H. Docker / LXD (Privilege Escalation)

```bash
docker ps
```

- 👉 Liste conteneurs actifs
    
- 🔎 Vérifier accès Docker
    

```bash
docker images
```

- 👉 Liste images Docker
    
- 🔥 Peut mener à root via montage `/`
    

```bash
lxd init
```

- 👉 Initialisation LXD
    
- ⚠️ Souvent exploitable pour root
    

---

## 🧠 Rappel CTF

> 🔁 **Toujours la même logique** :
> 
> 1. Recon (`nmap`)
>     
> 2. Enumération
>     
> 3. Exploitation
>     
> 4. Privilege Escalation
>     

---

🎯 **Cette fiche = base solide pour HTB / THM / OSCP-style**