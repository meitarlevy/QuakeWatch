# 🌍 QuakeWatch - Docker & Kubernetes Orchestrated Application

## 📌 Overview
QuakeWatch is a Flask-based application that visualizes and manages earthquake data.

This project demonstrates a complete DevOps workflow including:
- Containerization with Docker
- Local orchestration with Docker Compose
- Kubernetes deployment on Docker Desktop
- Auto-scaling using HPA
- Health monitoring using Liveness & Readiness Probes
- Automated checks using CronJob

---

## 🧱 Architecture

The application runs as:
- A Flask app inside a Docker container
- Deployed on Kubernetes as a Deployment
- Exposed using a Kubernetes Service
- Automatically scaled using HPA
- Monitored using Kubernetes Probes
- Periodically checked using a CronJob inside the cluster

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
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   ├── hpa.yaml
│   └── cronjob.yaml
├── Dockerfile
├── docker-compose.yml
└── README.md

````

---

## 🐳 Docker Setup

### Build Docker Image
```bash
docker build -t meitarle/quake-watch:latest .
````

### Run Locally

```bash
docker run -p 5000:5000 meitarle/quake-watch:latest
```

---

## ☸️ Kubernetes Deployment

### Apply all Kubernetes resources

```bash
kubectl apply -f k8s/
```

---

## 🚀 Access the Application

Since this project runs on Docker Desktop Kubernetes:

```bash
kubectl port-forward service/quake-watch-service 8080:80
```

Then open in browser:

```
http://localhost:8080
```

---

## ⚙️ Kubernetes Components

### 📦 Deployment

Manages application pods and ensures high availability using replicas.

---

### 🌐 Service

Exposes the application inside the cluster and via NodePort/port-forward.

---

### ⚙️ ConfigMap

Stores non-sensitive configuration values for the application.

---

### 🔐 Secret

Stores sensitive data such as API keys or credentials.

---

### 🧠 Liveness & Readiness Probes

Ensures application health:

* Readiness Probe: ensures pod is ready to receive traffic
* Liveness Probe: restarts pod if application becomes unhealthy

---

### 📈 Horizontal Pod Autoscaler (HPA)

Automatically scales pods based on CPU usage:

* Minimum replicas: 2
* Maximum replicas: 5
* CPU utilization threshold: 50%

---

### ⏱ CronJob

Runs periodic health checks inside the cluster:

* Executes every 1 minute
* Sends HTTP request to the service
* Logs response status

---

## 🧪 Useful Kubernetes Commands

```bash
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get hpa
kubectl get cronjob
kubectl top pods
```

---

## 📌 Notes

* The project runs on Docker Desktop Kubernetes
* Use `kubectl port-forward` instead of NodePort when accessing locally
* Metrics-server is required for HPA functionality
* HPA may not scale if CPU usage is low (this is expected behavior)

