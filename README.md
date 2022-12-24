# sdu-evac

Watch the video here: https://youtu.be/frBWr6k9xoI

## Playbook 1

### Build container images

```console
docker build -f Frontend/Dockerfile --no-cache -t sdu-evac-frontend ./Frontend
docker build -f Backend/Dockerfile --no-cache -t sdu-evac-backend ./Backend
```

### Run local registry

```console
docker run -d -p 5001:5000 --restart=always --name registry registry:2
```

### Tag/push images

```console
docker tag sdu-evac-frontend localhost:5001/sdu-evac-frontend:v0.1.0
docker push localhost:5001/sdu-evac-frontend:v0.1.0
docker tag sdu-evac-backend localhost:5001/sdu-evac-backend:v1.0.0
docker push localhost:5001/sdu-evac-backend:v1.0.0
```

### Install mongodb / redis

```console
helm repo add bitnami https://charts.bitnami.com/bitnami
```

```console
helm install mongodb bitnami/mongodb \
    --set image.repository=arm64v8/mongo \
    --set image.tag=latest \
    --set persistence.mountPath=/data/db
```

```console
helm install redis bitnami/redis \
    --set architecture=standalone \
    --set auth.enabled=false
```

### Apply manifests

```console
kubectl apply -f ./manifests
```

## Playbook 2

### Terraform apply

```console
terraform -chdir=terraform init
terraform -chdir=terraform apply
```
