

---->Émule les services AWS localement (gratuit)
---->Parfait pour tester Terraform avec AWS sans frais
---->Nécessite Docker sur votre machine

Source :  https://github.com/localstack/localstack

# AWS Local avec LocalStack & Terraform

> [!abstract] Objectif Simuler un environnement Cloud AWS sur sa machine de manière fluide et rapide pour les TP de L3 DevOps.

## 1. Démarrage de l'environnement (Docker)

On utilise Docker pour isoler LocalStack.

- **Lancer le service** :
    

Bash

```
docker compose up -d
```

- **Vérifier le statut** :
    

Bash

```
curl http://localhost:4566/_localstack/health
```

---

## 2. Configuration des accès (CLI)

Terraform a besoin de credentials par défaut pour ne pas bloquer.

Bash

```
aws configure set aws_access_key_id test
aws configure set aws_secret_access_key test
aws configure set region us-east-1
```

---

## 3. Configuration Terraform (`main.tf`)

Voici le bloc optimal pour forcer Terraform à ignorer le vrai Cloud Amazon.

> [!warning] Point de vigilance L'option `s3_use_path_style = true` est obligatoire pour éviter les timeouts en local.

Terraform

```
provider "aws" {
  region                      = "us-east-1"
  access_key                  = "test"
  secret_key                  = "test"
  
  skip_credentials_validation = true
  skip_metadata_api_check     = true
  skip_requesting_account_id  = true
  s3_use_path_style           = true 

  endpoints {
    s3  = "http://localhost:4566"
    ec2 = "http://localhost:4566"
    iam = "http://localhost:4566"
  }
}

resource "aws_s3_bucket" "mon_bucket" {
  bucket = "mon-super-projet-l3"
}
```

---

## 4. Déploiement

Workflow standard pour appliquer les changements :

1. **Initialiser** : `terraform init`
    
2. **Appliquer** : `terraform apply -auto-approve`
    

> [!tip] En cas de bug Si Terraform est perdu ("No changes" alors que rien n'existe), supprime le fichier d'état : `rm terraform.tfstate`

---

## 5. Vérification

Vérifier que le bucket est bien présent dans le simulateur :

Bash

```
aws --endpoint-url=http://localhost:4566 s3 ls
```