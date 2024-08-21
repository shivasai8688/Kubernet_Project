# Jenkins Installation on AWS VM

This guide covers the steps to install Jenkins on an AWS EC2 instance running Ubuntu.

## Prerequisites

- An AWS EC2 instance running Ubuntu.
- SSH access to the instance.

## Steps

### 1. Update Packages and Install Java

Jenkins requires Java to run. Install the default JDK and OpenJDK 11:

```
sudo apt update && sudo apt install default-jdk -y && sudo apt install openjdk-11-jdk -y
```
### 2. Verify Java Installation
- After the installation, check the Java version:
```
java -version
```
- This should display the installed Java version.
### 3. Add Jenkins Repository Key
- Download the Jenkins keyring:
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
### 4. Add Jenkins Repository to the Sources List
- Add the Jenkins repository:
```
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```
### 5. Update Package List and Install Jenkins
- Update the package list and install Jenkins:
```
sudo apt-get update && sudo apt-get install jenkins -y
```
### 6. Configure AWS Security Group
In the AWS Management Console:
1) Navigate to the EC2 dashboard.
2) Select your Jenkins instance.
3) In the Security tab, click on the security group associated with the instance.
4) Edit the inbound rules and add a rule to allow traffic on port 8080 (HTTP).
- Type: Custom TCP Rule
- Port Range: 8080
- Source: Anywhere (0.0.0.0/0) for public access.

### 7. Access Jenkins UI
- Once Jenkins is installed and the security group is configured, you can access the Jenkins UI in your browser using the instance's public IP and port 8080:
```
http://<instance-ip>:8080
```
### 8. Unlock Jenkins
1) SSH into your instance.
2) Retrieve the initial admin password:
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3) Enter the password on the Jenkins UI to complete the setup.

### Conclusion
- Jenkins should now be installed and accessible from your browser. You can proceed with configuring your Jenkins instance and setting up your CI/CD pipelines.



# Install Google Cloud CLI and kubectl on Jenkins Server

This guide provides instructions to install the Google Cloud CLI and kubectl on your Jenkins server. These tools are essential for managing Google Cloud resources and Kubernetes clusters.

## Prerequisites

- Jenkins server running on an AWS EC2 instance (or any Linux-based server).
- SSH access to the server.

## Steps

### 1. Install Google Cloud CLI

Google Cloud CLI (gcloud) is needed to manage Google Cloud resources, including Kubernetes clusters.

#### 1.1. Download Google Cloud CLI
```
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
```

#### 1.2. Extract the Archive
```
tar -xf google-cloud-cli-linux-x86_64.tar.gz
```
#### 1.3. Run the Installation Script
```
./google-cloud-sdk/install.sh
```
- You can also run the script with additional options:
```
./google-cloud-sdk/install.sh --help
```
#### 1.4. Initialize the gcloud CLI
After installation, initialize the gcloud CLI to configure the environment:
```
./google-cloud-sdk/bin/gcloud init
```
- Follow the prompts to set up your Google Cloud account and project.

### 2. Install kubectl
kubectl is the command-line tool for interacting with Kubernetes clusters.

#### 2.1. Download kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
#### 2.2. Validate the Binary (Optional)
- You can validate the kubectl binary by checking its SHA-256 checksum:
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
- Validate the checksum:
```
echo "$(cat kubectl.sha256) kubectl" | sha256sum --check
```
#### 2.3. Install kubectl
- Move the binary to your /usr/local/bin directory:
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
#### 2.4. Verify Installation
- Check the kubectl version to ensure it was installed correctly:
```
kubectl version --client
```
#### Conclusion
- You have successfully installed the Google Cloud CLI and kubectl on your Jenkins server. You can now create and manage Kubernetes clusters directly from the Jenkins server.







# Google Cloud Login and Kubernetes Cluster Creation on Jenkins Server

This guide provides instructions for logging into Google Cloud, initializing the gcloud CLI, and creating a Kubernetes cluster using `gcloud` and `kubectl` on your Jenkins server.

## Prerequisites

- Jenkins server with Google Cloud CLI (gcloud) and kubectl installed.
- A Google Cloud account.

## Steps

### 1. Login to Google Cloud

Before creating a Kubernetes cluster, you need to authenticate your Jenkins server with your Google Cloud account.

#### 1.1. Authenticate with gcloud

Run the following command to log in:

```
gcloud auth login
```

#### 1.2. Authenticate via Browser
- After running the command, you'll receive a URL in the terminal.
- Copy the URL and open it in your web browser.
- Log in with your Google Cloud account.
- After successful login, a key will be generated.
- Copy the key and paste it back into the terminal to complete the authentication process.

#### 2. Initialize gcloud CLI
- After authentication, initialize the gcloud CLI to set up your default project and region:
```
gcloud init
```
- Follow the prompts to configure your environment, including selecting your Google Cloud project and setting a default region.
#### 3. Create a Kubernetes Cluster
- Once authenticated and initialized, you can create a Kubernetes cluster using the following command:
```
gcloud container clusters create mycluster --num-nodes=1
```
#### 4. Check Cluster Information
- After creating the cluster, you can check the cluster's status and configuration using kubectl:
```
kubectl cluster-info
```
- This command provides information about the cluster's components and endpoints.

### Conclusion
- You have successfully logged into Google Cloud, initialized the gcloud CLI, and created a Kubernetes cluster on your Jenkins server. You can now manage your Kubernetes cluster using kubectl and integrate it with your CI/CD pipelines.

# Automated CI/CD Pipeline Setup for React, Java, and Python Applications on GKE Using Jenkins

## Prerequisites

Before setting up the Jenkins pipeline and deploying applications to GKE, ensure you have the following:

- *Google Cloud Platform (GCP) Account* with a GKE cluster set up.
- *AWS Virtual Machine (VM)* with Jenkins installed.
- *Kubernetes CLI (kubectl)* configured to interact with your GKE cluster.
- *GitHub Repositories* containing the Jenkinsfile for React, Java, and Python applications.
- *Jenkins Kubernetes Plugins* installed on the Jenkins instance.

## Steps

### 1. *Jenkins Configuration on AWS VM*
   - Install Jenkins on your AWS VM.
   - Install and configure the necessary Kubernetes plugins in Jenkins for seamless interaction with your GKE cluster.

### 2. *Pipeline Creation*
   - *Create Jenkins Pipelines*: Set up a separate pipeline for each application (React, Java, Python) in the Jenkins UI.
   - *Configure GitHub Repositories*: Link each pipeline to the corresponding GitHub repository by providing the path to the Jenkinsfile in the pipeline configuration.

### 3. *Pipeline Execution*
   - *Run the Pipeline*: Start the pipeline for each application from the Jenkins UI.
   - *Build and Deployment*:
     - The pipeline will fetch the latest code from the GitHub repository.
     - It will then build the application.
     - Kubernetes resources (services, deployments, PVCs) defined in the Jenkinsfile will be deployed to the GKE cluster.

### 4. *Verify Deployment*
   - *Check GKE Cluster*: Verify that the services, deployments, and persistent volume claims (PVCs) are successfully running in the GKE cluster by using kubectl commands or the Google Cloud Console.

## Conclusion

By following these steps, you have successfully automated the CI/CD process for React, Java, and Python applications using Jenkins on an AWS VM, deploying to a GKE cluster. This setup ensures that each code change is automatically built, tested, and deployed, allowing for more efficient and error-free management of your applications.
