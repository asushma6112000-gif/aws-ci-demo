 
# AWS CI/CD Pipeline Project (Flask + Docker + ECS)
 
## Project Overview
 
This project demonstrates a complete CI/CD pipeline using AWS services to automate the build and deployment of a containerized Flask application.
 
A simple Python Flask application was containerized using Docker and deployed to Amazon ECS through a fully automated CI/CD pipeline using GitHub, AWS CodePipeline, and AWS CodeBuild.
 
The goal of this project was to understand end-to-end CI/CD workflow, Docker deployment, container orchestration, load balancing, auto scaling, monitoring, and troubleshooting real AWS deployment issues.

architecture Diagram 

_
## Architecture Flow
 
Developer Push Code ↓ GitHub Repository ↓ AWS CodePipeline ↓ AWS CodeBuild ↓ Docker Image Build ↓ Amazon ECR ↓ Amazon ECS (Fargate) ↓ Application Load Balancer (ALB) ↓ Users ↓ CloudWatch Monitoring ↓ Auto Scaling
 
## Technologies Used
 
 
- 
 

 
Python Flask
 
 
- 
 

 
Docker
 
 
- 
 

 
GitHub
 
 
- 
 

 
AWS CodePipeline
 
 
- 
 

 
AWS CodeBuild
 
 
- 
 

 
Amazon ECR
 
 
- 
 

 
Amazon ECS (Fargate)
 
 
- 
 

 
Application Load Balancer (ALB)
 
 
- 
 

 
Amazon CloudWatch
 
 
- 
 

 
ECS Service Auto Scaling
 
## Deployment Workflow
 
 
1. 
 

 
Source code is pushed to GitHub
 
 
1. 
 

 
AWS CodePipeline detects code changes
 
 
1. 
 

 
AWS CodeBuild starts automatically
 
 
1. 
 

 
Docker image is built from the Flask application
 
 
1. 
 

 
Docker image is pushed to Amazon ECR
 
 
1. 
 

 
Amazon ECS pulls the latest image from ECR and deploys the container
 
 
1. 
 

 
Application Load Balancer routes user traffic to ECS tasks
 
 
1. 
 

 
Application becomes accessible through ALB DNS
 
## Buildspec Workflow
 
The build process is defined inside `buildspec.yml`, which is used by AWS CodeBuild.
 
### pre_build
 
 
- 
 

 
Authenticate Docker with Amazon ECR
 
 
- 
 

 
Prepare Amazon ECR repository URI
 
 
- 
 

 
Set Docker image tag
 
### build
 
 
- 
 

 
Build Docker image from Dockerfile
 
 
- 
 

 
Tag Docker image with latest tag
 
### post_build
 
 
- 
 

 
Push Docker image to Amazon ECR
 
 
- 
 

 
Generate `imagedefinitions.json` for Amazon ECS deployment
 
### artifacts
 
 
- 
 

 
Export `imagedefinitions.json`
 
 
- 
 

 
Pass artifact to Deploy stage in CodePipeline
 
## Project Structure
 
`. ├── app.py ├── Dockerfile ├── requirements.txt ├── buildspec.yml ├── screenshots/ │   ├── pipeline-success.png │   ├── deploy-troubleshooting.png │   ├── ecs-public-ip-app.png │   └── alb-running-app.png └── README.md `
 
## Docker Build Process
 
The application is containerized using Docker.
 
### Dockerfile Responsibilities
 
 
- 
 

 
Use Python base image
 
 
- 
 

 
Copy application files
 
 
- 
 

 
Install dependencies
 
 
- 
 

 
Run Flask application
 
## AWS Services Used
 
### AWS CodePipeline
 
Automates the complete CI/CD workflow.
 
### AWS CodeBuild
 
Builds Docker images and prepares deployment artifacts.
 
### Amazon ECR
 
Stores Docker container images securely.
 
### Amazon ECS (Fargate)
 
Runs the containerized Flask application.
 
### Application Load Balancer (ALB)
 
Distributes incoming user traffic across ECS tasks.
 
### Amazon CloudWatch
 
Monitors logs, metrics, and service health.
 
