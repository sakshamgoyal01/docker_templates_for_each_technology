# 🐳 Docker Templates — Multi-Technology Container Blueprints

This repository provides **ready-to-use Docker templates** for different technologies — designed for production-grade DevOps, CI/CD, and Kubernetes environments.

Each template follows **best practices**:

- ✅ Multi-stage builds (small, efficient images)
- ✅ Security-hardened (non-root users, healthchecks)
- ✅ CI/CD and Kubernetes ready
- ✅ Works with Argo CD, Jenkins, and GitHub Actions
- ✅ Fully commented for easy customization

---

## 🧩 Template Overview

| Folder                      | Technology                     | Purpose                     | Highlights                         |
| --------------------------- | ------------------------------ | --------------------------- | ---------------------------------- |
| **frontend-react/**         | React + Vite                   | Frontend web app            | Multi-stage build (node → nginx)   |
| **backend-springboot/**     | Java (Spring Boot)             | Enterprise backend          | JAR build + distroless runtime     |
| **backend-python-fastapi/** | Python (FastAPI)               | Lightweight API             | Uvicorn server, `/health` endpoint |
| **backend-node-express/**   | Node.js / NestJS               | REST API / Microservices    | Alpine image + npm ci build        |
| **backend-go/**             | Go (Gin / Fiber)               | Compiled microservice       | Distroless binary, <20MB           |
| **worker-celery-kafka/**    | Python Celery / Kafka Consumer | Async task processing       | Redis/Kafka integration            |
| **ai-ml-inference/**        | TensorFlow / PyTorch           | Model serving               | GPU / CPU runtime ready            |
| **database-postgresql/**    | PostgreSQL                     | Persistent DB               | Auto-init schema via SQL           |
| **storage-minio/**          | MinIO (S3 compatible)          | Object storage              | For file backups, ML models        |
| **gateway-nginx/**          | NGINX / Envoy                  | API Gateway / Reverse Proxy | Multi-service routing              |
| **monitoring/**             | Prometheus + Grafana           | Metrics & dashboards        | System + app observability         |

---

## 🧱 Usage Example

### 🧰 Build and Run (local)

```bash
cd backend-python-fastapi
docker build -t fastapi-backend:1.0 .
docker run -d -p 8080:8080 fastapi-backend:1.0
```

### ☸️ Deploy to Kubernetes

Each template is **Kubernetes-ready**.
You can use Helm or plain YAML manifests like:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fastapi-backend
spec:
  replicas: 2
  template:
    spec:
      containers:
        - name: fastapi
          image: ghcr.io/org/fastapi-backend:1.0
          ports:
            - containerPort: 8080
```

---

## 🚀 GitHub Actions Integration Example

Add this in `.github/workflows/docker-build.yml`:

```yaml
name: Build and Push Docker Image
on:
  push:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build and Push Image
        run: |
          docker build -t ghcr.io/${{ github.repository }}/backend-python-fastapi:${{ github.sha }} .
          docker push ghcr.io/${{ github.repository }}/backend-python-fastapi:${{ github.sha }}
```

---

## 📊 Monitoring Integration

All templates support Prometheus metrics out of the box.
For example:

- **FastAPI / Flask**: expose `/metrics`
- **Go / Node.js**: use Prometheus client libraries
- **PostgreSQL / NGINX**: use exporters
- **Grafana dashboards** preloaded via `monitoring/`

---

## 🧭 Suggested Use in DevOps Flow

```
[ Developer Code ]
       ↓
[ Docker Template (from this repo) ]
       ↓
[ Build + Scan + Push (CI/CD) ]
       ↓
[ Kubernetes Deployment (ArgoCD) ]
       ↓
[ Monitoring (Prometheus + Grafana) ]
```

```

```
