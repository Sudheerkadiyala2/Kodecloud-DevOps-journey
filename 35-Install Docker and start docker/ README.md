# Day 35: Install Docker Packages and Start Docker Service

## Challenge

The Nautilus DevOps team planned to begin containerizing applications after discussions with the application development team.

As part of the initial Docker setup, the following tasks were required on **App Server 3**:

1. Install Docker Engine (`docker-ce`)
2. Install Docker Compose
3. Start the Docker service

---

## Objective

Prepare App Server 3 for containerized workloads by installing and starting Docker.

---

## Why This Challenge Matters

Docker is one of the most widely used containerization platforms in modern DevOps environments.

Before deploying containers, the Docker runtime must be installed and running on the target server.

This challenge covered the foundational setup required before working with:

- Containers
- Images
- Docker Networks
- Docker Volumes
- Docker Compose
- Kubernetes

---

## Commands Used

### SSH into App Server 3

```bash
ssh banner@app03
```

or connect using the method provided in the lab.

---

### Switch to Root User

```bash
sudo su -
```

---

### Install Docker Engine

```bash
yum install -y docker-ce
```

Verify installation:

```bash
docker --version
```

Example output:

```text
Docker version 28.x.x
```

---

### Install Docker Compose

```bash
yum install -y docker-compose-plugin
```

or depending on repository availability:

```bash
yum install -y docker-compose
```

Verify installation:

```bash
docker compose version
```

or

```bash
docker-compose --version
```

---

### Start Docker Service

```bash
systemctl start docker
```

---

### Verify Docker Service

```bash
systemctl status docker
```

Expected:

```text
active (running)
```

---

### Optional: Enable Docker on Boot

```bash
systemctl enable docker
```

This ensures Docker starts automatically after a reboot.

---

## Verification

Check Docker service:

```bash
systemctl is-active docker
```

Expected:

```text
active
```

Check Docker version:

```bash
docker --version
```

Check Compose version:

```bash
docker compose version
```

or

```bash
docker-compose --version
```

---

## What I Learned

Before this challenge, Docker was just a tool I used to run containers.

This task helped me understand the initial setup process required before any container operations can be performed.

I learned that Docker consists of:

- Docker Engine
- Docker CLI
- Docker Service (daemon)
- Docker Compose

and all of them must be properly installed and configured.

---

## Key Concepts

### Docker Engine

The core runtime responsible for:

- Pulling images
- Creating containers
- Managing container lifecycle

Command:

```bash
docker
```

---

### Docker Service (Daemon)

Background service responsible for executing Docker commands.

Service name:

```text
docker
```

Management:

```bash
systemctl start docker
systemctl stop docker
systemctl restart docker
```

---

### Docker Compose

A tool used to define and run multi-container applications.

Example:

```yaml
services:
  web:
    image: nginx
  db:
    image: mysql
```

Instead of running multiple `docker run` commands manually.

---

## Visual Workflow

```text
Install Docker Engine
          |
          v

Install Docker Compose
          |
          v

Start Docker Service
          |
          v

Verify Installation
          |
          v

Ready for Containers
```

---

## Useful Commands

Install Docker:

```bash
yum install -y docker-ce
```

Install Compose:

```bash
yum install -y docker-compose-plugin
```

Start Docker:

```bash
systemctl start docker
```

Check status:

```bash
systemctl status docker
```

Enable on boot:

```bash
systemctl enable docker
```

Check version:

```bash
docker --version
```

---

## Real-World Relevance

In production environments, Docker is commonly used for:

- Application deployment
- CI/CD pipelines
- Microservices
- Development environments
- Container orchestration with Kubernetes

Every Docker host must first undergo the same setup process completed in this challenge.

---

## Key Takeaways

- Docker Engine must be installed before containers can run.
- Docker Compose helps manage multi-container applications.
- The Docker daemon must be running for Docker commands to work.
- Service verification is an important part of installation.
- Containerization begins with proper Docker host preparation.

---

## Final Thoughts

This challenge marked the beginning of my Docker journey within the KodeKloud DevOps Challenge.

Although simple, it introduced the fundamental setup required before working with containers, images, and orchestration tools.

Understanding Docker installation and service management is the foundation for all future container-related tasks.