### ECS Auto Scaling
 
Automatically scales ECS tasks based on workload.
 
## Troubleshooting & Fixes
 
### 1. GitHub Source Connection Issue
 
**Issue:** GitHub source connection was not configured properly during pipeline creation.
 
**Fix:** Configured GitHub connection using AWS CodeConnections.
 
**Result:** Source stage connected successfully.
 
### 2. ECR Repository Configuration Issue
 
**Issue:** Incorrect Amazon ECR repository URI in `buildspec.yml`.
 
**Fix:** Configured the correct ECR repository URI and image tagging.
 
**Result:** Docker image pushed successfully to Amazon ECR.
 
### 3. Deploy Stage Failed
 
**Issue:** Build stage succeeded, but Deploy stage failed.
 
**Root Cause:** Deployment artifact configuration was incorrect. The container name inside `imagedefinitions.json` did not match the ECS task definition.
 
**Fix:** Updated `buildspec.yml` to generate correct `imagedefinitions.json` with proper container name and image URI.
 
Example: `[   {     "name": "aws-ci-demo-container",     "imageUri": "<ECR_IMAGE_URI>"   } ] ` **Result:** Deployment succeeded.
 
### 4. Deployment Delay During ECS Service Update
 
**Issue:** Deployment remained in progress longer than expected.
 
**Root Cause:** Amazon ECS waits for new tasks to pass health checks and reach steady state before completing deployment.
 
**Fix:** Verified task status, service events, and deployment health.
 
**Result:** Deployment completed successfully.
 
### 5. Security Group / ALB Access Issue
 
**Issue:** Application deployed successfully but was not accessible through ALB DNS.
 
**Root Cause:** Security group inbound rules were not configured correctly.
 
**Fix:** Updated security groups:
 
 
- 
 

 
ALB security group allowed inbound traffic on port 80
 
 
- 
 

 
ECS task security group allowed traffic from ALB on port 5000
 
**Result:** Application became accessible through ALB DNS.
 
## Application Access
 
Application was successfully accessible through ALB DNS.
 
Example: `http://<alb-dns-name> `
 
## Screenshots
 
### Pipeline Success
 
Pipeline Success
 
### Deployment Troubleshooting
 
Deployment Troubleshooting
 
### ECS Public IP Running Application
 
ECS Public IP
 
### ALB DNS Running Application
 
ALB Running App
 
## Key Learnings
 
 
- 
 

 
Built end-to-end AWS CI/CD pipeline
 
 
- 
 

 
Learned Docker image creation and deployment
 
 
- 
 

 
Understood Amazon ECR and ECS integration
 
 
- 
 

 
Configured Application Load Balancer
 
 
- 
 

 
Implemented ECS Service Auto Scaling
 
 
- 
 

 
Monitored infrastructure using CloudWatch
 
 
- 
 

 
Practiced debugging real AWS deployment issues
 
## Future Improvements
 
 
- 
 

 
Enable HTTPS using a custom domain and AWS Certificate Manager (ACM)
 
 
- 
 

 
Implement Blue/Green deployment strategy
 
## Project Status
 
Project completed successfully.
 
# AWS CI/CD Pipeline Project (Flask + Docker + ECS)
 
## Project Overview
 
This project demonstrates a complete CI/CD pipeline using AWS services to automate the build and deployment of a containerized Flask application.
 
A simple Python Flask application was containerized using Docker and deployed to Amazon ECS through a fully automated CI/CD pipeline using GitHub, AWS CodePipeline, and AWS CodeBuild.
 
The goal of this project was to understand end-to-end CI/CD workflow, Docker deployment, container orchestration, load balancing, auto scaling, monitoring, and troubleshooting real AWS deployment issues.
 
## Architecture Flow
 
Developer Push Code ↓ GitHub Repository ↓ AWS CodePipeline ↓ AWS CodeBuild ↓ Docker Image Build ↓ Amazon ECR ↓ Amazon ECS (Fargate) ↓ Application Load Balancer (ALB) ↓ Users ↓ CloudWatch Monitoring ↓ Auto Scaling
 
