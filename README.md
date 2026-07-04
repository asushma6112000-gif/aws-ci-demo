# AWS CI/CD Pipeline Project (Flask + Docker + ECS)

## Architecture Diagram

![Architecture Diagram](screenshots/aws-cicd-architecture.png)

## Project Overview

This project demonstrates a complete CI/CD pipeline using AWS services to automate the build and deployment of a containerized Flask application.

A simple Python Flask application was containerized using Docker and deployed to Amazon ECS through a fully automated CI/CD pipeline using GitHub, AWS CodePipeline, and AWS CodeBuild.

The goal of this project was to understand end-to-end CI/CD workflow, Docker deployment, container orchestration, load balancing, auto scaling, monitoring, and troubleshooting real AWS deployment issues.

## Architecture Flow

Developer Push Code → GitHub Repository → AWS CodePipeline → AWS CodeBuild → Docker Image Build → Amazon ECR → Amazon ECS (Fargate) → Application Load Balancer (ALB) → Users → CloudWatch Monitoring → ECS Auto Scaling

## Technologies Used

- Python Flask  
- Docker  
- GitHub  
- AWS CodePipeline  
- AWS CodeBuild  
- Amazon ECR  
- Amazon ECS (Fargate)  
- Application Load Balancer (ALB)  
- Amazon CloudWatch  
- ECS Service Auto Scaling  

## Deployment Workflow

1. Source code pushed to GitHub  
2. AWS CodePipeline detects changes automatically  
3. AWS CodeBuild starts build process  
4. Docker image is built from Flask application  
5. Image is pushed to Amazon ECR  
6. ECS pulls latest image and deploys container  
7. Application Load Balancer routes traffic to ECS tasks  
8. Application becomes accessible via ALB DNS  

## Buildspec Workflow

### pre_build
- Authenticate Docker with Amazon ECR  
- Set ECR repository URI  
- Define Docker image tag  

### build
- Build Docker image using Dockerfile  
- Tag image as latest  

### post_build
- Push Docker image to Amazon ECR  
- Generate imagedefinitions.json for ECS deployment  

### artifacts
- Pass build artifact to ECS deploy stage  

## Project Structure

