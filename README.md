# Jenkins Kubernetes Deployment
This repository contains Kubernetes manifests and configurations to deploy Jenkins in a Kubernetes cluster. The setup includes Jenkins deployment, Ingress for external access, persistent storage, and necessary RBAC configurations.
## Contents
* deployment.yaml: Defines the Jenkins Deployment, including container settings, probes, and volume mounts.
* ingress.yaml: Configures external access to Jenkins with TLS and Prometheus integration.
* pv.yaml: Specifies the PersistentVolume for Jenkins storage.
* pvc.yaml: Defines the PersistentVolumeClaim for Jenkins storage.
* service.yaml: Exposes Jenkins service with appropriate ports and Prometheus annotations.
* serviceAccount.yaml: Creates the Jenkins ServiceAccount.
* kustomization.yaml: Kustomize configuration for managing the deployment and resources.
