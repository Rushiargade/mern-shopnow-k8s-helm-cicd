
📌 MERN Kubernetes Helm Jenkins CI/CD Project
Phase 1 – Manual Kubernetes Deployment

Created namespace shopnow

Deployed MongoDB, Backend, Frontend

Created Services

Configured Ingress

Application accessible via http://shopnow.local

Phase 2 – Helm Chart Implementation

Parameterized:

image repository

image tag

replica counts

ingress enable/disable

Used helm install and helm upgrade

Verified rollout success

Phase 3 – Jenkins Deployment in Kubernetes

Installed Jenkins via Helm

Configured RBAC

Created custom Jenkins Docker image

Integrated DockerHub credentials

Implemented Kubernetes-based CI/CD pipeline

CI/CD Design

Pipeline stages:

Checkout

Build Backend Image

Build Frontend Image

Push to DockerHub

Helm upgrade deployment

Tools Used

Kubernetes (Docker Desktop)

Helm

Jenkins (Kubernetes plugin)

Kaniko (for container builds)

DockerHub

GitHub