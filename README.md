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
