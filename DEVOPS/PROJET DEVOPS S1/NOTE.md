# Vue d'ensemble de l'architecture

```
Développeur → Git (GitHub/Gitea/GitLab)
                    │
                 Jenkins (Docker)
                    │
    ┌───────────────┼───────────────────┐
    │               │                   │
 Stage 1         Stage 2            Stage 3
 SAST            Build              Scan
 Bandit          Docker             Trivy
 Semgrep         Image              OWASP
 Gitleaks                           
    │               │                   │
    └───────────────┼───────────────────┘
                    │
                 Stage 4 : Cosign (signature)
                    │
                 Stage 5 : Push → Harbor
                    │
                 Stage 6 : Harbor scan policy
                    │
                 Stage 7 : Docker Compose deploy
```

---

# 📁 Structure du projet à créer

```
mon-projet-devsecops/
├── Jenkinsfile
├── README.md
├── docker-compose.yml
├── docker-compose.deploy.yml
├── docs/
│   ├── analyse_risques.md
│   └── rapport_final.md
├── src/
│   ├── app.py
│   └── utils.py
├── tests/
│   └── test_app.py
├── docker/
│   └── Dockerfile
└── scripts/
    ├── scan.sh
    ├── sign.sh
    └── verify.sh
```

---

# 🚀 PHASE 1 — Setup de l'infrastructure (Semaines 1-3)

## Étape 1 : Préparer Ubuntu

bash

```bash
# Mettre à jour le système
sudo apt update && sudo apt upgrade -y

# Installer les dépendances de base
sudo apt install -y \
  curl wget git vim \
  ca-certificates gnupg \
  lsb-release apt-transport-https

# Installer Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Ajouter votre user au groupe docker (évite sudo)
sudo usermod -aG docker $USER
newgrp docker

# Vérifier
docker --version
docker compose version
```

---

## Étape 2 : Déployer Jenkins via Docker

bash

```bash
# Créer les dossiers persistants
mkdir -p ~/devsecops/jenkins_home
mkdir -p ~/devsecops/harbor

# Lancer Jenkins
docker run -d \
  --name jenkins \
  --restart=unless-stopped \
  -p 8080:8080 \
  -p 50000:50000 \
  -v ~/devsecops/jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(which docker):/usr/bin/docker \
  jenkins/jenkins:lts-jdk17

# Récupérer le mot de passe initial
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

> 🌐 Ouvrez `http://localhost:8080` et collez le mot de passe. Choisissez **"Install suggested plugins"** puis créez votre compte admin.


Interface principale d'harbor

![[Pasted image 20260326102308.png]]


# 🔐 PHASE 3 — Sécurisation (Semaines 7-9)

## Étape 1 : Générer les clés Cosign

bash

```bash
# Installer Cosign sur Ubuntu
curl -O -L "https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64"
sudo mv cosign-linux-amd64 /usr/local/bin/cosign
sudo chmod +x /usr/local/bin/cosign

# Générer la paire de clés
cosign generate-key-pair
# → Crée cosign.key (privée) et cosign.pub (publique)

# Uploader cosign.key dans Jenkins Credentials (type: Secret file, ID: cosign-key)
```

![[Pasted image 20260326105629.png]]
## Étape 2 : Script de vérification `scripts/verify.sh`

bash

```bash
#!/bin/bash
IMAGE=$1
echo "🔍 Vérification de la signature de : $IMAGE"

cosign verify \
  --key cosign.pub \
  --insecure-ignore-tlog \
  "$IMAGE" && echo "✅ Signature valide" || { echo "❌ Signature invalide !"; exit 1; }
```

## Étape 3 : Docker Compose de déploiement `docker-compose.deploy.yml`

yaml

```yaml
version: '3.8'

services:
  fastapi-app:
    image: ${HARBOR_URL}/${HARBOR_PROJECT}/${IMAGE_NAME}:${IMAGE_TAG}
    container_name: fastapi-devsecops
    restart: unless-stopped
    ports:
      - "8000:8000"
    environment:
      - ENV=production
    # Sécurité : restrictions Docker
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp
    cap_drop:
      - ALL
    deploy:
      resources:
        limits:
          memory: 256M
          cpus: '0.5'

  # Monitoring optionnel
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9091:9090"
    profiles: ["monitoring"]
```

## Étape 4 : Politique de sécurité Harbor

Dans Harbor, allez dans votre projet → **Policy** :

1. **Vulnerability scanning** : activer le scan automatique à chaque push
2. **Deployment security** : bloquer les images avec vulnérabilités CRITICAL
3. **Tag immutability** : empêcher l'écrasement d'un tag existant
4. **Content Trust** : activer pour n'autoriser que les images signées

![[Pasted image 20260326112536.png]]
---

# 📊 PHASE 4 — Analyse STRIDE simplifiée

Créez `docs/analyse_risques.md` :

markdown

```markdown
# Analyse des Risques STRIDE du Pipeline CI/CD

## Composants analysés
Jenkins → Git Repo → Harbor → Serveur déploiement

| Menace | Composant | Risque | Contre-mesure |
|--------|-----------|--------|---------------|
| **S**poofing | Harbor registry | Fausse image injectée | Cosign signature |
| **T**ampering | Code source | Modification malveillante | Gitleaks + SAST |
| **R**epudiation | Pipeline logs | Nier une action | Audit logs Jenkins |
| **I**nformation Disclosure | Secrets dans code | Fuite credentials | Gitleaks + Jenkins Creds |
| **D**enial of Service | Jenkins | Saturation pipeline | Limites ressources |
| **E**levation of Privilege | Container | Escalade root | no-new-privileges, USER non-root |
```

---