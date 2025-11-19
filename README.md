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

  # 2. Kind runs all the Kubernetes components (API server, scheduler, etc.) in a single-node cluster

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
  
- **Kubeflow - AI Platforms on Kubernetes**

  https://github.com/kubeflow/kubeflow

- **Minikube**
  
  Minikube runs a single-node cluster
  
  https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download
  
  Hello Minikube
  
  https://kubernetes.io/docs/tutorials/hello-minikube/
  
  minikube dashboard --port=63840
