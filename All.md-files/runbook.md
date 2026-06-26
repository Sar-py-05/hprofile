# Operations Runbook

## Purpose

This runbook provides operational procedures for maintaining the VProfile ECS application.

---

# Daily Health Checks

## ECS Service

Verify service status.

```bash
aws ecs describe-services \
--cluster vproapp-act \
--services vproapp-act-svc
```

Expected:

```text
Running Count = Desired Count
```

---

## ECS Tasks

Verify tasks.

```bash
aws ecs list-tasks \
--cluster vproapp-act
```

Expected:

```text
2 Running Tasks
```

---

## Target Group

Check target health.

AWS Console:

Target Groups → vproapp-act-tg

Expected:

```text
Healthy
```

---

## Load Balancer

Verify application availability.

```bash
curl http://ALB-DNS/login
```

Expected:

```text
HTTP 200
```

---

## RDS Connectivity

Connect to database.

```bash
mysql -h <endpoint> -u admin -p accounts
```

Check tables:

```sql
show tables;
```

---

# Deployment Procedure

Deployment is performed automatically through GitHub Actions.

Workflow:

```text
Testing
→ Build_and_Publish
→ Deploy
```

---

# Incident Response

## Service Down

Check:

1. ECS Tasks
2. Target Group Health
3. CloudWatch Logs
4. RDS Availability

---

## Container Crash

View logs:

CloudWatch

```text
/ecs/vroapp-act-tdef
```

Investigate stack trace.

---

## Database Failure

Verify:

```bash
telnet <rds-endpoint> 3306
```

Check:

* Security Groups
* RDS Status
* Credentials

---

# Scaling

Increase task count.

AWS Console:

ECS Service

```text
Desired Count = 4
```

Or:

```bash
aws ecs update-service \
--desired-count 4
```

---

# Backup

Database backups handled by Amazon RDS automated backups.

Verify retention policy periodically.

---

# Recovery

In case of deployment failure:

1. Roll back task definition
2. Force new deployment
3. Validate target group health
4. Confirm application availability

