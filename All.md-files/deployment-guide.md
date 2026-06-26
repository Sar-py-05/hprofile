# Deployment Guide

## Project Overview

This project implements a complete CI/CD pipeline for deploying the VProfile application on AWS ECS Fargate using GitHub Actions.

The deployment workflow includes:

1. Source Code Management using GitHub
2. Maven Build and Unit Testing
3. SonarCloud Code Analysis
4. JaCoCo Code Coverage Reporting
5. Checkstyle Static Analysis
6. Docker Image Build
7. Amazon ECR Image Storage
8. Amazon ECS Fargate Deployment
9. Application Load Balancer Integration
10. Amazon RDS MySQL Backend

---

# Prerequisites

## AWS Resources

### ECS Cluster

vproapp-act

### ECS Service

vproapp-act-svc

### ECS Task Definition

vroapp-act-tdef

### ECR Repository

actapp

### RDS Database

accounts

### Load Balancer

vproapp-act-elb

---

# GitHub Secrets Required

| Secret Name           | Description        |
| --------------------- | ------------------ |
| AWS_ACCESS_KEY_ID     | AWS Access Key     |
| AWS_SECRET_ACCESS_KEY | AWS Secret Key     |
| REGISTRY              | ECR Registry URL   |
| RDS_USER              | Database Username  |
| RDS_PASS              | Database Password  |
| RDS_ENDPOINT          | RDS Endpoint       |
| SONAR_TOKEN           | SonarCloud Token   |
| SONAR_URL             | SonarCloud URL     |
| SONAR_PROJECT_KEY     | Sonar Project Key  |
| SONAR_ORGANIZATION    | Sonar Organization |

---

# Deployment Workflow

## Stage 1: Testing

Executed by GitHub Actions.

Tasks:

* Checkout Source Code
* Setup Java 11
* Maven Verify
* Sonar Scanner
* SonarCloud Analysis

Command:

```bash
mvn clean verify
```

---

## Stage 2: Build and Publish

Tasks:

* Update application.properties
* Build Docker Image
* Push Docker Image to Amazon ECR

Image Tag Format:

```text
latest
github-run-number
```

Example:

```text
actapp:latest
actapp:45
```

---

## Stage 3: Deploy

Tasks:

* Render ECS Task Definition
* Replace Image URI
* Register New Task Revision
* Update ECS Service
* Wait for Service Stabilization

Deployment Tool:

```yaml
aws-actions/amazon-ecs-render-task-definition
aws-actions/amazon-ecs-deploy-task-definition
```

---

# Verify Deployment

## ECS Service

```bash
aws ecs describe-services \
--cluster vproapp-act \
--services vproapp-act-svc
```

---

## Check Running Tasks

```bash
aws ecs list-tasks \
--cluster vproapp-act
```

---

## Verify ECR Image

```bash
aws ecr list-images \
--repository-name actapp
```

---

## Verify Application

Open:

http://<ALB-DNS>/login

Example:

http://vproapp-act-elb-xxxxxxxx.us-east-1.elb.amazonaws.com/login

---

# Rollback Procedure

## Find Previous Task Revision

```bash
aws ecs list-task-definitions \
--family-prefix vroapp-act-tdef
```

---

## Update Service

Select an older revision and update service.

```bash
aws ecs update-service \
--cluster vproapp-act \
--service vproapp-act-svc \
--task-definition vroapp-act-tdef:1
```

---

# Deployment Success Criteria

The deployment is considered successful when:

* Pipeline completes successfully
* ECS service becomes stable
* Two healthy tasks are running
* Target group health checks pass
* Application login page loads successfully

