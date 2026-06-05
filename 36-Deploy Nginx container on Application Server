# Day 36: Deploy Nginx Container on Application Server

**Date:** 2026-06-04  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Containers

---

## Challenge

The Nautilus DevOps team was conducting application deployment tests on selected application servers.

The requirement was to deploy an Nginx container on **Application Server 3** using the Alpine version of the Nginx image.

Task requirements:

- Create a container named:

```text
nginx_3
```

- Use image:

```text
nginx:alpine
```

- Ensure the container remains in a running state.

---

## Objective

Deploy a lightweight Nginx web server container using Docker and verify that it is running successfully.

---

## Why This Challenge Matters

Containers are the foundation of modern application deployment.

Instead of installing software directly on servers, applications are packaged into containers that provide:

- Portability
- Consistency
- Faster deployments
- Isolation

This challenge introduced the process of deploying a container from a Docker image.

---

## Commands Used

### Connect to Application Server 3

```bash
ssh banner@app03
```

---

### Switch to Root User

```bash
sudo su -
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

### Pull Nginx Alpine Image

```bash
docker pull nginx:alpine
```

---

### Create and Run Container

```bash
docker run -d --name nginx_3 nginx:alpine
```

Explanation:

```text
-d          → Run in detached mode
--name      → Assign container name
nginx:alpine → Nginx image using Alpine Linux
```

---

### Verify Running Container

```bash
docker ps
```

Expected:

```text
CONTAINER ID   IMAGE           NAMES
xxxxxxxxxxxx   nginx:alpine    nginx_3
```

---

## Verification

List running containers:

```bash
docker ps
```

Check specific container:

```bash
docker inspect nginx_3
```

Check container status:

```bash
docker ps -a
```

Expected:

```text
STATUS: Up
```

---

## What I Learned

This challenge introduced the most common Docker workflow:

```text
Docker Image
      |
      v
Docker Container
```

I learned that containers are created from images and can be started with a single command.

---

## Key Concepts

### Docker Image

A read-only template used to create containers.

Example:

```text
nginx:alpine
```

Components:

```text
nginx  → Repository
alpine → Tag
```

---

### Alpine Image

Alpine Linux is a lightweight Linux distribution.

Benefits:

- Small size
- Faster downloads
- Reduced attack surface
- Efficient resource usage

---

### Docker Container

A running instance of a Docker image.

Example:

```text
Image:
nginx:alpine

Container:
nginx_3
```

---

## Visual Workflow

```text
Docker Hub
     |
     v

nginx:alpine Image
     |
     v

docker run
     |
     v

nginx_3 Container
     |
     v

Running Nginx Service
```

---

## Useful Commands

Pull image:

```bash
docker pull nginx:alpine
```

Run container:

```bash
docker run -d --name nginx_3 nginx:alpine
```

View running containers:

```bash
docker ps
```

View all containers:

```bash
docker ps -a
```

Stop container:

```bash
docker stop nginx_3
```

Start container:

```bash
docker start nginx_3
```

Remove container:

```bash
docker rm -f nginx_3
```

---

## Real-World Relevance

Containerized Nginx deployments are commonly used for:

- Web servers
- Reverse proxies
- Load balancers
- Kubernetes ingress controllers
- Static website hosting

This simple deployment is very similar to how web services are launched in production environments.

---

## Key Takeaways

- Containers are created from images.
- `docker run` creates and starts containers.
- Alpine images are lightweight and efficient.
- Container names simplify management.
- Verification is important after deployment.

---

## Final Thoughts

This challenge was my first container deployment task using Docker.

It demonstrated how quickly applications can be deployed using containers and reinforced the core relationship between Docker images and running containers.

Understanding container deployment is the foundation for future Docker concepts such as networking, volumes, image creation, and orchestration.
