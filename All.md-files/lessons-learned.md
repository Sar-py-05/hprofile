# Lessons Learned

## GitHub Actions

* Always push workflow changes before testing.
* Verify branch and workflow files are synchronized.
* Use job dependencies with `needs`.

---

## SonarCloud

* Disable Automatic Analysis when using CI analysis.
* Validate project key and organization key.
* Test Sonar independently before adding Quality Gates.

---

## JaCoCo

* Generate XML reports, not only HTML reports.
* SonarCloud consumes XML coverage reports.
* Verify report paths carefully.

---

## Checkstyle

* Introduce Checkstyle gradually.
* Legacy projects often generate hundreds of warnings.
* Use warnings initially instead of failing builds.

---

## Docker

* Public images can become deprecated.
* Use actively maintained base images.
* Multi-stage builds reduce final image size.

---

## Amazon ECR

* Repository names must match exactly.
* Verify repository existence before pipeline integration.
* Store image tags using GitHub run numbers.

---

## Amazon ECS

* Export task definitions from ECS before automation.
* Container name must exactly match task definition.
* Service name and cluster name must match AWS resources.

---

## IAM

* ECS deployment failures frequently originate from IAM role issues.
* Execution Role and Task Role must be valid.
* Least privilege should be implemented after validation.

---

## Amazon RDS

* Confirm database existence before imports.
* Validate security groups and connectivity first.
* Test manual connectivity before application deployment.

---

## Debugging Strategy

Most issues were solved by:

1. Reading the exact error message.
2. Verifying assumptions.
3. Testing manually.
4. Fixing one problem at a time.
5. Re-running only after verification.

---

## Project Milestones Achieved

Successfully implemented:

* GitHub Actions CI/CD
* SonarCloud Integration
* JaCoCo Coverage Reporting
* Checkstyle Analysis
* Docker Containerization
* Amazon ECR Integration
* Amazon ECS Fargate Deployment
* Application Load Balancer
* Amazon RDS Integration
* Automated Production Deployment

---

## Key Takeaway

A working CI/CD pipeline is built incrementally. Small verified steps, careful troubleshooting, and thorough validation are more effective than making multiple changes simultaneously.

