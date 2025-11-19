# kubernetes

- **Notes on Kubernetes for WSL Local Deployment**
- Cluster -> Node -> Pod -> Container -> Images

  # 1. kubectl - CLI tool to talk to a Kubernetes cluster (comes bundled with kustomize)

  - Download the latest stable version
  `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"`

    or see here: kubectl installation: https://kubernetes.io/docs/tasks/tools/

  - Make it executable
  `chmod +x kubectl`
  
  - Move it to a directory in your PATH
  `sudo mv kubectl /usr/local/bin/`
  
  - Verify installation `kubectl version --client`
  
  Output of `kubectl version --client` :

  a. kubectl client version
  
  b. Kustomize (bundled with kubectl) - Helps you customize Kubernetes YAML files without changing the original files.

  Flow:
  
  -  You write or get YAML manifests (definitions of apps)
  -  Use Kustomize to customize them if needed.
  -  Use kubectl to apply them to a Kubernetes cluster.

  # 2. Install Kind and run a kubernetes cluster on WSL - Kind spins up a Kubernetes cluster inside Docker containers

  - Step 1: Install Kind on WSL:
    
    `curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64`
    
    `chmod +x ./kind`
  
    `sudo mv ./kind /usr/local/bin/kind`

    `kind version`

  - Step 2: Create a Kubernetes cluster:
 
    `kind create cluster`

  - Step 3: Verify the cluster:
 
    `kubectl get nodes`

    `kubectl cluster-info`

  - Step 4: Test a deployment:
 
    `kubectl create deployment hello-kind --image=k8s.gcr.io/echoserver:1.4`

    `kubectl get pods`
  
    `kubectl expose deployment hello-kind --type=NodePort --port=8080`

    `minikube service hello-kind  # optional if you want to see it in browser`

    `kubectl port-forward service/hello-kind 8080:8080` - Then open http://localhost:8080 in your browser.

  - Step 5: Optional: delete the cluster when done:

    `kind delete cluster`
 
    Now you have a fully working local Kubernetes environment on WSL:

    kubectl → your CLI to interact with Kubernetes.
    
    kind → a lightweight Kubernetes cluster running inside Docker Desktop.

 
        A Kubernetes cluster is the top-level entity. It is made up of nodes. Each node is a machine (physical or virtual).
        Nodes have roles, not sub-nodes. Control plane node(s) → manage the cluster.
        Worker node(s) → run your applications (Pods/containers). A node cannot contain other nodes.        
        Pods → Containers → Images. Worker nodes run Pods. Pods contain containers, which are instances of images.
        
        Cluster
         ├─ Node (Control Plane Role)
         ├─ Node (Worker Role)
         ├─ Node (Worker Role)
         ...
         └─ Node (Optional: Control Plane + Worker on same node in local cluster)
               └─ Pod
                    └─ Container
                         └─ Image

    <img width="1024" height="1024" alt="ChatGPT Image Nov 19, 2025, 11_38_38 PM" src="https://github.com/user-attachments/assets/02410853-9b14-46e6-9d86-e66b37865715" />

  
    - **Kubeflow - AI Platforms on Kubernetes**
    
      https://github.com/kubeflow/kubeflow
