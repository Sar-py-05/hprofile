# Architecture Document

## Project Name

HProfile Application CI/CD Deployment on AWS ECS Fargate

---

# 1. Solution Overview

This project implements a complete CI/CD pipeline for the HProfile Java web application using GitHub Actions, SonarCloud, Docker, Amazon ECR, Amazon ECS Fargate, Amazon RDS, and Application Load Balancer.

The pipeline automatically:

1. Pulls source code from GitHub
2. Executes Maven build and tests
3. Runs SonarCloud static code analysis
4. Generates JaCoCo coverage reports
5. Performs Checkstyle validation
6. Builds a Docker image
7. Pushes the image to Amazon ECR
8. Updates ECS Task Definition
9. Deploys the new application version to ECS Fargate
10. Serves traffic through an Application Load Balancer

---

# 2. High-Level Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions Pipeline
    │
    ├── Maven Build
    ├── Unit Tests
    ├── JaCoCo Coverage
    ├── Checkstyle
    └── SonarCloud Analysis
    │
    ▼
Docker Build
    │
    ▼
Amazon ECR
    │
    ▼
Amazon ECS Fargate
    │
    ▼
Application Load Balancer
    │
    ▼
End Users

ECS Tasks
    │
    ▼
Amazon RDS MySQL
```

---

# 3. CI/CD Workflow Architecture

## Stage 1 – Testing

### Activities

* Checkout source code
* Configure Java 11
* Execute Maven Verify
* Run Unit Tests
* Generate JaCoCo Reports
* Execute Checkstyle
* Perform SonarCloud Scan

### Tools Used

* GitHub Actions
* Maven
* SonarCloud
* JaCoCo
* Checkstyle

---

## Stage 2 – Build and Publish

### Activities

* Update application.properties
* Build Docker Image
* Tag image with GitHub Run Number
* Push image to Amazon ECR

### Tools Used

* Docker
* Amazon ECR

---

## Stage 3 – Deploy

### Activities

* Render ECS Task Definition
* Update container image URI
* Register new task definition revision
* Deploy updated service
* Wait for service stability

### Tools Used

* ECS Deploy Action
* Amazon ECS
* AWS IAM

---

# 4. Application Architecture

The application is a Java-based web application packaged as a WAR file and deployed inside a Tomcat container.

## Application Components

### Frontend

* JSP Pages
* Spring MVC Controllers

### Business Layer

* Spring Framework
* Service Classes

### Persistence Layer

* Spring Data JPA
* Hibernate

### Database

* Amazon RDS MySQL

---

# 5. Container Architecture

## Build Stage

```dockerfile
FROM maven:3.8.6-openjdk-11
```

Responsibilities:

* Compile source code
* Execute Maven build
* Generate WAR package

---

## Runtime Stage

```dockerfile
FROM tomcat:9-jre11
```

Responsibilities:

* Host application
* Serve HTTP requests
* Execute business logic

---

# 6. AWS Infrastructure Inventory

---

## ECS Cluster

Name:

```text
vproapp-act
```

Capacity Providers:

```text
FARGATE
FARGATE_SPOT
```

Purpose:

Hosts ECS services and running tasks.

---

## ECS Service

Name:

```text
vproapp-act-svc
```

Configuration:

```text
Desired Tasks: 2
Launch Type: FARGATE
```

Purpose:

Maintains required number of application containers.

---

## ECS Task Definition

Family:

```text
vroapp-act-tdef
```

Resources:

```text
CPU: 1024
Memory: 3072 MB
```

Purpose:

Defines container configuration and runtime requirements.

---

## Amazon ECR

Repository:

```text
actapp
```

Purpose:

Stores versioned Docker container images.

---

## Amazon RDS

Engine:

```text
MySQL
```

Database:

```text
accounts
```

Purpose:

Stores application data.

---

## Application Load Balancer

Name:

```text
vproapp-act-elb
```

Scheme:

```text
Internet Facing
```

Purpose:

Distributes traffic across ECS tasks.

---

## Target Group

Name:

```text
vproapp-act-tg
```

Health Check:

```text
/login
```

Purpose:

Routes requests only to healthy containers.

---

## Security Group

Name:

```text
vproapp-act-svc-sg
```

Allowed Inbound Ports:

```text
80
443
8080
```

Purpose:

Controls network access.

---

## CloudWatch Logs

Log Group:

```text
/ecs/vroapp-act-tdef
```

Purpose:

Stores container logs and deployment events.

---

## SSL Certificate

Service:

```text
AWS Certificate Manager
```

Purpose:

Provides HTTPS encryption.

---

# 7. Networking Architecture

## VPC

```text
vpc-014f8a9c302aaa05a
```

All infrastructure resources reside inside this VPC.

---

## Public Subnets

```text
subnet-03af1a9bdddd62b40
subnet-0e25182b111a433d5
subnet-0a5aad9df36fe4388
subnet-0d8a76e626766ef6d
```

Distributed across multiple Availability Zones for high availability.

---

# 8. Security Architecture

## GitHub Secrets

Sensitive values are stored in GitHub Secrets:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
REGISTRY

RDS_USER
RDS_PASS
RDS_ENDPOINT

SONAR_TOKEN
SONAR_PROJECT_KEY
SONAR_ORGANIZATION
SONAR_URL
```

---

## IAM Roles

### ECS Task Execution Role

Responsibilities:

* Pull images from ECR
* Publish logs to CloudWatch
* Start ECS tasks

---

# 9. Deployment Flow

```text
Git Push
   │
   ▼
GitHub Actions
   │
   ▼
Maven Build
   │
   ▼
SonarCloud Analysis
   │
   ▼
Docker Build
   │
   ▼
Amazon ECR
   │
   ▼
ECS Task Definition Update
   │
   ▼
ECS Service Deployment
   │
   ▼
Application Load Balancer
   │
   ▼
End User Access
```

---

# 10. High Availability Design

The application is deployed using:

* Multiple Availability Zones
* Two ECS Tasks
* Application Load Balancer
* Health Checks
* Automatic Task Replacement

Benefits:

* Fault tolerance
* Improved availability
* Reduced downtime
* Automated recovery

---

# 11. Monitoring Strategy

Monitoring Components:

* ECS Service Health
* Target Group Health Checks
* CloudWatch Logs
* SonarCloud Quality Reports
* GitHub Actions Workflow Logs

---

# 12. Future Enhancements

Planned Improvements:

* Infrastructure as Code using Terraform
* Blue/Green Deployments
* ECS Auto Scaling
* AWS Secrets Manager
* Trivy Container Scanning
* Prometheus Monitoring
* Grafana Dashboards
* Slack Notifications
* Multi-Environment Deployment Strategy

---

# Architecture Summary

This project demonstrates an end-to-end DevOps implementation using modern cloud-native deployment practices. The architecture provides automated build, test, code quality validation, containerization, image management, and production deployment capabilities using AWS managed services and GitHub Actions.

