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

## Prerequisites
* Kubernetes cluster
* <b>kubectl</b> command-line tool
* <b>kustomize</b> command-line tool (or <b>kubectl</b> with Kustomize support)
* <b>cert-manager</b> for TLS certificate management (if using Ingress)

## Setup
<b>1.</b> ## Clone the Repository
```
git clone <repository-url>
cd <repository-directory>
```

<b>2.</b> ## Apply the Manifests
Use Kustomize to build and apply the manifests:
```
kubectl apply -k .

```

<b>3.</b> ## Verify Deployment
Ensure all resources are correctly deployed:
```
kubectl get deployments -n default
kubectl get services -n default
kubectl get ingress -n default
kubectl get pvc -n default
kubectl get pv
kubectl get serviceaccounts -n default
kubectl get clusterrolebindings
```

<b>4.</b> ## Access Jenkins
After deployment, Jenkins will be accessible via the Ingress hostname specified in ingress.yaml. Ensure that DNS or /etc/hosts file is configured to resolve the hostname to your Kubernetes cluster's external IP.

<b>5.</b> ## Prometheus Integration
The Jenkins service is annotated for Prometheus scraping. Ensure Prometheus is configured to scrape metrics from the Jenkins endpoint:

* Path: <b/
* <b>Port:</b> 8080
