# kubernetes

- **Notes on Kubernetes Local Deployment**

  1. kubectl - CLI tool to talk to a Kubernetes cluster (comes bundled with kustomize)
  # Download the latest stable version
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

  # Make it executable
  chmod +x kubectl
  
  # Move it to a directory in your PATH
  sudo mv kubectl /usr/local/bin/
  
  # Verify installation
  kubectl version --client
  shows:
  a. kubectl client version 
  b. Kustomize (bundled with kubectl) - Helps you customize Kubernetes YAML files without changing the original files.

  Flow
  
  -  You write or get YAML manifests (definitions of apps).
  -  Use Kustomize to customize them if needed.
  -  Use kubectl to apply them to a Kubernetes cluster.

  2.   
  
  Kubernetes command-line tool, kubectl installation: https://kubernetes.io/docs/tasks/tools/

  kubectl get pods

  kubectl logs PODNAME

  kubectl delete pod PODNAME

  kubectl get pods -o wide

  kubectl proxy ( start a proxy on the local machine - 127.0.0.1:8001 )
  http://127.0.0.1:8001/api/v1/namespaces/default/pods

- **Kubeflow - AI Platforms on Kubernetes**

  https://github.com/kubeflow/kubeflow

- **Minikube**
  
  Minikube runs a single-node cluster
  
  https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download
  
  Hello Minikube
  
  https://kubernetes.io/docs/tutorials/hello-minikube/
  
  minikube dashboard --port=63840
