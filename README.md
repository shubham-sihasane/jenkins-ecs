# Simple_NodeJS_App
A Simple Node.js Application Deployment Example

## Video Link
Watch the comprehensive tutorial on YouTube: [CI/CD Pipeline with Jenkins, Docker, and AWS ECS](https://www.youtube.com/watch?v=uGWF3meicAk&t=1827s)

---

## Video Summary
**Comprehensive Guide to CI/CD Pipeline with Jenkins, Docker, and AWS ECS**

This tutorial provides a detailed walkthrough of building a CI/CD pipeline using Jenkins, Docker, and AWS ECS, aimed at improving software development efficiency and security.

### **Main Insights**
1. **Setting Up Jenkins**: Automated code builds from GitHub repositories.
2. **SonarQube Integration**: Vulnerability checks to ensure secure applications.
3. **Docker for Image Creation**: Building and managing Docker images.
4. **AWS ECS Deployment**: Using AWS Fargate for simplified infrastructure management.
5. **Resource Management**: Cleaning up resources to prevent unnecessary costs.

---

## Overview of the CI/CD Pipeline
The video demonstrates the creation of an end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline with the following steps:

### **Pipeline Steps**
1. **Code Build and Vulnerability Checks**
   - Jenkins setup for GitHub integration.
   - Running unit tests and scanning for vulnerabilities using SonarQube.
2. **Docker Integration**
   - Building Docker images.
   - Pushing images to Amazon Elastic Container Registry (ECR).
3. **Deployment to AWS**
   - Deploying Docker images to Amazon ECS with AWS Fargate.

---

## Jenkins Configuration and GitHub Integration
- **Setting up Jenkins**: Configuring GitHub credentials and defining pipeline stages using a `Jenkinsfile`.
- **Running Tests**: Unit tests with npm and code quality analysis with SonarQube.

---

## Docker Integration and Deployment
- **Building Docker Images**: Creating lightweight Docker images using Alpine.
- **Vulnerability Scanning**: Using Trivy to scan Docker images.
- **AWS Integration**:
  - Pushing Docker images to AWS ECR.
  - Deploying to AWS ECS using Fargate.

---

## AWS ECS Deployment
1. **Infrastructure Setup**:
   - Creating ECS clusters and services.
   - Configuring load balancers and security groups.
2. **Application Management**:
   - Deploying and managing containerized applications.
   - Monitoring performance and ensuring public accessibility.
3. **Cost Optimization**:
   - Deleting unused resources to prevent unnecessary expenses.

---

### Set up Jenkins 
Run the command below to create the Jenkins Container:
```sh
docker run -d --name jenkins-dind \
-p 8080:8080 -p 50000:50000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
-u root \
-e DOCKER_GID=$(getent group docker | cut -d: -f3) \
jenkins/jenkins:lts
```

Check Running Docker Container
```sh
docker ps
```

Log into the Jenkins container
```sh
docker exec -it jenkins-dind bash
```
Run the following bash commands in the Jenkins Container's Terminal:
```sh
groupadd -for -g $DOCKER_GID docker
usermod -aG docker jenkins
exit
```
```sh
docker restart jenkins-dind
```

Check the Jenkins Container Logs to Get Jenkins' Initial Admin Password
```sh
docker logs -f jenkins-dind
```

On your browser, open:
```sh 
PublicIP:8080
```
Paste Initial Admin Password & Install Suggested Plugins
Create an Admin User Account with username & password, email, etc.


### Configure Jenkins & GitHub Integration
Create 






## Resource Cleanup
To maintain cost efficiency, ensure proper deletion of:
- Target groups
- Load balancers
- ECS clusters
- Any associated AWS resources.

---

## Conclusion
This guide empowers developers with a streamlined CI/CD pipeline setup. By combining Jenkins, Docker, and AWS ECS, you can build, test, and deploy applications effectively while maintaining high-quality standards and cost efficiency.

---
