# 🚀 Local Kubernetes Lab with Kind on WSL

A clean, visually appealing guide to setting up a lightweight local Kubernetes environment on **Windows Subsystem for Linux (WSL)** using **kubectl** and **kind**.
Kubernetes (K8s) builds on top of Docker (or any container runtime), but it adds orchestration, scaling, and self-healing.

__Analogy:__ Think of Docker as a single car. Kubernetes is a traffic control system for thousands of cars,

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

A pod is the smallest deployable unit in Kubernetes. It can contain one or more containers (usually one). Pods are ephemeral: they can be created, destroyed, or rescheduled at any time.

#### ➤ Expose the deployment - Make the application accessible from outside the cluster  
    kubectl expose deployment hello-kind --type=NodePort --port=8080 --target-port=80

With this command Kubernetes creates a Service object automatically. 
This Service is named hello-kind (same as the deployment, unless you add --name) of type NodePort listening on port 8080 on the Service forwarding traffic to port 80 on the Pods.

#### ➤ (Optional) Check what NodePort was assigned - NodePort defaults to 30000-32767 unless set with `--node-port`

    kubectl get svc hello-kind

#### ➤ Forward traffic to your machine - creates a temporary tunnel (Dies when you close terminal)  
    
    kubectl port-forward service/hello-kind 8080:8080

Open **http://localhost:8080** to see the service running.

__Service:__ exposes one or more pods to other pods or external traffic

__One Service → many pods, one pod → many containers/images__

#### ➤ Scale your Deployment to multiple replicas

    kubectl scale deployment hello-kind --replicas=3

After scaling to 3 replicas run again

    kubectl get pods

#### ➤ Perform a rolling update to upgrade your Nginx version without downtime.

    kubectl set image deployment/hello-kind nginx=nginx:latest

After running this, you can check the rollout status:

    kubectl rollout status deployment/hello-kind

#### ➤ Export your current Deployment from the cluster into a local YAML file called hello-kind-deployment.yaml

    kubectl get deployment hello-kind -o yaml > hello-kind-deployment.yaml

#### ➤ Edit the hello-kind-deployment.yaml to add liveness and readiness probes, so Kubernetes can perform self-healing.

    nano hello-kind-deployment.yaml

Add this code in the Containers section:

          containers:
    - image: nginx:latest
      imagePullPolicy: Always
      name: nginx
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 5
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 5

 Save the YAML file. Apply it back to the cluster:

     kubectl apply -f hello-kind-deployment.yaml

#### ➤ Test self-healing probes safely without manually stopping processes inside the pod. 

The idea is to temporarily make the pod fail its liveness probe and watch Kubernetes restart it.

__Check the pods:__

     kubectl get pods 

__Use kubectl patch to change the probe path. You can point the liveness probe to a path that doesn’t exist, causing it to fail:__

      kubectl patch deployment hello-kind \
    --type='json' \
    -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/livenessProbe/httpGet/path", "value": "/fail"}]'

__Watch the pods restart:__

    kubectl get pods -w
    
__Restore the original path:__

      kubectl patch deployment hello-kind \
    --type='json' \
    -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/livenessProbe/httpGet/path", "value": "/"}]'

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
- **Pod** — a single running container (the smallest unit)  
- **Container** — Runs your application   
- **Deployment** — Runs and manages Pods
- **Service** — Exposes Pods to the network
- **Namespace** — Organizes resources
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
