# Day 37: Create Custom Apache Docker Image Using Ubuntu 24.04

---

## Challenge

The Nautilus application development team requested a custom Docker image for one of their projects. The DevOps team was asked to create a Dockerfile that builds an image based on Ubuntu 24.04 and runs Apache on a non-default port.

### Task Requirements

Create a Dockerfile at:

```bash
/opt/docker/Dockerfile
```

Requirements:

- Use the base image:

```bash
ubuntu:24.04
```

- Install:

```bash
apache2
```

- Configure Apache to listen on:

```bash
6200
```

- Do not modify any other Apache configuration settings such as:
  - Document root
  - Virtual host settings
  - Directory configurations

---

## Objective

Create a custom Docker image using a Dockerfile that:

- Uses Ubuntu 24.04 as the base image.
- Installs Apache HTTP Server.
- Changes the default Apache port from 80 to 6200.
- Runs Apache in the foreground when the container starts.

---

## Why This Challenge Matters

While running pre-built containers is common, real-world DevOps engineers frequently need to build custom images.

Dockerfiles allow teams to:

- Standardize environments
- Automate software installation
- Apply custom configurations
- Version infrastructure as code
- Create reusable deployment artifacts

This challenge introduced the process of creating a custom image rather than simply deploying an existing one.

---

## Commands Used

### Connect to Application Server 2

```bash
ssh steve@app02
```

### Switch to Root User

```bash
sudo su -
```

### Navigate to Docker Directory

```bash
cd /opt/docker
```

### Create Dockerfile

```bash
vi Dockerfile
```

### Dockerfile Content

```dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y apache2 && \
    sed -i 's/80/6200/g' /etc/apache2/ports.conf && \
    sed -i 's/:80/:6200/g' /etc/apache2/sites-available/000-default.conf

EXPOSE 6200

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

### Save and Exit

```bash
:wq
```

### Build Docker Image

```bash
docker build -t apache-ubuntu:latest .
```

### Verify Image Creation

```bash
docker images
```

Expected Output:

```bash
REPOSITORY      TAG       IMAGE ID
apache-ubuntu   latest    xxxxxxxxxxxx
```

---

## Verification

Verify that the Dockerfile exists:

```bash
ls -l /opt/docker/Dockerfile
```

Check Dockerfile contents:

```bash
cat /opt/docker/Dockerfile
```

Build the image successfully:

```bash
docker build -t apache-ubuntu:latest .
```

Ensure no build errors occur.

---

## What I Learned

This challenge introduced Docker image creation using a Dockerfile.

Unlike running existing images, Dockerfiles allow us to define:

- Base operating system
- Installed packages
- Configuration changes
- Startup commands

Everything becomes reproducible and version-controlled.

---

## Key Concepts

### Dockerfile

A text file containing instructions used to build a Docker image.

Example:

```dockerfile
FROM ubuntu:24.04
```

Common Instructions:

```dockerfile
FROM
RUN
ENV
EXPOSE
CMD
```

---

### Base Image

The starting point for a custom image.

Used in this challenge:

```dockerfile
FROM ubuntu:24.04
```

Benefits:

- Consistent environment
- Predictable behavior
- Easy customization

---

### Apache HTTP Server

Apache is one of the most widely used web servers.

Installed using:

```bash
apt-get install -y apache2
```

Default Port:

```bash
80
```

Modified Port:

```bash
6200
```

---

### EXPOSE Instruction

Documents which port the containerized application uses.

Example:

```dockerfile
EXPOSE 6200
```

This helps when mapping ports during container deployment.

---

### CMD Instruction

Defines the default command executed when a container starts.

Example:

```dockerfile
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

Purpose:

- Starts Apache
- Keeps the container running
- Runs in foreground mode

---

## Visual Workflow

```text
Ubuntu 24.04 Base Image
           |
           v

Install Apache2
           |
           v

Modify Apache Port
(80 → 6200)
           |
           v

Build Docker Image
           |
           v

Custom Apache Image
           |
           v

Run Container
           |
           v

Apache Listening on Port 6200
```

---

## Useful Commands

Create Dockerfile:

```bash
vi Dockerfile
```

Build image:

```bash
docker build -t apache-ubuntu:latest .
```

View images:

```bash
docker images
```

Run container:

```bash
docker run -d -p 6200:6200 apache-ubuntu:latest
```

View running containers:

```bash
docker ps
```

View logs:

```bash
docker logs <container-id>
```

Remove image:

```bash
docker rmi apache-ubuntu:latest
```

---

## Real-World Relevance

Custom Docker images are widely used for:

- Application packaging
- Web server deployments
- CI/CD pipelines
- Infrastructure automation
- Kubernetes workloads
- Standardized enterprise environments

Most production deployments rely on custom-built images rather than generic public images.

---

## Key Takeaways

- Dockerfiles automate image creation.
- Ubuntu 24.04 can be used as a custom base image.
- Apache can be configured during image build time.
- Port configuration can be automated using shell commands.
- Custom images improve consistency and repeatability.
- Infrastructure can be defined as code.

---

## Final Thoughts

This challenge provided hands-on experience with creating a custom Docker image using a Dockerfile.

Instead of simply launching a pre-built image, I learned how to build an image from scratch, install required software, modify configuration files, and prepare the image for deployment.

Understanding Dockerfile creation is a critical skill for DevOps engineers because it forms the foundation for containerized application delivery, CI/CD automation, and Kubernetes-based deployments.
