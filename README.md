# Criando um Pipeline de Deploy de uma Aplicação Utilizando Gitlab, Docker e Kubernetes

### 🛠 REPOSITÓRIO EM CONSTRUÇÃO, PRECISO ESTUDAR MAIS KUBERNETES... 🛠

## Imagem Docker

```docker
docker build -t minha-app-node .
```

## Rodar o Container

```docker
docker run -p 3000:3000 minha-app-node
```

## Push para o Github

```bash
git checkout -b docker-deploy
git add Dockerfile
git commit -m "Add Dockerfile"
git push origin docker-deploy
```

## Manifests Kubernetes

> **deployment.yml**

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: usuario/minha-app-node:latest
        ports:
        - containerPort: 3000
```

> **service.yml**

```YAML
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

---

### Kubernetes

```Bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```
