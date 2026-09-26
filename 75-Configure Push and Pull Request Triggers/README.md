# Task 75: Configure Push and Pull Request Triggers

**Platform:** KodeKloud
**Category:** GitHub Actions  
**Project:** CloudNexus DevSecOps

---

## Challenge

Configure a GitHub Actions workflow so that CI runs for both pushes to main and pull requests targeting main. Use this to understand how CI can validate changes before they are merged.

> **Note:** This is a self-designed KodeKloud-style practice task created from the DevSecOps and AWS work already completed in the CloudNexus project. It is not presented as an official KodeKloud challenge.

---

## Task Requirements

- Run on `push` to `main`.
- Run on `pull_request` targeting `main`.
- Use one reusable CI job.
- Verify both event types from the Actions tab.

---

## Objective

The goal of this challenge is to extend the CloudNexus project with a practical DevOps capability and understand how it fits into a real CI/CD or monitoring workflow.

---

## Implementation

```text
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

### Important

Replace placeholders such as `<INSTANCE_ID>`, `<SNS_TOPIC_ARN>`, `<EC2_HOST>`, `<START_TIME>`, `<END_TIME>`, and secret names with values from the environment used for the lab.

Never commit private keys, passwords, access tokens, or AWS credentials to GitHub.

---

## Verification

Use the appropriate checks for the task:

```bash
# GitHub Actions
Check the repository Actions tab and confirm the workflow completed successfully.

# Docker
docker images
docker ps

# AWS
aws sts get-caller-identity

# CloudWatch
aws cloudwatch describe-alarms

# Prometheus
curl http://localhost:9090/-/healthy

# Node Exporter
curl http://localhost:9100/metrics
```

For tasks that use only GitHub UI or AWS Console, verify the corresponding workflow run, resource, metric, alarm, dashboard, or log entry from the console.

---

## What I Learned

- How github actions fits into the CloudNexus DevSecOps workflow.
- How automation reduces repetitive manual work.
- How to verify infrastructure and deployment changes instead of assuming they worked.
- How secure configuration should be separated from application code.
- How monitoring and CI/CD together improve operational visibility.

---

## Real-World Relevance

This challenge builds on the CloudNexus pipeline, which already uses GitHub Actions, SonarCloud, Trivy, Docker, SSH, and AWS EC2. The next step is to make the pipeline more reusable, secure, observable, and reliable.

The overall flow is:

```text
Developer Push
      |
      v
GitHub Actions
      |
      +---- Code Validation
      |
      +---- SonarCloud
      |
      +---- Trivy
      |
      +---- Docker Build
      |
      v
AWS EC2 Deployment
      |
      +---- CloudWatch
      |
      +---- Prometheus
      |
      v
Grafana / Alerts
```

---

## Key Takeaways

- CI/CD should validate changes before deployment.
- Secrets should never be hard-coded.
- Security scans are more useful when they can enforce a deployment policy.
- Immutable Docker image tags make deployments traceable and rollback easier.
- Monitoring provides visibility after deployment, completing the DevOps lifecycle.

---

## Result

The CloudNexus project was extended with the capability covered in **Day 75** and verified using the relevant GitHub Actions, Docker, AWS, Prometheus, or Grafana checks.

---