## Technologies Used
 
 
- 
 

 
Python Flask
 
 
- 
 

 
Docker
 
 
- 
 

 
GitHub
 
 
- 
 

 
AWS CodePipeline
 
 
- 
 

 
AWS CodeBuild
 
 
- 
 

 
Amazon ECR
 
 
- 
 

 
Amazon ECS (Fargate)
 
 
- 
 

 
Application Load Balancer (ALB)
 
 
- 
 

 
Amazon CloudWatch
 
 
- 
 

 
ECS Service Auto Scaling
 
## Deployment Workflow
 
 
1. 
 

 
Source code is pushed to GitHub
 
 
1. 
 

 
AWS CodePipeline detects code changes
 
 
1. 
 

 
AWS CodeBuild starts automatically
 
 
1. 
 

 
Docker image is built from the Flask application
 
 
1. 
 

 
Docker image is pushed to Amazon ECR
 
 
1. 
 

 
Amazon ECS pulls the latest image from ECR and deploys the container
 
 
1. 
 

 
Application Load Balancer routes user traffic to ECS tasks
 
 
1. 
 

 
Application becomes accessible through ALB DNS
 
## Buildspec Workflow
 
The build process is defined inside `buildspec.yml`, which is used by AWS CodeBuild.
 
### pre_build
 
 
- 
 

 
Authenticate Docker with Amazon ECR
 
 
- 
 

 
Prepare Amazon ECR repository URI
 
 
- 
 

 
Set Docker image tag
 
### build
 
 
- 
 

 
Build Docker image from Dockerfile
 
 
- 
 

 
Tag Docker image with latest tag
 
### post_build
 
 
- 
 

 
Push Docker image to Amazon ECR
 
 
- 
 

 
Generate `imagedefinitions.json` for Amazon ECS deployment
 
### artifacts
 
 
- 
 

 
Export `imagedefinitions.json`
 
 
- 
 

 
Pass artifact to Deploy stage in CodePipeline
 
## Project Structure
 
`. ├── app.py ├── Dockerfile ├── requirements.txt ├── buildspec.yml ├── screenshots/ │   ├── pipeline-success.png │   ├── deploy-troubleshooting.png │   ├── ecs-public-ip-app.png │   └── alb-running-app.png └── README.md `
 
## Docker Build Process
 
The application is containerized using Docker.
 
### Dockerfile Responsibilities
 
 
- 
 

 
Use Python base image
 
 
- 
 

 
Copy application files
 
 
- 
 

 
Install dependencies
 
 
- 
 

 
Run Flask application
 
## AWS Services Used
 
### AWS CodePipeline
 
Automates the complete CI/CD workflow.
 
### AWS CodeBuild
 
Builds Docker images and prepares deployment artifacts.
 
### Amazon ECR
 
Stores Docker container images securely.
 
### Amazon ECS (Fargate)
 
Runs the containerized Flask application.
 
### Application Load Balancer (ALB)
 
Distributes incoming user traffic across ECS tasks.
 
### Amazon CloudWatch
 
Monitors logs, metrics, and service health.
 
### ECS Auto Scaling
 
Automatically scales ECS tasks based on workload.
 
## Troubleshooting & Fixes
 
### 1. GitHub Source Connection Issue
 
**Issue:** GitHub source connection was not configured properly during pipeline creation.
 
**Fix:** Configured GitHub connection using AWS CodeConnections.
 
**Result:** Source stage connected successfully.
 
### 2. ECR Repository Configuration Issue
 
**Issue:** Incorrect Amazon ECR repository URI in `buildspec.yml`.
 
**Fix:** Configured the correct ECR repository URI and image tagging.
 
**Result:** Docker image pushed successfully to Amazon ECR.
 
### 3. Deploy Stage Failed
 
**Issue:** Build stage succeeded, but Deploy stage failed.
 
**Root Cause:** Deployment artifact configuration was incorrect. The container name inside `imagedefinitions.json` did not match the ECS task definition.
 
**Fix:** Updated `buildspec.yml` to generate correct `imagedefinitions.json` with proper container name and image URI.
 
