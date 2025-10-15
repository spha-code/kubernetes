# kubernetes

- **Notes on Kubernetes**

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
