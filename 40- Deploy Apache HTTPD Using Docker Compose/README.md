# Day 41: Deploy Apache HTTPD Using Docker Compose

**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Compose

## Challenge

The Nautilus application development team shared static website content that needed to be hosted on an Apache HTTPD web server using a containerized platform.

### Requirements

- Create a Docker Compose file at `/opt/docker/docker-compose.yml` on **App Server 3**.
- Use the latest `httpd` image.
- Ensure the container name is `httpd`.
- Map host port `8086` to container port `80`.
- Mount the host directory `/opt/data` to the container directory `/usr/local/apache2/htdocs`.
- Do not modify any existing data within the mounted directories.

## Solution

### Step 1: SSH into App Server 3

```bash
ssh banner@stapp03
```

### Step 2: Create the Docker Compose Directory

```bash
mkdir -p /opt/docker
```

### Step 3: Create the Docker Compose File

```bash
nano /opt/docker/docker-compose.yml
```

Add the following configuration:

```yaml
services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8086:80"
    volumes:
      - /opt/data:/usr/local/apache2/htdocs
```

### Step 4: Deploy the Container

```bash
cd /opt/docker
docker compose up -d
```

### Step 5: Verify the Deployment

```bash
docker ps
```

## Docker Compose Configuration

```yaml
services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8086:80"
    volumes:
      - /opt/data:/usr/local/apache2/htdocs
```

## Commands Used

```bash
ssh banner@stapp03

mkdir -p /opt/docker

nano /opt/docker/docker-compose.yml

cd /opt/docker
docker compose up -d

docker ps

curl http://localhost:8086
```

## Verification

Verify that the container is running:

```bash
docker ps
```

Expected output should include:

```text
httpd
```

Verify the port mapping:

```bash
docker port httpd
```

Expected output:

```text
80/tcp -> 0.0.0.0:8086
```

Test the hosted website content:

```bash
curl http://localhost:8086
```

The command should return the contents served from the `/opt/data` directory.

## Troubleshooting

While creating the Docker Compose file:

- Fixed a YAML formatting issue related to list syntax.
- Ensured proper indentation for the `ports` and `volumes` sections.
- Saved and exited the Nano editor successfully.

## Outcome

Successfully deployed an Apache HTTPD container using Docker Compose on App Server 3. The container was named `httpd`, mapped host port `8086` to container port `80`, and served static website content from the existing `/opt/data` directory through a volume mount.