Example: `[   {     "name": "aws-ci-demo-container",     "imageUri": "<ECR_IMAGE_URI>"   } ] ` **Result:** Deployment succeeded.
 
### 4. Deployment Delay During ECS Service Update
 
**Issue:** Deployment remained in progress longer than expected.
 
**Root Cause:** Amazon ECS waits for new tasks to pass health checks and reach steady state before completing deployment.
 
**Fix:** Verified task status, service events, and deployment health.
 
**Result:** Deployment completed successfully.
 
### 5. Security Group / ALB Access Issue
 
**Issue:** Application deployed successfully but was not accessible through ALB DNS.
 
**Root Cause:** Security group inbound rules were not configured correctly.
 
**Fix:** Updated security groups:
 
 
- 
 

 
ALB security group allowed inbound traffic on port 80
 
 
- 
 

 
ECS task security group allowed traffic from ALB on port 5000
 
**Result:** Application became accessible through ALB DNS.
 
## Application Access
 
Application was successfully accessible through ALB DNS.
 
Example: `http://<alb-dns-name> `
 
## Screenshots
 
### Pipeline Success
 
Pipeline Success
 
### Deployment Troubleshooting
 
Deployment Troubleshooting
 
### ECS Public IP Running Application
 
ECS Public IP
 
### ALB DNS Running Application
 
ALB Running App
 
## Key Learnings
 
 
- 
 

 
Built end-to-end AWS CI/CD pipeline
 
 
- 
 

 
Learned Docker image creation and deployment
 
 
- 
 

 
Understood Amazon ECR and ECS integration
 
 
- 
 

 
Configured Application Load Balancer
 
 
- 
 

 
Implemented ECS Service Auto Scaling
 
 
- 
 

 
Monitored infrastructure using CloudWatch
 
 
- 
 

 
Practiced debugging real AWS deployment issues
 
## Future Improvements
 
 
- 
 

 
Enable HTTPS using a custom domain and AWS Certificate Manager (ACM)
 
 
- 
 

 
Implement Blue/Green deployment strategy
 
## Project Overview
 
# AWS CI/CD Pipeline Project (Flask + Docker + ECS)
 
## Project Overview
 
This project demonstrates a complete CI/CD pipeline using AWS services to automate the build and deployment of a containerized Flask application.
 
A simple Python Flask application was containerized using Docker and deployed to Amazon ECS through a fully automated CI/CD pipeline using GitHub, AWS CodePipeline, and AWS CodeBuild.
 
The goal of this project was to understand end-to-end CI/CD workflow, Docker deployment, container orchestration, load balancing, auto scaling, monitoring, and troubleshooting real AWS deployment issues.
 
## Architecture Flow
 
Developer Push Code ↓ GitHub Repository ↓ AWS CodePipeline ↓ AWS CodeBuild ↓ Docker Image Build ↓ Amazon ECR ↓ Amazon ECS (Fargate) ↓ Application Load Balancer (ALB) ↓ Users ↓ CloudWatch Monitoring ↓ Auto Scaling
 
## Technologies Used
 
 
- 
 

 
Python Flask
 
 
- 
 

 
Docker
 
 
- 
 

 
GitHub
 
 
- 
 

 
AWS CodePipeline
 
 
- 
 

 
AWS CodeBuild
 
 
- 
 

 
Amazon ECR
 
 
- 
 

 
Amazon ECS (Fargate)
 
 
- 
 

 
Application Load Balancer (ALB)
 
 
- 
 

 
Amazon CloudWatch
 
 
- 
 

 
ECS Service Auto Scaling
 
## Deployment Workflow
 
 
1. 
 

 
Source code is pushed to GitHub
 
 
1. 
 

 
AWS CodePipeline detects code changes
 
 
1. 
 

 
AWS CodeBuild starts automatically
 
 
1. 
 

 
Docker image is built from the Flask application
 
 
1. 
 

 
Docker image is pushed to Amazon ECR
 
 
1. 
 

 
Amazon ECS pulls the latest image from ECR and deploys the container
 
 
1. 
 

 
Application Load Balancer routes user traffic to ECS tasks
 
 
1. 
 

 
Application becomes accessible through ALB DNS
 
