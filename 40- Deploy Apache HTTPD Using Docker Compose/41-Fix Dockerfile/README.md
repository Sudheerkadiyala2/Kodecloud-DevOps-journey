# Day 41: Fix Dockerfile Build Failure and Create Custom Apache Image

**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Troubleshooting & Image Creation

---

## Challenge

The Nautilus DevOps team was tasked with creating a custom Docker image based on requirements provided by the development team.

A team member had already created a Dockerfile on **Application Server 1**, but the Docker image build was failing due to configuration issues.

### Task Requirements

- Dockerfile located at:

```bash
/opt/docker/Dockerfile
```

- Identify and fix the Docker build issues.
- Ensure the image builds successfully.
- Do not modify:
  - Base image
  - Existing valid configurations
  - Application data files (e.g., `index.html`)
- Build the image successfully using the corrected Dockerfile.

---

## Objective

Troubleshoot and fix Dockerfile errors without altering the intended application configuration.

The goal was to:

- Analyze build failures.
- Correct file path issues.
- Preserve application content and configurations.
- Successfully build the custom Apache image.

---

## Why This Challenge Matters

Docker image creation often fails because of:

- Incorrect file paths
- Missing assets
- Invalid COPY instructions
- Misconfigured build contexts

In production environments, DevOps engineers spend significant time troubleshooting build pipelines and Dockerfiles.

This challenge simulated a real-world debugging scenario where existing configurations had to be preserved while fixing build failures.

---

## Investigation Process

### Navigate to Docker Project

```bash
cd /opt/docker
```

### Review Directory Structure

```bash
ls -R
```

Discovered the following layout:

```text
/opt/docker
├── Dockerfile
├── certs
│   ├── server.crt
│   └── server.key
└── html
    └── index.html
```

---

## Root Cause Analysis

The Docker build was failing because the Dockerfile expected files in locations different from the actual directory structure.

Incorrect assumptions:

```dockerfile
COPY server.crt ...
COPY server.key ...
COPY index.html ...
```

Actual locations:

```text
certs/server.crt
certs/server.key
html/index.html
```

As a result, Docker could not locate the required files during build time.

---

## Fix Applied

Updated the COPY instructions to match the existing folder structure.

### SSL Certificate Files

```dockerfile
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
```

### Web Content

```dockerfile
COPY html/index.html /usr/local/apache2/htdocs/
```

---

## Existing Configuration Preserved

The following valid configurations were left unchanged:

### Base Image

```dockerfile
FROM httpd:latest
```

### Apache Listening Port

```apache
Listen 8080
```

### SSL Module Configuration

```apache
LoadModule ssl_module modules/mod_ssl.so
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so
```

### SSL Configuration Include

```apache
Include conf/extra/httpd-ssl.conf
```

### Application Content

```text
html/index.html
```

No changes were made to application files or valid Apache configurations.

---

## Build the Image

```bash
docker build -t custom-apache-image .
```

Expected Output:

```text
Successfully built xxxxxxxxxxxx
Successfully tagged custom-apache-image:latest
```

---

## Verification

### Verify Image Exists

```bash
docker images
```

Expected:

```text
REPOSITORY            TAG       IMAGE ID
custom-apache-image   latest    xxxxxxxxxxxx
```

### Verify Build Success

```bash
docker build .
```

Expected:

```text
BUILD SUCCESSFUL
```

No COPY-related errors should appear.

---

## What I Learned

This challenge reinforced the importance of understanding Docker build contexts.

A Docker build can fail even when files exist if:

- Relative paths are incorrect.
- Directory structures are misunderstood.
- COPY instructions don't match the actual file locations.

Successful troubleshooting requires validating both:

- Dockerfile instructions
- Host filesystem layout

---

## Key Concepts

### Docker Build Context

The build context is the set of files available to Docker during image creation.

Example:

```bash
docker build .
```

The current directory becomes the build context.

Only files inside that context can be copied into the image.

---

### COPY Instruction

Used to transfer files from the host into the image.

Example:

```dockerfile
COPY html/index.html /usr/local/apache2/htdocs/
```

Syntax:

```dockerfile
COPY <source> <destination>
```

---

### Apache SSL Configuration

SSL support requires:

```apache
mod_ssl
```

and

```apache
mod_socache_shmcb
```

These modules enable HTTPS communication and SSL session caching.

---

### Docker Troubleshooting Workflow

```text
Docker Build Failure
        |
        v

Read Error Logs
        |
        v

Inspect Dockerfile
        |
        v

Verify File Paths
        |
        v

Correct COPY Instructions
        |
        v

Rebuild Image
        |
        v

Successful Build
```

---

## Useful Commands

View project structure:

```bash
ls -R /opt/docker
```

View Dockerfile:

```bash
cat /opt/docker/Dockerfile
```

Build image:

```bash
docker build -t custom-apache-image .
```

List images:

```bash
docker images
```

Run container:

```bash
docker run -d -p 8080:8080 custom-apache-image
```

View logs:

```bash
docker logs <container-id>
```

Remove image:

```bash
docker rmi custom-apache-image
```

---

## Real-World Relevance

Docker build troubleshooting is a daily responsibility for DevOps engineers.

Common scenarios include:

- CI/CD pipeline failures
- Missing configuration files
- Incorrect artifact paths
- SSL certificate packaging
- Application deployment errors

The ability to diagnose and fix Dockerfile issues quickly is essential for maintaining deployment reliability.

---

## Key Takeaways

- Docker COPY paths must match the actual filesystem structure.
- Build context determines what Docker can access.
- Troubleshooting begins with reading error messages carefully.
- Existing configurations should be preserved whenever possible.
- Small path mistakes can completely break image builds.
- Successful image creation depends on both Dockerfile correctness and project organization.

---

## Final Thoughts

This challenge focused on Docker troubleshooting rather than Docker creation.

By carefully analyzing the project structure and correcting the Dockerfile's file references, I was able to resolve the build failures without modifying application data or valid configurations.

This exercise closely mirrors real-world DevOps responsibilities, where the goal is often to identify and fix deployment issues while preserving the integrity of existing systems.
