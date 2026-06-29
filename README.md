# 🌍 QuakeWatch - Full DevOps & GitOps Kubernetes Application

## 📌 Overview

QuakeWatch is a Flask-based application for visualizing and managing earthquake data.

This project demonstrates a complete **DevOps + GitOps workflow**, including:

* Containerization with Docker
* Local orchestration with Docker Compose
* Kubernetes deployment on Docker Desktop
* Helm-based packaging
* Auto-scaling using HPA
* Health monitoring using Liveness & Readiness Probes
* Automated cluster checks using CronJobs
* GitOps deployment using ArgoCD

---

## 🧱 Architecture

The system follows a GitOps-driven architecture:

```
GitHub Repository
        ↓
     ArgoCD
        ↓
      Helm Chart
        ↓
   Kubernetes Cluster
```

Application flow:

* Flask app runs in a Docker container
* Deployed via Helm chart
* Managed in Kubernetes as a Deployment
* Exposed using a Service
* Automatically scaled using HPA
* Continuously monitored using probes
* Periodically checked using CronJob

---

## 📁 Project Structure

```
QuakeWatch/
├── app/
│   ├── app.py
│   ├── dashboard.py
│   ├── utils.py
│   ├── requirements.txt
│   ├── static/
│   └── templates/
│
├── helm/
│   └── quakewatch/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           ├── hpa.yaml
│           ├── cronjob.yaml
│           ├── configmap.yaml
│           └── secret.yaml
│
├── argocd/
│   └── application.yaml
│
├── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## 🐳 Docker Setup

### Build Image

```bash
docker build -t meitarle/quake-watch:latest .
```

### Run Locally

```bash
docker run -p 5000:5000 meitarle/quake-watch:latest
```

---

## ☸️ Kubernetes Deployment (Helm)

### Install via Helm

```bash
helm install quakewatch helm/quakewatch -n class4
```

### Upgrade release

```bash
helm upgrade quakewatch helm/quakewatch -n class4
```

---

## 🚀 Access Application

Using port-forward:

```bash
kubectl port-forward service/quake-watch-service 8080:80 -n class4
```

Open:

```
http://localhost:8080
```

---

## 🔄 GitOps (ArgoCD)

ArgoCD automatically syncs the application from GitHub.

### Apply ArgoCD Application

```bash
kubectl apply -f argocd/application.yaml
```

### Check status

```bash
kubectl get applications -n argocd
```

Expected output:

* Sync: Synced
* Health: Healthy

---

## ⚙️ Kubernetes Components

### 📦 Deployment

Manages replicas of the Flask application.

### 🌐 Service

Exposes the application inside the cluster.

### 📈 HPA (Horizontal Pod Autoscaler)

Automatically scales pods based on CPU usage:

* Min replicas: 2
* Max replicas: 5
* Target CPU: 50%

### 🧠 Probes

* Liveness Probe: restarts unhealthy pods
* Readiness Probe: ensures traffic is only sent to ready pods

### ⏱ CronJob

Runs periodic health checks inside the cluster every minute.

### 🔐 ConfigMap & Secret

Stores configuration and sensitive values separately from code.

---

## 🧪 Useful Commands

```bash
kubectl get all -n class4
kubectl get hpa -n class4
kubectl top pods -n class4
kubectl get pods -n argocd
helm list -n class4
```

---

## 📌 Notes

* This project runs on Docker Desktop Kubernetes
* Metrics-server is required for HPA functionality
* HPA may remain stable if CPU load is low (expected behavior)
* ArgoCD enables full GitOps automation (no manual deployments)

---

# 🏁 Summary

This project demonstrates a complete production-style DevOps workflow:

✔ Docker containerization
✔ Kubernetes orchestration
✔ Helm packaging
✔ Auto-scaling (HPA)
✔ Monitoring (Probes + Metrics)
✔ Automation (CronJob)
✔ GitOps (ArgoCD)
