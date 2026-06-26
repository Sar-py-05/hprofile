# Troubleshooting Guide

## SonarCloud Issues

### Automatic Analysis Enabled

Error:

```text
You are running CI analysis while Automatic Analysis is enabled
```

Solution:

Disable Automatic Analysis in SonarCloud.

---

### Invalid Sonar Token

Error:

```text
Not authorized
```

Solution:

Generate a new token.

Update GitHub Secret:

```text
SONAR_TOKEN
```

---

### Project Not Analyzed

Cause:

Incorrect project key.

Solution:

Verify:

```text
SONAR_PROJECT_KEY
SONAR_ORGANIZATION
```

---

# JaCoCo Issues

### Coverage Not Showing

Verify:

```bash
find target -name jacoco.xml
```

Expected:

```text
target/site/jacoco/jacoco.xml
```

---

### Sonar Coverage Missing

Verify:

```properties
sonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
```

---

# Checkstyle Issues

### checkstyle-result.xml Missing

Verify:

```bash
find target -name checkstyle-result.xml
```

Expected:

```text
target/checkstyle-result.xml
```

---

### Indentation Warnings

Example:

```text
incorrect indentation level
```

Impact:

Build succeeds but warnings appear.

---

# GitHub Actions Issues

### Workflow Not Updated

Cause:

Changes not pushed.

Verify:

```bash
git status
git push origin main
```

---

### Secret Not Found

Error:

```text
SONAR_TOKEN_PRESENT=NO
```

Solution:

Create missing secret in GitHub.

---

# Docker Issues

### OpenJDK Image Not Found

Error:

```text
openjdk:11 not found
```

Solution:

Use:

```dockerfile
FROM maven:3.9-eclipse-temurin-11
```

---

# ECR Issues

### Repository Does Not Exist

Error:

```text
repository does not exist
```

Cause:

Typo in repository name.

Example:

```text
acctapp
```

Correct:

```text
actapp
```

---

### Push Failed

Verify repository.

```bash
aws ecr describe-repositories \
--repository-names actapp \
--region us-east-1
```

---

# ECS Issues

### Task Definition File Not Found

Error:

```text
Task definition file does not exist
```

Cause:

Incorrect filename.

Expected:

```text
aws-files/taskdeffile.json
```

---

### Role Is Not Valid

Error:

```text
Failed to register task definition
Role is not valid
```

Cause:

Invalid IAM role.

Solution:

Use valid execution role ARN.

---

### ECS Deployment Failure

Check:

* Service Events
* CloudWatch Logs
* Target Group Health

---

# RDS Issues

### Unknown Database

Error:

```text
Unknown database 'accounts'
```

Solution:

Create database.

```sql
CREATE DATABASE accounts;
```

---

### Database Import Failure

Cause:

Wrong path.

Verify:

```bash
src/main/resources/db_backup.sql
```

Import:

```bash
mysql -h endpoint -u admin -p accounts < src/main/resources/db_backup.sql
```

---

# Quality Gate Failure

Error:

```text
Quality Gate has FAILED
```

Cause:

Coverage threshold not met.

Temporary Fix:

Reduce coverage threshold during project setup.

Long-Term Fix:

Increase test coverage.

