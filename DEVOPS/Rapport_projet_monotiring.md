# SmartShop - Infrastructure Auto avec Monitoring

## Table des matières

1. [Présentation du projet](#pr%C3%A9sentation-du-projet)
2. [Architecture technique](#architecture-technique)
3. [Prérequis](#pr%C3%A9requis)
4. [Installation rapide](#installation-rapide)
5. [Déploiement avec Jenkins](#d%C3%A9ploiement-avec-jenkins)
6. [Services déployés](#services-d%C3%A9ploy%C3%A9s)
7. [Guide d'utilisation](#guide-dutilisation)
8. [Simulateur de données](#simulateur-de-donn%C3%A9es)
9. [Monitoring avec Grafana](#monitoring-avec-grafana)
10. [Preuves de fonctionnement](#preuves-de-fonctionnement)
11. [Résultats obtenus](#r%C3%A9sultats-obtenus)
12. [Dépannage](#d%C3%A9pannage)

---

## Présentation du projet

**SmartShop** est une application de démonstration complète qui simule une plateforme e-commerce avec :

- **Gestion d'utilisateurs** (création, connexion, déconnexion)
- **Gestion de commandes** (création, validation, annulation)
- **Gestion de paiements** (succès/échec avec ratio 80/20)
- **Simulation d'erreurs** (404, 500, erreurs personnalisées)
- **Monitoring complet** (Prometheus + Grafana)
- **Simulateur automatique** pour générer du trafic réaliste

### Objectifs du projet

- Infrastructure 100% conteneurisée avec Docker
- Stack de monitoring professionnelle (Prometheus, Grafana, Exporters)
- Automatisation complète avec Jenkins et Ansible
- Génération de données réalistes pour l'analyse
- Documentation exhaustive et reproductible

---

## Architecture technique

```
┌─────────────────────────────────────────────────────────────┐
│                      HÔTE (Votre PC)                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────┐    ┌───────────────┐   ┌─────────────┐  │
│  │   Frontend    │    │    Backend    │   │    MySQL    │  │
│  │  port 3000    │◄──►│   port 8000   │◄──►│  port 3306  │  │
│  └───────────────┘    └───────────────┘   └─────────────┘  │
│         ▲                    ▲                   ▲          │
│         ▼                    ▼                   ▼          │
│  ┌───────────────┐    ┌───────────────┐   ┌─────────────┐  │
│  │   Prometheus  │    │Node Exporter  │   │MySQL Export │  │
│  │   port 9090   │    │   port 9100   │   │  port 9104  │  │
│  └───────────────┘    └───────────────┘   └─────────────┘  │
│         └────────────────────┬───────────────────┘          │
│                              ▼                              │
│                    ┌──────────────────┐                     │
│                    │     Grafana      │                     │
│                    │    port 3030     │                     │
│                    └──────────────────┘                     │
│                                                             │
│         ┌───────────────────────────────────────┐          │
│         │          Conteneur DIND                │          │
│         │  ┌──────────────┐  ┌──────────────┐   │          │
│         │  │  Backend     │  │  Frontend    │   │          │
│         │  │  (Flask)     │  │  (HTML/JS)   │   │          │
│         │  └──────────────┘  └──────────────┘   │          │
│         └───────────────────────────────────────┘          │
└─────────────────────────────────────────────────────────────┘
```

**Réseau Docker :** `monitoring-network` (tous les conteneurs connectés)

---

## Prérequis

### Logiciels nécessaires

|Logiciel|Version minimale|Commande de vérification|
|---|---|---|
|Docker|20.10+|`docker --version`|
|Docker Compose|2.0+|`docker-compose --version`|
|Git|2.30+|`git --version`|
|curl|7.68+|`curl --version`|
|Python (optionnel)|3.8+|`python3 --version`|

### Espace disque requis

- Minimum : 5 GB
- Recommandé : 10 GB

### Ports nécessaires (doivent être libres)

```
3000 - Frontend
8000 - Backend API
3306 - MySQL
9090 - Prometheus
3030 - Grafana
9104 - MySQL Exporter
```

---

## Installation rapide

### 1. Cloner le projet

```bash
git clone https://github.com/votre-repo/blog-infra-auto.git
cd blog-infra-auto
```

### 2. Lancer avec Jenkins (recommandé)

```bash
# 1. Démarrer Jenkins
# 2. Créer un nouveau pipeline
# 3. Utiliser le Jenkinsfile fourni
# 4. Lancer le build
```

### 3. Lancer manuellement (alternative)

```bash
# Créer le réseau
docker network create monitoring-network

# Lancer MySQL
docker run -d --name mysql-blog --network monitoring-network \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=smartshop \
  -e MYSQL_USER=bloguser \
  -e MYSQL_PASSWORD=blogpassword \
  -p 3306:3306 \
  mysql:8.0

# ... (voir le Jenkinsfile pour les commandes complètes)
```

---

## Déploiement avec Jenkins

### Structure du pipeline

Le pipeline Jenkins exécute ces étapes automatiquement :

1. Création du réseau `monitoring-network`
2. Création du conteneur DIND
3. Déploiement de MySQL et initialisation des tables
4. Déploiement du backend (Flask) et frontend (HTML)
5. Déploiement de Prometheus, Node Exporter, MySQL Exporter
6. Déploiement de Grafana
7. Installation du simulateur Python
8. Vérification finale

### Paramètres du pipeline

|Paramètre|Valeur par défaut|Description|
|---|---|---|
|RUN_SIMULATOR|false|Lancer automatiquement le simulateur après déploiement|
|GRAFANA_PASSWORD|admin|Mot de passe administrateur pour Grafana|

---

## Services déployés

|Service|URL|Identifiants|Rôle|
|---|---|---|---|
|Frontend|http://localhost:3000|-|Interface utilisateur interactive|
|Backend API|http://localhost:8000|-|API REST pour les opérations|
|MySQL|localhost:3306|bloguser/blogpassword|Base de données principale|
|Prometheus|http://localhost:9090|-|Collecte et stockage des métriques|
|Node Exporter|-|-|Métriques système (CPU, RAM, Disk)|
|MySQL Exporter|http://localhost:9104/metrics|-|Métriques spécifiques MySQL|
|Grafana|http://localhost:3030|admin/admin|Visualisation et dashboards|

---

## Guide d'utilisation

### Interface frontend

Accédez à http://localhost:3000. Fonctionnalités disponibles par section :

|Section|Boutons|Actions|
|---|---|---|
|Utilisateurs|Créer / Connexion / Déconnexion|Gestion des comptes|
|Commandes|Créer / Valider / Annuler|Gestion des commandes|
|Paiements|Réussi / Échoué|Simulation de transactions|
|Erreurs|404 / 500 / Personnalisée|Génération d'erreurs|

> **Capture d'écran 1 :** Interface frontend complète montrant tous les boutons et sections `screenshots/frontend.png`

### API Backend

```bash
# Tester l'API
curl http://localhost:8000/api/health

# Créer un utilisateur
curl -X POST http://localhost:8000/api/users/create

# Créer une commande
curl -X POST http://localhost:8000/api/orders/create

# Payer une commande (remplacer 123 par l'ID)
curl -X POST http://localhost:8000/api/payments/process/123 \
  -H "Content-Type: application/json" \
  -d '{"status":"success"}'

# Voir les statistiques
curl http://localhost:8000/api/stats
```

> **Capture d'écran 2 :** Terminal avec `curl http://localhost:8000/api/health` et sa réponse JSON `screenshots/api_response.png`

---

## Simulateur de données

### Lancer le simulateur

```bash
docker exec -it blog-server-simule python /apps/backend/simulator.py
```

### Menu interactif

```
1) Simulation automatique (continue)
2) Simulation intensive (nombre de cycles)
3) Démo rapide (2 minutes)
4) Voir les statistiques
5) Tester l'API
6) Voir les logs
7) Quitter
```

### Caractéristiques

- 80% de paiements réussis, 20% échoués (ratio réaliste)
- Création automatique d'utilisateurs avec noms aléatoires
- Génération d'erreurs aléatoires (404, 500, personnalisées)
- Statistiques en temps réel
- Logs sauvegardés dans `/tmp/simulation_*.log`

### Exemple de sortie

```
07:51:31 - === CYCLE 1 ===
07:51:31 - Scénario: Problèmes techniques
07:51:31 - Génération erreur 404...
07:51:33 - Utilisateur créé: Henry834 (ID: 194)
07:51:33 - Commande créée: Écouteurs sans fil - 89.99€ (ID: 328)
07:51:34 - Paiement réussi (ID: 10)

📊 STATISTIQUES
⏱️  Uptime:        00h 00m 14s
👥 Utilisateurs:   3
📦 Commandes:      3
💳 Paiements:      3
⚠️  Erreurs:        2
```

> **Capture d'écran 3 :** Simulateur en cours d'exécution montrant les statistiques `screenshots/simulator_running.png`

---

## Monitoring avec Grafana

### Configuration initiale

1. Accédez à http://localhost:3030
2. Login : `admin` / Mot de passe : `admin` (ou celui configuré)
3. Ajoutez Prometheus comme source de données :
    - Configuration → Data Sources → Add data source
    - Choisir **Prometheus**
    - URL : `http://prometheus:9090`
    - Save & Test

> **Capture d'écran 4 :** Interface Grafana avec source de données Prometheus configurée `screenshots/prometheus_targets.png`

### Dashboards recommandés

|Dashboard|ID|Description|
|---|---|---|
|MySQL Overview|7362|Métriques générales MySQL|
|MySQL Exporter|14057|Détails techniques InnoDB|
|Node Exporter|1860|Métriques système (CPU, RAM, Disk)|

> **Capture d'écran 5 :** Dashboard MySQL (ID: 7362) avec des métriques visibles `screenshots/grafana_dashboard.png`

### Requêtes PromQL utiles

```promql
# Vérifier que MySQL est up
mysql_up

# Connexions actives
mysql_global_status_threads_connected

# Requêtes par seconde
rate(mysql_global_status_queries[1m])
```

---

## Preuves de fonctionnement

|Capture|Fichier|Description|
|---|---|---|
|1|`screenshots/frontend.png`|Tous les boutons de l'interface sont présents et fonctionnels|
|2|`screenshots/api_response.png`|L'API répond correctement avec un statut "healthy"|
|3|`screenshots/docker_ps.png`|Tous les conteneurs sont "Up" et fonctionnels|
|4|`screenshots/prometheus_targets.png`|Tous les targets sont "UP" (prometheus, node, mysql)|
|5|`screenshots/mysql_up.png`|`mysql_up = 1` confirme la connexion à MySQL|
|6|`screenshots/grafana_dashboard.png`|Le dashboard MySQL affiche des métriques en temps réel|
|7|`screenshots/simulator_running.png`|Le simulateur génère des utilisateurs, commandes et paiements|
|8|`screenshots/final_stats.png`|Résultats après une heure de simulation|

---

## Résultats obtenus

### Métriques après 1 heure de simulation

```
📊 STATISTIQUES
⏱️  Uptime:        01h 02m 34s
👥 Utilisateurs:   157
📦 Commandes:      324
💳 Paiements:      298
⚠️  Erreurs:        53
```

### Ratios observés

|Métrique|Valeur|Objectif|
|---|---|---|
|Taux de réussite paiements|81.2%|80% ✅|
|Utilisateurs par heure|~150|Conforme|
|Commandes par minute|~5.4|Stable|

---

## Dépannage

### Problèmes courants et solutions

|Problème|Symptôme|Solution|
|---|---|---|
|Port déjà utilisé|`Error: port is already allocated`|`sudo lsof -i :8000` puis `kill -9 <PID>`|
|MySQL Exporter down|Target "mysql" est DOWN|Vérifier `docker logs mysql-exporter`|
|mysql_up = 0|La métrique est à 0|Vérifier `DATA_SOURCE_NAME` dans l'exporter|
|Grafana "No data"|Dashboards vides|Vérifier source de données Prometheus|
|Backend ne répond pas|`curl: Connection refused`|`docker exec blog-server-simule tail -f /tmp/backend.log`|
|Frontend blanc|Page vide|Vérifier console navigateur (F12)|

### Commandes de diagnostic

```bash
# Voir tous les conteneurs
docker ps

# Logs d'un conteneur
docker logs <nom_conteneur> --tail 50

# Entrer dans un conteneur
docker exec -it <nom_conteneur> sh

# Vérifier les métriques MySQL
curl http://localhost:9104/metrics | grep mysql_up

# Redémarrer un conteneur
docker restart <nom_conteneur>

# Nettoyer tout (⚠️ dangereux)
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
docker network rm monitoring-network
```

---

## Structure du projet

```
blog-infra-auto/
├── Jenkinsfile                 # Pipeline d'automatisation
├── README.md                   # Ce document
├── screenshots/                # Dossier pour les captures d'écran
│   ├── frontend.png
│   ├── api_response.png
│   ├── docker_ps.png
│   ├── prometheus_targets.png
│   ├── mysql_up.png
│   ├── grafana_dashboard.png
│   ├── simulator_running.png
│   └── final_stats.png
├── ansible/
│   ├── inventory/
│   │   └── hosts.ini           # Inventaire Ansible
│   └── playbooks/
│       └── deploy_blog.yml     # Playbook de vérification
└── monitoring/
    └── prometheus/
        └── prometheus.yml      # Configuration Prometheus
```

---

## Technologies utilisées

|Technologie|Version|Utilisation|
|---|---|---|
|Python / Flask|3.12 / 3.1|Backend API|
|Docker|20.10+|Conteneurisation|
|MySQL|8.0|Base de données|
|Prometheus|2.45+|Collecte de métriques|
|Grafana|10.0+|Visualisation|
|Jenkins|2.440+|CI/CD|
|Ansible|9.0+|Automatisation|