## Buildspec Workflow
 
The build process is defined inside `buildspec.yml`, which is used by AWS CodeBuild.
 
### pre_build
 
 
- 
 

 
Authenticate Docker with Amazon ECR
 
 
- 
 

 
Prepare Amazon ECR repository URI
 
 
- 
 

 
Set Docker image tag
 
### build
 
 
- 
 

 
Build Docker image from Dockerfile
 
 
- 
 

 
Tag Docker image with latest tag
 
### post_build
 
 
- 
 

 
Push Docker image to Amazon ECR
 
 
- 
 

 
Generate `imagedefinitions.json` for Amazon ECS deployment
 
### artifacts
 
 
- 
 

 
Export `imagedefinitions.json`
 
 
- 
 

 
Pass artifact to Deploy stage in CodePipeline
 
## Project Structure
 
`. ├── app.py ├── Dockerfile ├── requirements.txt ├── buildspec.yml ├── screenshots/ │   ├── pipeline-success.png │   ├── deploy-troubleshooting.png │   ├── ecs-public-ip-app.png │   └── alb-running-app.png └── README.md `
 
## Docker Build Process
 
The application is containerized using Docker.
 
### Dockerfile Responsibilities
 
 
- 
 

 
Use Python base image
 
 
- 
 

 
Copy application files
 
 
- 
 

 
Install dependencies
 
 
- 
 

 
Run Flask application
 
## AWS Services Used
 
### AWS CodePipeline
 
Automates the complete CI/CD workflow.
 
### AWS CodeBuild
 
Builds Docker images and prepares deployment artifacts.
 
### Amazon ECR
 
Stores Docker container images securely.
 
### Amazon ECS (Fargate)
 
Runs the containerized Flask application.
 
### Application Load Balancer (ALB)
 
Distributes incoming user traffic across ECS tasks.
 
### Amazon CloudWatch
 
Monitors logs, metrics, and service health.
 
### ECS Auto Scaling
 
Automatically scales ECS tasks based on workload.
 
## Troubleshooting & Fixes
 
### 1. GitHub Source Connection Issue
 
**Issue:** GitHub source connection was not configured properly during pipeline creation.
 
**Fix:** Configured GitHub connection using AWS CodeConnections.
 
**Result:** Source stage connected successfully.
 
### 2. ECR Repository Configuration Issue
 
**Issue:** Incorrect Amazon ECR repository URI in `buildspec.yml`.
 
**Fix:** Configured the correct ECR repository URI and image tagging.
 
**Result:** Docker image pushed successfully to Amazon ECR.
 
### 3. Deploy Stage Failed
 
**Issue:** Build stage succeeded, but Deploy stage failed.
 
**Root Cause:** Deployment artifact configuration was incorrect. The container name inside `imagedefinitions.json` did not match the ECS task definition.
 
**Fix:** Updated `buildspec.yml` to generate correct `imagedefinitions.json` with proper container name and image URI.
 
Example: `[   {     "name": "aws-ci-demo-container",     "imageUri": "<ECR_IMAGE_URI>"   } ] ` **Result:** Deployment succeeded.
 
### 4. Deployment Delay During ECS Service Update
 
**Issue:** Deployment remained in progress longer than expected.
 
**Root Cause:** Amazon ECS waits for new tasks to pass health checks and reach steady state before completing deployment.
 
**Fix:** Verified task status, service events, and deployment health.
 
**Result:** Deployment completed successfully.
 
### 5. Security Group / ALB Access Issue
 
**Issue:** Application deployed successfully but was not accessible through ALB DNS.
 
**Root Cause:** Security group inbound rules were not configured correctly.
 
**Fix:** Updated security groups:
 
 
- 
 

 
ALB security group allowed inbound traffic on port 80
 
 
- 
 

 
ECS task security group allowed traffic from ALB on port 5000
 
**Result:** Application became accessible through ALB DNS.
 
## Application Access
 
Application was successfully accessible through ALB DNS.
 
Example: `http://<alb-dns-name> `
 
