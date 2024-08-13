# Jenkins Kubernetes Deployment
This repository contains Kubernetes manifests and configurations to deploy Jenkins in a Kubernetes cluster. The setup includes Jenkins deployment, Ingress for external access, persistent storage, and necessary RBAC configurations.
## Contents
* <b>deployment.yaml:</b> Defines the Jenkins Deployment, including container settings, probes, and volume mounts.
* <b>ingress.yaml:</b> Configures external access to Jenkins with TLS and Prometheus integration.
* <b>pv.yaml:</b> Specifies the PersistentVolume for Jenkins storage.
* <b>pvc.yaml:</b> Defines the PersistentVolumeClaim for Jenkins storage.
* <b>service.yaml:</b> Exposes Jenkins service with appropriate ports and Prometheus annotations.
* <b>serviceAccount.yaml:</b> Creates the Jenkins ServiceAccount.
* <b>kustomization.yaml:</b> Kustomize configuration for managing the deployment and resources.
