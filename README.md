Yes. Below is the **entire README as a single Markdown code block**.

Just copy everything inside the code block and paste it into your `README.md` file in VS Code. Nothing will break formatting.

---

```markdown
# 📦 MERN Stack Deployment using Kubernetes, Helm & Jenkins CI/CD

## 🚀 Project Overview

This project demonstrates the complete deployment and CI/CD automation of a MERN (MongoDB, Express.js, React.js, Node.js) application using:

- Kubernetes (Docker Desktop Cluster)
- Helm (Kubernetes Package Manager)
- Jenkins (CI/CD automation inside Kubernetes)
- Kaniko (Daemonless container image builder)
- DockerHub (Container registry)
- GitHub (Source control)

### 🎯 Objectives

1. Deploy the MERN application manually using Kubernetes manifests.
2. Convert the deployment into a reusable Helm chart.
3. Implement a Jenkins-based CI/CD pipeline to automate build and deployment.

---

# 🏗️ Architecture

## Application Components

| Component  | Technology           | Purpose            |
|------------|----------------------|-------------------|
| Frontend   | React + NGINX        | User Interface    |
| Backend    | Node.js + Express    | REST API          |
| Database   | MongoDB              | Data Storage      |
| Ingress    | NGINX Ingress        | HTTP Routing      |
| CI/CD      | Jenkins (K8s)        | Automation        |
| Builder    | Kaniko               | Image Build Tool  |

## 🔄 Deployment Flow

```

GitHub → Jenkins Pipeline → Kaniko Build → DockerHub Push → Helm Upgrade → Kubernetes Rolling Update

````

---

# 🧩 Phase 1 – Manual Kubernetes Deployment

## 1️⃣ Namespace Creation

```bash
kubectl create namespace shopnow
````

## 2️⃣ MongoDB Deployment

* MongoDB Deployment created
* ClusterIP service exposed
* Backend connects using:

```
mongodb:27017
```

## 3️⃣ Backend Deployment

* Image: `rushii01/shopnow-backend:<tag>`
* Environment variable:

```bash
MONGODB_URI=mongodb://mongodb:27017/shopnow
```

* Replicas: 2
* Service Type: ClusterIP

## 4️⃣ Frontend Deployment

* Image: `rushii01/shopnow-frontend:<tag>`
* Served using NGINX
* Exposed initially via NodePort

## 5️⃣ Ingress Configuration

* Host: `shopnow.local`
* Routes traffic to frontend service

### ✅ Validation

```bash
kubectl get pods -n shopnow
kubectl get svc -n shopnow
kubectl get ingress -n shopnow
```

Application successfully accessible in browser.

---

# 🧩 Phase 2 – Helm Chart Implementation

Manual Kubernetes YAML files were converted into a Helm chart.

## 📁 Helm Chart Structure

```
shopnow-chart/
│
├── Chart.yaml
├── values.yaml
└── templates/
    ├── namespace.yaml
    ├── mongodb-deployment.yaml
    ├── mongodb-service.yaml
    ├── backend-deployment.yaml
    ├── backend-service.yaml
    ├── frontend-deployment.yaml
    ├── frontend-service.yaml
    └── ingress.yaml
```

## ⚙️ Parameterization (values.yaml)

Configurable values include:

* Image repository
* Image tag
* Replica count
* Service ports
* Ingress enable/disable
* Hostname
* Namespace

Example:

```yaml
backend:
  image:
    repository: rushii01/shopnow-backend
    tag: "3.0"
  replicas: 2
```

## 🚀 Helm Commands

Install:

```bash
helm install shopnow ./shopnow-chart -n shopnow
```

Upgrade:

```bash
helm upgrade shopnow ./shopnow-chart -n shopnow
```

Validation:

```bash
helm list -n shopnow
kubectl get pods -n shopnow
```

---

# 🧩 Phase 3 – Jenkins CI/CD in Kubernetes

Jenkins was deployed inside Kubernetes using the official Helm chart.

## 🔐 Jenkins Setup

* Namespace: `jenkins`
* Installed via Helm
* RBAC configured for deployment into `shopnow`
* DockerHub credentials configured
* Kaniko used for container builds

## 🔑 DockerHub Secret for Kaniko

```bash
kubectl create secret docker-registry dockerhub-secret \
  --namespace jenkins \
  --docker-username=rushii01 \
  --docker-password=*** \
  --docker-email=***
```

This allows Kaniko to securely push images.

---

# 🚀 CI/CD Pipeline Design

Pipeline implemented via `Jenkinsfile` stored in GitHub.

## 🛠 Pipeline Stages

### 1️⃣ Checkout

Clones GitHub repository.

### 2️⃣ Build Backend Image

Kaniko builds using:

```
shopNow/backend/Dockerfile
```

Pushes to:

```
rushii01/shopnow-backend:<BUILD_NUMBER>
```

### 3️⃣ Build Frontend Image

Kaniko builds using:

```
shopNow/frontend/Dockerfile
```

Pushes to:

```
rushii01/shopnow-frontend:<BUILD_NUMBER>
```

### 4️⃣ Helm Upgrade Deployment

```bash
helm upgrade --install shopnow ./shopnow-chart \
  -n shopnow \
  --set backend.image.tag=${BUILD_NUMBER} \
  --set frontend.image.tag=${BUILD_NUMBER}
```

Triggers rolling update.

---

# 🔄 CI/CD Workflow

```
Code Commit
     ↓
Jenkins Trigger
     ↓
Kaniko Build & Push
     ↓
Helm Upgrade
     ↓
Kubernetes Rolling Update
     ↓
Application Updated
```

---

# ⚠ Challenges & Solutions

| Challenge                           | Solution                                  |
| ----------------------------------- | ----------------------------------------- |
| Docker daemon not accessible in K8s | Replaced Docker-in-Docker with Kaniko     |
| Kaniko container exiting            | Used debug image and correct pod template |
| PowerShell compatibility issues     | Adjusted commands for Windows             |

---

# 📊 Technologies Used

* Kubernetes
* Helm
* Jenkins
* Kaniko
* DockerHub
* GitHub
* NGINX Ingress
* Docker Desktop (Windows 11)

---

# ✅ Final Outcome

✔ MERN application deployed using Kubernetes
✔ Helm chart created with parameterized templates
✔ Jenkins deployed inside Kubernetes
✔ CI/CD pipeline implemented
✔ Images built and pushed to DockerHub
✔ Automated Helm upgrade process designed

This project demonstrates strong understanding of Kubernetes deployment, Helm templating, CI/CD automation, and cloud-native container build strategies.

---

# 🔗 Repository

[https://github.com/Rushiargade/mern-shopnow-k8s-helm-cicd](https://github.com/Rushiargade/mern-shopnow-k8s-helm-cicd)

```

---

Now you can:

1. Open `README.md`
2. Delete everything
3. Paste the above content
4. Save
5. `git add .`
6. `git commit -m "Final professional README"`
7. `git push`

If you want, I can now give you a short viva explanation script so you can confidently explain this project.
```