## Screenshots
 
### Pipeline Success
 
Pipeline Success
 
### Deployment Troubleshooting
 
Deployment Troubleshooting
 
### ECS Public IP Running Application
 
ECS Public IP
 
### ALB DNS Running Application
 
ALB Running App
 
## Key Learnings
 
 
- 
 

 
Built end-to-end AWS CI/CD pipeline
 
 
- 
 

 
Learned Docker image creation and deployment
 
 
- 
 

 
Understood Amazon ECR and ECS integration
 
 
- 
 

 
Configured Application Load Balancer
 
 
- 
 

 
Implemented ECS Service Auto Scaling
 
 
- 
 

 
Monitored infrastructure using CloudWatch
 
 
- 
 

 
Practiced debugging real AWS deployment issues
 
## Future Improvements
 
 
- 
 

 
Enable HTTPS using a custom domain and AWS Certificate Manager (ACM)
 
 
- 
 

 
Implement Blue/Green deployment strategy
 
## Project Status
 
Project completed successfully.
 
# AWS CI/CD Pipeline Project (Flask + Docker + ECS)
 
## Project Overview
 
This project demonstrates a complete CI/CD pipeline using AWS services to automate the build and deployment of a containerized Flask application.
 
A simple Python Flask application was containerized using Docker and deployed to Amazon ECS through a fully automated CI/CD pipeline using GitHub, AWS CodePipeline, and AWS CodeBuild.
 
The goal of this project was to understand end-to-end CI/CD workflow, Docker deployment, container orchestration, load balancing, auto scaling, monitoring, and troubleshooting real AWS deployment issues.
 
## Architecture Flow
 
Developer Push Code ↓ GitHub Repository ↓ AWS CodePipeline ↓ AWS CodeBuild ↓ Docker Image Build ↓ Amazon ECR ↓ Amazon ECS (Fargate) ↓ Application Load Balancer (ALB) ↓ Users ↓ CloudWatch Monitoring ↓ Auto Scaling
 
## Technologies Used
 
 
- 
 

 
Python Flask
 
 
- 
 

 
Docker
 
 
- 
 

 
GitHub
 
 
- 
 

 
AWS CodePipeline
 
 
- 
 

 
AWS CodeBuild
 
 
- 
 

 
Amazon ECR
 
 
- 
 

 
Amazon ECS (Fargate)
 
 
- 
 

 
Application Load Balancer (ALB)
 
 
- 
 

 
Amazon CloudWatch
 
 
- 
 

 
ECS Service Auto Scaling
 
## Deployment Workflow
 
 
1. 
 

 
Source code is pushed to GitHub
 
 
1. 
 

 
AWS CodePipeline detects code changes
 
 
1. 
 

 
AWS CodeBuild starts automatically
 
 
1. 
 

 
Docker image is built from the Flask application
 
 
1. 
 

 
Docker image is pushed to Amazon ECR
 
 
1. 
 

 
Amazon ECS pulls the latest image from ECR and deploys the container
 
 
1. 
 

 
Application Load Balancer routes user traffic to ECS tasks
 
 
1. 
 

 
Application becomes accessible through ALB DNS
 
## Buildspec Workflow
 
The build process is defined inside `buildspec.yml`, which is used by AWS CodeBuild.
 
### pre_build
 
 
- 
 

 
Authenticate Docker with Amazon ECR
 
 
- 
 

 
Prepare Amazon ECR repository URI
 
 
- 
 

 
Set Docker image tag
 
### build
 
 
- 
 

 
Build Docker image from Dockerfile
 
 
- 
 

 
Tag Docker image with latest tag
 
### post_build
 
 
- 
 

 
Push Docker image to Amazon ECR
 
 
- 
 

 
Generate `imagedefinitions.json` for Amazon ECS deployment
 
### artifacts
 
 
- 
 

 
Export `imagedefinitions.json`
 
 
- 
 

 
Pass artifact to Deploy stage in CodePipeline
 
## Project Structure
 
