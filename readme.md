##                     DevOps Assessment - Next.js Application

## Overview
This repository contains a simple **Next.js application** deployed using **Docker**, **GitHub Container Registry (GHCR)**, and **Kubernetes (Minikube)** on an **AWS EC2 instance**.  

The app displays a front page for the DevOps assignment and demonstrates containerization, CI/CD, and Kubernetes deployment.

---

## Repository Structure
.
├── Dockerfile
├── package.json
├── package-lock.json
├── k8s/
│ ├── deployment.yaml
│ └── service.yaml
├── .github/
│ └── workflows/ci.yml
└── README.md


---

## Prerequisites

- AWS EC2 instance (Ubuntu)
- Docker installed
- Minikube installed
- kubectl installed
- GitHub account with GHCR access
- GitHub Personal Access Token (PAT) with `read:packages` and `write:packages` scopes

---
## Prerequisites

- AWS EC2 instance (Ubuntu)
- Docker installed
- Minikube installed
- kubectl installed
- GitHub account with GHCR access
- GitHub Personal Access Token (PAT) with `read:packages` and `write:packages` scopes
---


## Setup Instructions

### 1. Clone Repository

```bash
git clone https://github.com/Gautamkathuria/devops-assessment.git
cd nextjs-devops-assessment

### 2. Build dcoker image
docker build -t ghcr.io/gautamkathuria/nextjs-app:latest .

### 3. Login to GHCR
echo <PAT> | docker login ghcr.io -u gautamkathuria --password-stdin

### 4. Push Docker Image to GHCR
docker push ghcr.io/gautamkathuria/nextjs-app:latest

### Kubernetes Deployment
1. Create Docker Pull Secret (if image is private)
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=gautamkathuria \
  --docker-password=<PAT> \
  --docker-email=kathuriagautam2411@gmail.com

2. Apply Manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

3. Verify Pods & Services
kubectl get pods
kubectl get svc

### Accessing the Application
1: NodePort (Permanent)
If Minikube is started with host networking:
minikube start --driver=none
EC2 public IP: http://<EC2_PUBLIC_IP>:30080

## GitHub Actions Workflow
A workflow (.github/workflows/docker-ghcr.yml) is configured to:
Build Docker image on push to main branch
Push the image to GHCR with proper tagging

### Kubernetes Manifests
## Deployment (k8s/deployment.yaml)
Runs 2 replicas of Next.js app
Uses ghcr-secret for private image pull
Exposes container port 3000

## Service (k8s/service.yaml)
Type: NodePort
Maps container port 3000 → NodePort 30080
Exposes the app externally

### Testing
Check logs of your pods:
kubectl logs -f <pod-name>

Test access:
curl http://<EC2_PUBLIC_IP>:30080

### Submission Details

Repository URL: https://github.com/Gautamkathuria/devOps-Assignment.git

GHCR Image URL:  ghcr.io/gautamkathuria/nextjs-app:latest


Author
Name: Gautam 
Email: kathuriagautam2411@gmail.com
Assessment: DevOps Internship