# Day 43: Dockerize and Deploy a Python Application

**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker | Python Application Deployment

---

## Challenge

The Nautilus Application Development team required a Python application to be containerized and deployed on Application Server 1.

A `requirements.txt` file containing the application's dependencies was already available under:

```bash
/python_app/src/
```

The task was to create a Docker image, deploy the application as a container, and expose it for external access.

### Task Requirements

#### Create a Dockerfile

Location:

```bash
/python_app/Dockerfile
```

Requirements:

- Use any Python image as the base image.
- Install dependencies using:

```bash
requirements.txt
```

- Expose port:

```bash
8082
```

- Run:

```bash
server.py
```

using CMD.

---

#### Build Docker Image

Image name:

```bash
nautilus/python-app
```

---

#### Deploy Container

Container name:

```bash
pythonapp_nautilus
```

Port mapping:

```text
Host Port      Container Port
8098      -->      8082
```

---

#### Validate Application

```bash
curl http://localhost:8098/
```

---

## Objective

Containerize a Python application and deploy it using Docker while ensuring:

- Dependency management through requirements.txt.
- Consistent runtime environment.
- External accessibility through port mapping.
- Reproducible deployments.

---

## Why This Challenge Matters

One of the biggest advantages of Docker is the ability to package applications together with all their dependencies.

Without containers:

- Different Python versions can cause issues.
- Dependency conflicts may occur.
- Deployments become inconsistent.

Docker solves these problems by creating a portable and isolated environment.

This challenge demonstrated a complete application containerization workflow.

---

## Commands Used

### Connect to Application Server 1

```bash
ssh tony@app01
```

### Switch to Root User

```bash
sudo su -
```

### Navigate to Application Directory

```bash
cd /python_app
```

### Create Dockerfile

```bash
vi Dockerfile
```

---

## Dockerfile Configuration

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY src/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY src/ .

EXPOSE 8082

CMD ["python", "server.py"]
```

---

## Build Docker Image

```bash
docker build -t nautilus/python-app .
```

Expected Output:

```text
Successfully built xxxxxxxxxxxx
Successfully tagged nautilus/python-app:latest
```

---

## Deploy Container

```bash
docker run -d \
--name pythonapp_nautilus \
-p 8098:8082 \
nautilus/python-app
```

Explanation:

```text
-d                 Run container in background
--name             Assign container name
-p 8098:8082       Port mapping
```

---

## Verification

### Verify Image

```bash
docker images
```

Expected:

```text
REPOSITORY             TAG       IMAGE ID
nautilus/python-app    latest    xxxxxxxxxxxx
```

---

### Verify Container

```bash
docker ps
```

Expected:

```text
CONTAINER ID   IMAGE                 NAMES
xxxxxxxxxxxx   nautilus/python-app   pythonapp_nautilus
```

---

### Test Application

```bash
curl http://localhost:8098/
```

Expected:

```text
Application response
```

The exact output depends on the implementation inside `server.py`.

---

## What I Learned

This challenge demonstrated the complete lifecycle of application containerization:

1. Create Dockerfile
2. Install dependencies
3. Build image
4. Run container
5. Verify deployment

I also learned how Docker simplifies Python application deployment by packaging both code and dependencies into a single image.

---

## Key Concepts

### Dockerfile

A Dockerfile defines how a Docker image is built.

Example:

```dockerfile
FROM python:3.9-slim
```

Common instructions:

```dockerfile
FROM
WORKDIR
COPY
RUN
EXPOSE
CMD
```

---

### Python Base Image

Used:

```dockerfile
python:3.9-slim
```

Benefits:

- Lightweight
- Official image
- Includes Python runtime
- Faster image builds

---

### Requirements File

Dependency installation:

```dockerfile
RUN pip install -r requirements.txt
```

Benefits:

- Automated package installation
- Version consistency
- Easier maintenance

Example:

```text
flask
requests
numpy
```

---

### EXPOSE

Defines the application's listening port.

```dockerfile
EXPOSE 8082
```

This documents the container's network requirements.

---

### CMD

Defines the startup command.

```dockerfile
CMD ["python", "server.py"]
```

Executed automatically when the container starts.

---

## Visual Workflow

```text
Application Source Code
          |
          v

requirements.txt
          |
          v

Dockerfile
          |
          v

Docker Build
          |
          v

nautilus/python-app
          |
          v

docker run
          |
          v

pythonapp_nautilus
          |
          v

Host Port 8098
          |
          v

Container Port 8082
          |
          v

Python Application
```

---

## Useful Commands

Build image:

```bash
docker build -t nautilus/python-app .
```

Run container:

```bash
docker run -d --name pythonapp_nautilus -p 8098:8082 nautilus/python-app
```

View running containers:

```bash
docker ps
```

View all containers:

```bash
docker ps -a
```

View logs:

```bash
docker logs pythonapp_nautilus
```

Stop container:

```bash
docker stop pythonapp_nautilus
```

Start container:

```bash
docker start pythonapp_nautilus
```

Remove container:

```bash
docker rm -f pythonapp_nautilus
```

Remove image:

```bash
docker rmi nautilus/python-app
```

---

## Real-World Relevance

Dockerized Python applications are commonly used for:

- REST APIs
- Flask applications
- Django applications
- Machine Learning services
- Data processing pipelines
- Microservices

Containerization ensures that applications behave consistently across:

- Development
- Testing
- Staging
- Production

environments.

---

## Key Takeaways

- Dockerfiles automate application packaging.
- Python dependencies can be managed through requirements.txt.
- Images provide portable application environments.
- Containers isolate applications from the host system.
- Port mapping exposes applications externally.
- Docker simplifies deployment and scaling.

---

## Final Thoughts

This challenge provided hands-on experience in Dockerizing a Python application from scratch.

By creating a Dockerfile, installing dependencies, building a custom image, and deploying a running container, I learned how modern applications are packaged and distributed using containers.

This workflow is one of the most common patterns in DevOps and cloud-native environments, forming the foundation for CI/CD pipelines, Kubernetes deployments, and microservices architectures.