`. ├── app.py ├── Dockerfile ├── requirements.txt ├── buildspec.yml ├── screenshots/ │   ├── pipeline-success.png │   ├── deploy-troubleshooting.png │   ├── ecs-public-ip-app.png │   └── alb-running-app.png └── README.md `
 
## Docker Build Process
 
The application is containerized using Docker.
 
### Dockerfile Responsibilities
 
 
- 
 

 
Use Python base image
 
 
- 
 

 
Copy application files
 
 
- 
 

 
Install dependencies
 
 
- 
 

 
Run Flask application
 
## AWS Services Used
 
### AWS CodePipeline
 
Automates the complete CI/CD workflow.
 
### AWS CodeBuild
 
Builds Docker images and prepares deployment artifacts.
 
### Amazon ECR
 
Stores Docker container images securely.
 
### Amazon ECS (Fargate)
 
Runs the containerized Flask application.
 
### Application Load Balancer (ALB)
 
Distributes incoming user traffic across ECS tasks.
 
### Amazon CloudWatch
 
Monitors logs, metrics, and service health.
 
### ECS Auto Scaling
 
Automatically scales ECS tasks based on workload.
 
## Troubleshooting & Fixes
 
### 1. GitHub Source Connection Issue
 
**Issue:** GitHub source connection was not configured properly during pipeline creation.
 
**Fix:** Configured GitHub connection using AWS CodeConnections.
 
**Result:** Source stage connected successfully.
 
### 2. ECR Repository Configuration Issue
 
**Issue:** Incorrect Amazon ECR repository URI in `buildspec.yml`.
 
**Fix:** Configured the correct ECR repository URI and image tagging.
 
**Result:** Docker image pushed successfully to Amazon ECR.
 
### 3. Deploy Stage Failed
 
**Issue:** Build stage succeeded, but Deploy stage failed.
 
**Root Cause:** Deployment artifact configuration was incorrect. The container name inside `imagedefinitions.json` did not match the ECS task definition.
 
**Fix:** Updated `buildspec.yml` to generate correct `imagedefinitions.json` with proper container name and image URI.
 
Example: `[   {     "name": "aws-ci-demo-container",     "imageUri": "<ECR_IMAGE_URI>"   } ] ` **Result:** Deployment succeeded.
 
### 4. Deployment Delay During ECS Service Update
 
**Issue:** Deployment remained in progress longer than expected.
 
**Root Cause:** Amazon ECS waits for new tasks to pass health checks and reach steady state before completing deployment.
 
**Fix:** Verified task status, service events, and deployment health.
 
**Result:** Deployment completed successfully.
 
### 5. Security Group / ALB Access Issue
 
**Issue:** Application deployed successfully but was not accessible through ALB DNS.
 
**Root Cause:** Security group inbound rules were not configured correctly.
 
**Fix:** Updated security groups:
 
 
- 
 

 
ALB security group allowed inbound traffic on port 80
 
 
- 
 

 
ECS task security group allowed traffic from ALB on port 5000
 
**Result:** Application became accessible through ALB DNS.
 
## Application Access
 
Application was successfully accessible through ALB DNS.
 
Example: `http://<alb-dns-name> `
 
## Screenshots
 
### Pipeline Success
 
Pipeline Success
 
### Deployment Troubleshooting
 
Deployment Troubleshooting
 
### ECS Public IP Running Application
 
ECS Public IP
 
### ALB DNS Running Application
 
ALB Running App
 
## Key Learnings
 
 
- 
 

 
Built end-to-end AWS CI/CD pipeline
 
 
- 
 

 
Learned Docker image creation and deployment
 
 
- 
 

 
Understood Amazon ECR and ECS integration
 
 
- 
 

 
Configured Application Load Balancer
 
 
- 
 

 
Implemented ECS Service Auto Scaling
 
 
- 
 

 
Monitored infrastructure using CloudWatch
 
 
- 
 

 
Practiced debugging real AWS deployment issues
 
## Future Improvements
 
 
- 
 

 
Enable HTTPS using a custom domain and AWS Certificate Manager (ACM)
 
 
- 
 

 
Implement Blue/Green deployment strategy
 
## Project Status
 
Project completed successfully.
 

