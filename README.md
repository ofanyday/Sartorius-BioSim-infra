# 1. Setup infra

### The following dependencies are required on the deployer host:

- Terraform
- Terragrunt
- kubectl
- helm
- aws-iam-authenticator

### AWS requirements
- At least one AWS account
- awscli configured (see installation instructions) to access your AWS account.
- A route53 hosted zone if you plan to use external-dns or cert-manager but it is not a hard requirement.

### Start a new cluster - EKS
```
 git clone https://github.com/anydaytmn/Sartorius-BioSim-infra.git
 ```
 
### Running Terragrunt command

```
cd vpc
terragrunt init
terragrunt apply
```
```
cd eks 
terragrunt init
terragrunt apply
```
```
cd eks-addinns
terragrunt init
terragrunt apply
```
### 2. Install services Sartorius

##### Backend
```
git clone https://github.com/kanda-soft/Sartorius-BioSim-Dev-Backend.git
```
```
helm upgrade --install -n dev $SERVICE_NAME ./helm -f ./helm/values-dev.yml \
          --set releaseOverride=$SERVICE_NAME \
          --set image.repository="$REGISTRY_HOST/$SERVICE_NAME" \
          --set image.tag=
```
example github actions
```
https://github.com/kanda-soft/Sartorius-BioSim-Dev-Backend/tree/develop/.github/workflows
```
Configuration backend
```
  FLASK_ENV: production
  FLASK_APP: app.py
  DB_HOST: 
  DB_PORT: "5432"
  DB_NAME: postgres
  DB_USER: postgres
  DB_PASSWORD: 
  SECRET_KEY: secret
  DEBUG: "True"
  S3_ENDPOINT: ""
  MINIO_ROOT_PASSWORD: ""
  MINIO_ROOT_USER: ""
  S3_ACCESS_KEY: ""
  S3_SECRET_KEY: ""
  BUCKET_NAME: "dev-sartorius"
  CELERY_BROKER_URL: "redis://redis:6379/0"
  FLOWER_PORT: "5555"
  FLOWER_BASIC_AUTH: "admin:admin"
```
##### Frontend
```
git clone https://github.com/kanda-soft/Sartorius-BioSim-Dev.git
```
```
helm upgrade --install -n dev $SERVICE_NAME ./helm -f ./helm/values-dev.yml \
          --set releaseOverride=$SERVICE_NAME \
          --set image.repository="$REGISTRY_HOST/$SERVICE_NAME" \
          --set image.tag=
```
example github actions
```
https://github.com/kanda-soft/Sartorius-BioSim-Dev/tree/develop/.github/workflows
```
Configuration frontend (.env)
.
```
REACT_APP_BACKEND_URL=http://api-dev.sartorius.local
```


