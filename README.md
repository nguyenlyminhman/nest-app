# 🚀 Nest App – Docker Build & Push for Kubernetes Learning

This project demonstrates how to:

- 🐳 Build a Docker image from a NestJS application  
- ⚙️ Automate build & push using GitHub Actions  
- 📦 Publish image to Docker Hub (public)  
- ☸️ Use the image for Kubernetes deployment  

👉 This repository is designed for **learning Kubernetes end-to-end** (from code → image → deploy)

---

## 📌 Overview
```
Pipeline flow:

Code (NestJS)
   ↓
GitHub Actions (CI)
   ↓
Build Docker Image
   ↓
Push → Docker Hub (public)
   ↓
Pull → Kubernetes Cluster

```

## 🧱 Tech Stack

- **Backend**: NestJS (Node.js)
- **CI/CD**: GitHub Actions
- **Containerization**: Docker
- **Registry**: Docker Hub
- **Orchestration**: Kubernetes (external)

---

## 📂 Project Structure
```
nest-app/
├── .github/
│   └── workflows/
│       └── docker.yml      # CI build & push image
├── src/
├── Dockerfile
├── package.json
└── README.md
```

## ⚙️ CI/CD Workflow

Main file:

.github/workflows/docker.yml

### 🔄 Flow

1. Trigger:
   - push to `main`
   - pull request

2. Pipeline steps:

- Checkout source
- Setup Docker Buildx
- Login Docker Hub
- Build image from Dockerfile
- Push image to Docker Hub

---

## 📌 Example Workflow

```yaml
name: Build & Push Docker Image

on:
  push:
    branches: [ "main" ]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login Docker Hub
        uses: docker/login-action@v4
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build & Push
        uses: docker/build-push-action@v7
        with:
          context: .
          push: true
          tags: <your-dockerhub-username>/nest-app:latest
```

---

## 🔐 Secrets Configuration

Go to:

Settings → Secrets → Actions

Add:

- DOCKER_USERNAME
- DOCKER_PASSWORD (use Docker Hub Access Token)

---

## 🐳 Docker Usage

### Build locally

```bash
docker build -t nest-app .
```

### Run container

```bash
docker run -p 3001:3001 nest-app
```

### Pull from Docker Hub

```bash
docker pull <your-username>/nest-app:latest
```

---

## ☸️ Kubernetes Usage

After pushing image to Docker Hub:

### Deployment example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nest-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nest-app
  template:
    metadata:
      labels:
        app: nest-app
    spec:
      containers:
        - name: nest-app
          image: <your-username>/nest-app:latest
          ports:
            - containerPort: 3001
```

---

## 🎯 Purpose

This is NOT a production-ready app.

Main goals:

- Learn CI/CD pipeline
- Understand Docker image lifecycle
- Practice Kubernetes deployment
- Use as a base for microservices

---

## 📈 Suggested Learning Path

1. Dockerize app
2. Push to Docker Hub
3. Deploy to:
   - Minikube
   - Kind
   - K3s
4. Scale & rolling update
5. Add Ingress / Service

---

## 🔥 Best Practices

- Use version tags (v1, v2, commit SHA)
- Avoid using only `latest`
- Cache Docker layers
- Scan images for vulnerabilities

---

## 🚀 Future Improvements

- [ ] Multi-stage Dockerfile
- [ ] Version tagging (git SHA)
- [ ] Helm chart
- [ ] GitOps (ArgoCD)
- [ ] Healthcheck & readinessProbe

---

## 🤝 Contributing

Feel free to fork and use for your Kubernetes learning journey.

---

## 📄 License

MIT
