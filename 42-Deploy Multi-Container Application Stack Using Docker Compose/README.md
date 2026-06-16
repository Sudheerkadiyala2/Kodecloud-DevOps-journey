# Day 42: Deploy Multi-Container Application Stack Using Docker Compose

**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Compose

---

## Challenge

The Nautilus Application Development team completed an application that needed to be tested in a containerized environment before production deployment.

The DevOps team was asked to create a complete multi-container stack using Docker Compose on Application Server 2.

### Task Requirements

Create the following file:

```bash
/opt/data/docker-compose.yml
```

The compose file should deploy two services:

### Web Service

- Container name:

```bash
php_host
```

- Use image:

```bash
php:<apache-tag>
```

- Map:

```bash
Host Port: 8085
Container Port: 80
```

- Mount volume:

```bash
Host: /var/www/html
Container: /var/www/html
```

### Database Service

- Container name:

```bash
mysql_host
```

- Use MariaDB image.
- Expose database port 3306.
- Persist database data using a host volume.
- Create the required database and user credentials.

---

## Objective

Deploy a complete two-tier application stack consisting of:

1. PHP Apache Web Server
2. MariaDB Database Server

using a single Docker Compose configuration file.

---

## Why This Challenge Matters

Modern applications rarely run as a single container.

Most production applications require:

- Frontend services
- Backend APIs
- Databases
- Persistent storage
- Service orchestration

Docker Compose simplifies deployment by managing multiple containers through a single configuration file.

This challenge introduced the fundamentals of multi-container deployments.

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

### Navigate to Data Directory

```bash
cd /opt/data
```

### Create Docker Compose File

```bash
vi docker-compose.yml
```

---

## Docker Compose Configuration

```yaml
version: '3.8'

services:

  web:
    image: php:apache
    container_name: php_host
    ports:
      - "8085:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    image: mariadb:latest
    container_name: mysql_host
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_host
      MYSQL_USER: dbuser
      MYSQL_PASSWORD: dbpassword
      MYSQL_ROOT_PASSWORD: rootpassword
```

---

## Deploy the Stack

Start all services:

```bash
docker compose up -d
```

Expected Output:

```text
Creating php_host ...
Creating mysql_host ...
```

Both containers should start successfully.

---

## Verification

### Check Running Containers

```bash
docker ps
```

Expected:

```text
CONTAINER ID   IMAGE            NAMES
xxxxxxxxxxxx   php:apache       php_host
xxxxxxxxxxxx   mariadb:latest   mysql_host
```

### Verify Compose Services

```bash
docker compose ps
```

Expected:

```text
NAME         STATUS
php_host     running
mysql_host   running
```

### Verify Web Service

```bash
curl http://localhost:8085
```

Expected:

```html
<html>...
```

or Apache/PHP response.

### Verify Database Container

```bash
docker ps | grep mysql_host
```

Expected:

```text
Up
```

---

## What I Learned

This challenge introduced Docker Compose and multi-container orchestration.

Instead of managing containers individually:

```bash
docker run ...
docker run ...
docker run ...
```

Docker Compose allows an entire application stack to be managed from a single YAML file.

This significantly improves deployment consistency and maintainability.

---

## Key Concepts

### Docker Compose

Docker Compose is a tool used to define and run multi-container Docker applications.

Example:

```bash
docker compose up -d
```

Benefits:

- Single configuration file
- Easy deployment
- Service management
- Repeatable environments

---

### Services

Each application component is defined as a service.

Example:

```yaml
services:
  web:
  db:
```

In this challenge:

- web → PHP Apache
- db → MariaDB

---

### Port Mapping

Port mapping exposes container ports to the host.

Example:

```yaml
ports:
  - "8085:80"
```

Meaning:

```text
Host Port      Container Port
8085     --->      80
```

Users access the application via:

```text
http://server-ip:8085
```

---

### Volume Mapping

Volumes persist application and database data.

Web Volume:

```yaml
/var/www/html:/var/www/html
```

Database Volume:

```yaml
/var/lib/mysql:/var/lib/mysql
```

Benefits:

- Data persistence
- Easier backups
- Data survives container recreation

---

### Environment Variables

Used to initialize MariaDB during container creation.

Example:

```yaml
environment:
  MYSQL_DATABASE: database_host
```

Used for:

- Database creation
- User creation
- Authentication setup

---

## Visual Workflow

```text
Docker Compose
        |
        v

+-------------------+
|   docker-compose  |
+-------------------+
        |
        |
  -------------------
  |                 |
  v                 v

PHP Apache      MariaDB
Container       Container

php_host       mysql_host
  |                 |
  |                 |
  -------------------
        |
        v

Application Stack
```

---

## Useful Commands

Start stack:

```bash
docker compose up -d
```

Stop stack:

```bash
docker compose down
```

View running containers:

```bash
docker ps
```

View compose services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs
```

Restart stack:

```bash
docker compose restart
```

Remove stack:

```bash
docker compose down -v
```

---

## Real-World Relevance

Docker Compose is widely used for:

- Local development environments
- Testing environments
- Application prototyping
- CI/CD validation
- Multi-service application deployments

Typical Compose deployments include:

- PHP + MySQL
- Node.js + MongoDB
- Python + PostgreSQL
- Nginx + Backend APIs

Many developers use Compose before migrating workloads to Kubernetes.

---

## Key Takeaways

- Docker Compose manages multiple containers from a single file.
- Services simplify application architecture.
- Volume mapping enables data persistence.
- Port mapping exposes applications externally.
- Environment variables automate database initialization.
- Compose reduces deployment complexity.

---

## Final Thoughts

This challenge demonstrated how Docker Compose can orchestrate an entire application stack using a single YAML configuration.

By deploying both a PHP Apache web server and a MariaDB database together, I gained hands-on experience with service definitions, port mappings, persistent storage, and container orchestration.

Understanding Docker Compose is an important milestone because it bridges the gap between single-container deployments and larger orchestration platforms such as Kubernetes.
