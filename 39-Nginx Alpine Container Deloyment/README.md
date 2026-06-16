# Day 39: Deploy an Nginx Alpine Container
**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Containers

## Challenge

The Nautilus DevOps team needed to deploy an Nginx container on **Application Server 2** for testing purposes.

The task requirements were:

- Pull the **nginx:alpine** Docker image.
- Create a container named **official** using the pulled image.
- Map host port **8087** to container port **80**.
- Ensure the container remains in a running state.

## Solution

### Step 1: SSH into Application Server 2

```bash
ssh steve@stapp02
```

### Step 2: Pull the Required Docker Image

```bash
docker pull nginx:alpine
```

### Step 3: Create and Start the Container

```bash
docker run -d \
  --name official \
  -p 8087:80 \
  nginx:alpine
```

### Step 4: Verify the Container Status

```bash
docker ps
```

### Step 5: Test the Application

```bash
curl http://localhost:8087
```

## Commands Used

```bash
ssh steve@stapp02

docker pull nginx:alpine

docker run -d \
  --name official \
  -p 8087:80 \
  nginx:alpine

docker ps

curl http://localhost:8087
```

## Verification

Verify that the container is running:

```bash
docker ps
```

Expected output should show the container:

```text
official
```

Verify that Nginx is accessible through the mapped port:

```bash
curl http://localhost:8087
```

Expected result:

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```

## Outcome

Successfully pulled the **nginx:alpine** image, created the **official** container, mapped host port **8087** to container port **80**, and verified that the Nginx web server was accessible and running successfully.
