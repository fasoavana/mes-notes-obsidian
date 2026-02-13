# 🚀 Leçon complète : Pousser des images Docker vers GHCR & OCIR

## 1) GitHub Container Registry (GHCR)
Prérequis : Compte GitHub, Docker installé, image prête.  
Créer un token GitHub (PAT) avec write:packages et read:packages depuis Settings → Developer settings → Personal access tokens.  

### Étapes GHCR :
echo "<GITHUB_PAT>" | docker login ghcr.io -u <GITHUB_USER> --password-stdin  
cat > Dockerfile <<'EOF'  
FROM alpine:3.20  
RUN apk add --no-cache curl  
CMD ["sh", "-c", "echo Hello GHCR && sleep 3600"]  
EOF  
docker build -t demo-ghcr:latest .  
docker tag demo-ghcr:latest ghcr.io/<GITHUB_USER>/demo-ghcr:1.0.0  
docker push ghcr.io/<GITHUB_USER>/demo-ghcr:1.0.0  
docker pull ghcr.io/<GITHUB_USER>/demo-ghcr:1.0.0  

---

## 2) Oracle Cloud Infrastructure Registry (OCIR)
Prérequis : Compte OCI, namespace (Object Storage Namespace), Docker installé, Auth Token OCI généré via User Settings → Auth Tokens.  
Endpoints régionaux : iad.ocir.io (Ashburn), fra.ocir.io (Francfort), mar.ocir.io (Marseille).  
Format d’image : <region>.ocir.io/<namespace>/<repo>:<tag>  

### Étapes OCIR :
echo "<OCI_AUTH_TOKEN>" | docker login <region>.ocir.io -u "<namespace>/<user_oci>" --password-stdin  
cat > Dockerfile <<'EOF'  
FROM alpine:3.20  
RUN apk add --no-cache curl  
CMD ["sh", "-c", "echo Hello OCIR && sleep 3600"]  
EOF  
docker build -t demo-ocir:latest .  
docker tag demo-ocir:latest <region>.ocir.io/<namespace>/apps/demo-ocir:1.0.0  
docker push <region>.ocir.io/<namespace>/apps/demo-ocir:1.0.0  
docker pull <region>.ocir.io/<namespace>/apps/demo-ocir:1.0.0  

---

## 3) Tableau comparatif
| Aspect           | GHCR (GitHub)                       | OCIR (Oracle)                                |
|------------------|--------------------------------------|-----------------------------------------------|
| Hôte             | ghcr.io                             | <region>.ocir.io (ex: iad.ocir.io)            |
| Authentification | PAT GitHub (write:packages)          | OCI Auth Token                                |
| Nom d’image      | ghcr.io/<user>/<image>:tag           | <region>.ocir.io/<namespace>/<repo>:tag       |
| Création repo    | Auto au push                        | Auto ou via console                           |
| Gestion accès    | Teams/Orgs GitHub                   | Compartments + IAM Policies                   |
| Visibilité       | Public/Private/Internal             | Contrôlé via IAM                              |

---

## 4) Bonnes pratiques
Ne pas exposer vos tokens → utiliser des variables d’environnement ou secrets.  
Publier plusieurs tags utiles :  
docker tag app:1.0.0 ghcr.io/org/app:latest  
docker push ghcr.io/org/app:1.0.0  
docker push ghcr.io/org/app:latest  
Utiliser des images minimales (alpine, multi-stage builds).  
Automatiser avec GitHub Actions (GHCR) ou OCI DevOps (OCIR).  

---

## 5) Dépannage
Erreur "denied" sur GHCR → vérifier PAT et organisation.  
Erreur "unauthorized" → refaire docker login (bon host + bon token).  
Erreur "name unknown" → vérifier namespace/region pour OCIR, visibilité pour GHCR.  
OCIR login échoue → username doit être "<namespace>/<user>".  

---

## 6) Exemples complets
### GHCR
GITHUB_USER="<user>"  
GITHUB_PAT="<token>"  
docker build -t demo-ghcr:latest .  
echo "$GITHUB_PAT" | docker login ghcr.io -u "$GITHUB_USER" --password-stdin  
docker tag demo-ghcr:latest ghcr.io/$GITHUB_USER/demo-ghcr:1.0.0  
docker push ghcr.io/$GITHUB_USER/demo-ghcr:1.0.0  

### OCIR
REGION_HOST="iad.ocir.io"  
NAMESPACE="<namespace>"  
OCI_USER="<oci_user>"  
OCI_AUTH_TOKEN="<oci_token>"  
docker build -t demo-ocir:latest .  
echo "$OCI_AUTH_TOKEN" | docker login $REGION_HOST -u "$NAMESPACE/$OCI_USER" --password-stdin  
docker tag demo-ocir:latest $REGION_HOST/$NAMESPACE/apps/demo-ocir:1.0.0  
docker push $REGION_HOST/$NAMESPACE/apps/demo-ocir:1.0.0  

---

## 7) CI/CD avec GitHub Actions
### GHCR
name: Publish to GHCR  
on:  
  push:  
    tags:  
      - "v*.*.*"  
jobs:  
  build-and-push:  
    runs-on: ubuntu-latest  
    permissions:  
      contents: read  
      packages: write  
    steps:  
      - uses: actions/checkout@v4  
      - run: echo "${{ secrets.GITHUB_TOKEN }}" | docker login ghcr.io -u ${{ github.actor }} --password-stdin  
      - run: |  
          IMAGE=ghcr.io/${{ github.repository_owner }}/demo-ghcr  
          TAG=${GITHUB_REF_NAME#v}  
          docker build -t $IMAGE:$TAG .  
          docker push $IMAGE:$TAG  

### OCIR
name: Publish to OCIR  
on:  
  push:  
    tags:  
      - "v*.*.*"  
jobs:  
  build-and-push:  
    runs-on: ubuntu-latest  
    steps:  
      - uses: actions/checkout@v4  
      - run: |  
          echo "${{ secrets.OCI_AUTH_TOKEN }}" | docker login "${{ secrets.OCI_REGION_HOST }}" -u "${{ secrets.OCI_NAMESPACE }}/${{ secrets.OCI_USER }}" --password-stdin  
          IMAGE="${{ secrets.OCI_REGION_HOST }}/${{ secrets.OCI_NAMESPACE }}/apps/demo-ocir"  
          TAG=${GITHUB_REF_NAME#v}  
          docker build -t demo-ocir:latest .  
          docker tag demo-ocir:latest $IMAGE:$TAG  
          docker push $IMAGE:$TAG  

---

## 8) Résumé rapide
### GHCR
echo "$GITHUB_PAT" | docker login ghcr.io -u "$GITHUB_USER" --password-stdin  
docker build -t app:latest .  
docker tag app:latest ghcr.io/$GITHUB_USER/app:1.0.0  
docker push ghcr.io/$GITHUB_USER/app:1.0.0  

### OCIR
echo "$OCI_AUTH_TOKEN" | docker login <region>.ocir.io -u "<namespace>/<user>" --password-stdin  
docker build -t app:latest .  
docker tag app:latest <region>.ocir.io/<namespace>/<repo>:1.0.0  
docker push <region>.ocir.io/<namespace>/<repo>:1.0.0  
