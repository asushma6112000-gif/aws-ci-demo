# AWS CI/CD Pipeline Project (Flask + Docker + ECS)

## Project Overview

This project demonstrates a complete CI/CD pipeline using AWS services.  
A simple Flask application was containerized using Docker and automatically deployed to Amazon ECS using AWS CodePipeline and AWS CodeBuild.

The goal of this project was to build and understand the end-to-end CI/CD workflow, Docker deployment, and troubleshoot real deployment issues in AWS.

---

## Architecture Diagram

![Architecture Diagram](screenshots/architecture-diagram.png)

### Architecture Flow

```text
Developer Push Code
        ↓
GitHub Repository
        ↓
AWS CodePipeline
        ↓
AWS CodeBuild
        ↓
Docker Image Build
        ↓
Amazon ECR
        ↓
Amazon ECS
        ↓
Flask Application (Public IP:5000)
```

---

## Technologies Used

- Python Flask
- Docker
- GitHub
- AWS CodePipeline
- AWS CodeBuild
- Amazon ECR
- Amazon ECS

---

## Deployment Workflow

1. Source code is pushed to GitHub  
2. AWS CodePipeline detects code changes  
3. CodeBuild starts automatically  
4. Docker image is built from the Flask application  
5. Docker image is pushed to Amazon ECR  
6. Amazon ECS pulls the latest image from ECR  
7. ECS deploys the updated container  
8. Application becomes accessible through ECS public IP on port 5000  

---

## Project Structure

```bash
.
├── app.py
├── Dockerfile
├── requirements.txt
├── buildspec.yml
├── screenshots/
│   ├── architecture-diagram.png
│   ├── pipeline-success.png
│   ├── deploy-troubleshooting.png
│   └── ecs-running-app.png
└── README.md
```

---

## Docker Build Process

The application is containerized using Docker.

### Dockerfile Responsibilities

- Use Python base image
- Copy application files
- Install dependencies
- Expose application port
- Run Flask application

---

## AWS Services Used

### AWS CodePipeline
Automates the complete CI/CD workflow.

### AWS CodeBuild
Builds Docker images and pushes them to Amazon ECR.

### Amazon ECR
Stores Docker container images securely.

### Amazon ECS
Runs the containerized Flask application.

---

## Troubleshooting & Fixes

### 1. Source Provider Configuration Issue

**Issue:**  
GitHub Version 2 was not visible during pipeline creation.

**Fix:**  
Used GitHub connection via GitHub App / CodeConnections.

**Result:**  
Source stage was configured successfully.

---

### 2. Build Stage Configuration Confusion

**Issue:**  
There was confusion between Commands and Other Build Providers during Build stage configuration.

**Fix:**  
Selected AWS CodeBuild and configured build settings correctly.

**Result:**  
Build stage started successfully.

---

### 3. Artifact Selection Confusion

**Issue:**  
There was confusion between Source Artifact and Build Artifact.

**Fix:**  
Used:
- Source Artifact as input for Build stage
- Build Artifact as input for Deploy stage

**Result:**  
Pipeline stages connected correctly.

---

### 4. Deploy Stage Failed

**Issue:**  
Source and Build stages succeeded, but Deploy stage failed.

**Root Cause:**  
Container name inside `imagedefinitions.json` did not match ECS task definition.

**Fix:**  
Updated container name to match ECS exactly.

Example:

```json
[
  {
    "name": "aws-ci-demo-container",
    "imageUri": "<ECR_IMAGE_URI>"
  }
]
```

**Result:**  
Deployment moved forward successfully.

---

### 5. ECR Repository Configuration Issue

**Issue:**  
Confusion while adding the correct Amazon ECR repository URI in `buildspec.yml`.

**Fix:**  
Configured correct ECR repository URI and image tagging.

**Result:**  
Docker image pushed successfully to Amazon ECR.

---

### 6. Deploy Stage Took Long Time

**Issue:**  
Deploy stage remained in progress for several minutes.

**Root Cause:**  
Amazon ECS waits for the new task to become healthy and the service to reach steady state.

**Fix:**  
Verified:
- ECS task status = RUNNING
- Deployment status
- ECS service events

**Result:**  
Deployment completed successfully.

---

### 7. Application Accessibility Verification

**Issue:**  
Needed to verify whether Flask application was accessible.

**Fix:**  
Verified:
- Security group allowed inbound traffic on port 5000
- Flask application listens on:

```python
app.run(host="0.0.0.0", port=5000)
```

**Result:**  
Application became accessible via ECS public IP.

---

## Application Access

Application was successfully accessible through ECS Public IP.

Example:

```bash
http://<public-ip>:5000
```

---

## Screenshots

### Pipeline Success
![Pipeline Success](screenshots/pipeline-success.png)

### Deployment Troubleshooting
![Deployment Troubleshooting](screenshots/deploy-troubleshooting.png)

### ECS Public IP Running Application
![ECS Running App](screenshots/ecs-running-app.png)

---

## Key Learnings

- Built end-to-end AWS CI/CD pipeline
- Learned Docker image creation and deployment
- Understood Amazon ECR and ECS integration
- Practiced debugging real AWS deployment issues
- Gained hands-on DevOps troubleshooting experience

---

## Future Improvements

- Add Application Load Balancer
- Configure Auto Scaling
- Enable HTTPS
- Add CloudWatch Monitoring

---

## Project Status

Project completed successfully.
