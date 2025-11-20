# 🚀 Local Kubernetes Lab with Kind on WSL

A clean, visually appealing guide to setting up a lightweight local Kubernetes environment on **Windows Subsystem for Linux (WSL)** using **kubectl** and **kind**.
Kubernetes (K8s) builds on top of Docker (or any container runtime), but it adds orchestration, scaling, and self-healing.

---

## 🎯 Table of Contents

- 📖 About this K8s (K + 8letters +s : Kubernetes) Guide
- ✅ Prerequisites
- 🛠️ Installation and Setup
  - 1. Install kubectl
  - 2. Install Kind
- 🚀 Test Your Deployment
- 💡 Understanding the Workflow
- 🤖 AI on Kubernetes: Kubeflow
- 🙌 Contributing
- 📄 License

---

## 📖 About this K8s guide

This guide walks you through creating a full Kubernetes (kubectl and kind) environment locally using WSL  

---

## ✅ Prerequisites

Make sure you have:

- **Windows Subsystem for Linux (WSL 2)**
- **Docker Desktop** with the WSL 2 backend enabled

---

## 🛠️ Installation and Setup

---

### 1. Install kubectl

`kubectl` is the command-line tool used to manage Kubernetes clusters.

#### ➤ Download the latest stable version  
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

#### ➤ Make it executable  
    chmod +x kubectl

#### ➤ Move it into your PATH  
    sudo mv kubectl /usr/local/bin/

#### ➤ Verify installation  
    kubectl version --client

---

### 2. Install Kind

Kind runs Kubernetes clusters inside Docker containers.

#### ➤ Install Kind  
    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
    kind version

#### ➤ Create a Kubernetes cluster  
    kind create cluster --name learning-cluster

By default, kind creates:

__1 control plane node__ (runs the Kubernetes API server, scheduler, etc.)        
__0 worker nodes__ (just the control plane)

So you get a single-node cluster where the control plane node also acts as a worker node. 

#### ➤ Now the cluster is created - Verify the cluster  
    kubectl get nodes
    kubectl cluster-info
    kubectl version

#### ➤ (Optional) Delete the cluster  
    kind delete cluster

---

## 🚀 Test Your Deployment

#### ➤ Create a test deployment - a Deployment acts as your automated application manager
        kubectl create deployment hello-kind --image=nginx     
                                                 ↑
                                                 This is the ACTUAL application code

#### ➤ Check pods  
    kubectl get pods

#### ➤ Expose the deployment - Make the application accessible from outside the cluster  
    kubectl expose deployment hello-kind --type=NodePort --port=8080 --target-port=80

    

#### ➤ (Optional) Check what NodePort was assigned - NodePort defaults to 30000-32767 unless set with `--node-port`

    kubectl get svc hello-kind

#### ➤ Forward traffic to your machine - creates a temporary tunnel (Dies when you close terminal)  
    
    kubectl port-forward service/hello-kind 8080:8080

Open **http://localhost:8080** to see the service running.

---

## 💡 Understanding the Workflow

Your Kubernetes environment follows this architecture:

    Cluster
     ├─ Node (Control Plane)
     ├─ Node (Worker)
     └─ Node (Local: CP + Worker)
          └─ Pod
               └─ Container
                    └─ Image

### Key Concepts

- **kubectl** — CLI for Kubernetes  
- **kind** — Local Kubernetes-in-Docker  
- **Cluster** — The full Kubernetes system  
- **Node** — Machine in the cluster  
- **Pod** — Smallest deployable unit  
- **Container** — Runs your application  

---

## 🤖 AI on Kubernetes: Kubeflow

Explore ML workflows on Kubernetes with **Kubeflow**:  
https://github.com/kubeflow/kubeflow

---

## 🙌 Contributing

Contributions welcome — open an issue or submit a pull request!

---

## 📄 License

Licensed under the **MIT License**.  
See the `LICENSE` file for details.
