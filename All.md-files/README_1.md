# HProfile CI/CD Deployment on AWS ECS Fargate

## Project Overview

This project demonstrates a complete DevOps CI/CD pipeline for deploying the HProfile Java web application using GitHub Actions, SonarCloud, Docker, Amazon ECR, Amazon ECS Fargate, Amazon RDS, and Application Load Balancer.

The objective of this project was to implement an enterprise-style automated delivery pipeline capable of:

* Building Java applications using Maven
* Executing automated tests
* Performing static code analysis using SonarCloud
* Measuring code coverage using JaCoCo
* Enforcing coding standards with Checkstyle
* Building Docker container images
* Publishing container images to Amazon ECR
* Deploying containerized workloads to Amazon ECS Fargate
* Using Amazon RDS as the backend database
* Exposing the application through an Application Load Balancer
* Enabling HTTPS using AWS Certificate Manager

---

## Technology Stack

### Source Control

* Git
* GitHub

### CI/CD

* GitHub Actions

### Build Tools

* Maven
* Java 11

### Code Quality

* SonarCloud
* JaCoCo
* Checkstyle

### Containerization

* Docker

### Container Registry

* Amazon Elastic Container Registry (ECR)

### Container Orchestration

* Amazon ECS Fargate

### Database

* Amazon RDS MySQL

### Networking

* VPC
* Subnets
* Security Groups
* Application Load Balancer

### Monitoring

* CloudWatch Logs

### SSL/TLS

* AWS Certificate Manager (ACM)

---

## CI/CD Pipeline Stages

### Stage 1 – Testing

* Checkout source code
* Configure Java 11
* Execute Maven Verify
* Run Unit Tests
* Generate JaCoCo Coverage Reports
* Execute Checkstyle Analysis
* Perform SonarCloud Scan

### Stage 2 – Build and Publish

* Update application configuration
* Build Docker Image
* Tag image using GitHub Run Number
* Push image to Amazon ECR

### Stage 3 – Deploy

* Render ECS Task Definition
* Replace image reference
* Register new ECS Task Definition Revision
* Update ECS Service
* Deploy new container version

---

## Repository Structure

```text
.
├── .github/workflows/
├── aws-files/
├── docs/
├── src/
├── Dockerfile
├── pom.xml
├── sonar-project.properties
└── README.md
```

---

## Documentation

Additional project documentation is available:

* architecture.md
* deployment-guide.md
* runbook.md
* troubleshooting-guide.md
* lessons-learned.md

---

## Key Achievements

* Automated build and deployment pipeline
* SonarCloud integration with quality checks
* JaCoCo coverage reporting
* Checkstyle integration
* Docker image publishing to ECR
* ECS Fargate deployment automation
* RDS database integration
* HTTPS-enabled load balancing
* Infrastructure validation and troubleshooting

---

## Future Improvements

* Infrastructure as Code using Terraform
* Blue/Green ECS deployments
* Automated rollback strategy
* Slack notifications
* Security scanning using Trivy
* Dependabot integration
* Prometheus and Grafana monitoring
* Multi-environment deployments (Dev, QA, Prod)

---

## Author

Abhishek Roy
AWS ECS Fargate CI/CD Deployment using GitHub Actions

