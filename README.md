# 🚀 My First DevOps Project — Containerized Website with Full CI/CD

A hands-on DevOps project that takes a simple website all the way from a single HTML file to a **containerized, orchestrated, Helm-packaged application with an automated CI/CD pipeline**. Built to learn the real, industry-standard workflow end to end — the same steps used to ship production software.

![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?logo=helm&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?logo=githubactions&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)

---

## 📋 What this project demonstrates

| Skill | Tool | What I did |
|-------|------|-----------|
| **Containerization** | Docker | Packaged a static site into a portable image on `nginx:alpine` |
| **Container orchestration** | Kubernetes | Deployed with 2 replicas behind a LoadBalancer service |
| **Local multi-container setup** | Docker Compose | Ran the same app declaratively with one command |
| **Package management** | Helm | Templated the deployment into a reusable, versioned chart with rollback |
| **CI/CD automation** | GitHub Actions | Auto-build and publish the image to a registry on every push |

---

## 🏗️ Architecture

```
        Source code (index.html)
                 │
                 ▼
          Dockerfile  ──►  Docker image (nginx:alpine + site)
                 │
        ┌────────┼─────────────────────────┐
        ▼        ▼                          ▼
   docker run   Docker Compose        Kubernetes (Helm)
   :8080        :8082                 :8081  (2 replicas + LoadBalancer)
                 │
                 ▼
         GitHub Actions CI/CD
   push to main ─► build image ─► publish to GHCR
```

The exact same application is deployed **three different ways** to show each tool in isolation, plus a CI/CD pipeline that automates the build.

---

## 📂 Project structure

```
.
├── index.html                       # The website (static page served by nginx)
├── Dockerfile                       # Builds the container image
├── docker-compose.yml               # Runs the app via Docker Compose
├── deployment.yaml                  # Raw Kubernetes manifests (Deployment + Service)
├── mywebsite-chart/                 # Helm chart (the "packaged" version)
│   ├── Chart.yaml                   # Chart metadata
│   ├── values.yaml                  # Config knobs: replicas, image, ports
│   └── templates/
│       ├── deployment.yaml          # Templated Deployment ({{ .Values.* }})
│       └── service.yaml             # Templated Service
└── .github/workflows/
    └── docker-build.yml             # CI/CD: build & push image to GHCR
```

---

## ▶️ How to run it

### 1. Docker (single container)
```bash
docker build -t my-website .
docker run -d -p 8080:80 --name my-site my-website
```
→ Visit **http://localhost:8080**

### 2. Docker Compose
```bash
docker compose up -d
```
→ Visit **http://localhost:8082**

### 3. Kubernetes (raw manifests)
```bash
kubectl apply -f deployment.yaml
kubectl get pods
```
→ Visit **http://localhost:8081**

### 4. Kubernetes with Helm (the packaged way)
```bash
helm install my-website ./mywebsite-chart
helm list
```
→ Visit **http://localhost:8081**

---

## ⎈ Helm highlights — why packaging matters

Instead of hand-editing YAML, the whole deployment is driven by knobs in `values.yaml`:

```bash
# Scale from 2 → 4 replicas with one flag (no file edits)
helm upgrade my-website ./mywebsite-chart --set replicaCount=4

# Made a mistake? Instantly roll back to the previous version
helm rollback my-website 1

# Full audit trail of every change
helm history my-website
```

This is the feature that makes teams trust Helm in production: **a bad deploy is one `helm rollback` away from fixed.**

---

## 🔄 CI/CD pipeline

Every push to `main` triggers a [GitHub Actions](.github/workflows/docker-build.yml) workflow that:

1. Checks out the code
2. Builds the Docker image
3. Publishes it to the **GitHub Container Registry (GHCR)** at `ghcr.io/<owner>/my-website`

No manual builds — push code, get a fresh published image automatically.

---

## 🧠 What I learned

- How a **Dockerfile** turns application code into a portable, reproducible image
- The difference between **running a container** and **orchestrating** many with Kubernetes (replicas, services, self-healing)
- How **Helm** packages Kubernetes apps into versioned, configurable, rollback-able releases
- How **CI/CD** removes manual steps and makes deployments repeatable and reliable
- Real debugging along the way: fixing kubectl contexts, recovering a crashed Docker Desktop, and resolving image/registry issues

---

## 🛠️ Tech stack

**Docker · Kubernetes · Helm · Docker Compose · GitHub Actions · Nginx · Git**

---

> Built as a learning-by-doing project to understand the complete modern deployment pipeline — from a single file to a fully automated, production-style workflow.
