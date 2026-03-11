# <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" width="30"> Kubernetes Node.js Application Deployment

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)

## 📋 Project Overview

This project demonstrates deploying a Node.js application on Kubernetes cluster running on AWS EC2 instances. The infrastructure consists of master and worker nodes with various Kubernetes manifests for pods, replica sets, deployments, and services.

---


┌─────────────────────────────────────────────────────┐
│                   Internet                          │
│                          │                          │
│                          ▼                          │
│              ┌─────────────────────┐                │
│              │  NodePort: 30003    │                │
│              └──────────┬──────────┘                │
│                         │                           │
│         ┌───────────────┼───────────────┐           │
│         ▼               ▼               ▼           │
│   ┌────────────┐   ┌────────────┐   ┌────────────┐  │
│   │  Worker    │   │  Worker    │   │  Master    │  │
│   │  Node 1    │   │  Node 2    │   │  Node      │  │
│   │            │   │            │   │            │  │
│   │ ┌────────┐ │   │ ┌────────┐ │   │ ┌────────┐ │  │
│   │ │Pod 1   │ │   │ │Pod 3   │ │   │ │Control │ │  │
│   │ │Port    │ │   │ │Port    │ │   │ │Plane   │ │  │
│   │ │8000    │ │   │ │8000    │ │   │ │        │ │  │
│   │ └────────┘ │   │ └────────┘ │   │ └────────┘ │  │
│   │ ┌────────┐ │   │ ┌────────┐ │   │            │  │
│   │ │Pod 2   │ │   │ │Pod 4   │ │   │            │  │
│   │ │Port    │ │   │ │Port    │ │   │            │  │
│   │ │8000    │ │   │ │8000    │ │   │            │  │
│   │ └────────┘ │   │ └────────┘ │   │            │  │
│   └────────────┘   └────────────┘   └────────────┘  │
└─────────────────────────────────────────────────────┘

---

## 🖥️ AWS EC2 Instances

<div align="center">
  <img src="Images/Instances.jpg" alt="AWS EC2 Instances" width="900">
  <p><em>Figure 1: AWS EC2 Console showing Kubernetes cluster nodes</em></p>
</div>

### EC2 Instance Details

| Instance Name | Instance ID | State | Instance Type | Status |
|--------------|-------------|-------|---------------|--------|
| **Master-node** | `i-0fed2709937dcc8ef` | **Running** | `m7i-flex.large` | ✅ Active |
| **Worker-node** | `i-09c4660e4de25f933` | **Running** | `m7i-flex.large` | ✅ Active |

> **Note:** Both instances are running with `m7i-flex.large` type, providing optimal performance for the Kubernetes cluster.

---

## 🌐 Application Web Interface

<div align="center">
  <img src="Images/web-page.jpg" alt="Node.js Todo Application" width="600">
  <p><em>Figure 2: Todo List Application  </em></p>
</div>

The application is a simple **Todo List** with the message:
- "Hi I am cloud engineer"
- "What should I do?" input field
- "Add" button for adding tasks

---

🚀 Deployment Commands
bash
# Create namespace
kubectl create namespace node-app

# Apply configurations
kubectl apply -f pod.yml

kubectl apply -f replica-sets.yml

kubectl apply -f deployment.yml

kubectl apply -f service.yml

# Verify deployments
kubectl get pods -n node-app

kubectl get replicaset -n node-app

kubectl get deployment -n node-app

kubectl get services -n node-app

# Access application

# http://<node-ip>:30003

---
## 📦 Kubernetes Manifests

### 1️⃣ Pod Configuration (`pod.yml`)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: node-pod
spec:
  containers:
    - name: node-container
      image: junaidtyagi/nodo-todo-app
      ports:
        - containerPort: 8000
