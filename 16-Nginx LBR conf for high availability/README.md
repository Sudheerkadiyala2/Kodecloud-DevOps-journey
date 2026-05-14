# Day 16 — Nginx Load Balancer Configuration for High Availability ⚖️

100 days of DevOps | Challenge #16

## What's This About?

As traffic grows, a single application server eventually becomes a bottleneck. One server handling all requests means:

- slower response times
- higher risk of downtime
- no fault tolerance

In this challenge, I configured Nginx as a Load Balancer (LBR) to distribute traffic across multiple Apache backend servers in a high-availability setup.

This is one of the most common real-world infrastructure patterns used in production systems.

---

# The Problem

The Nautilus production support team noticed increasing traffic causing website performance degradation.

To solve this, the application was migrated to a High Availability architecture using:

- 1 Load Balancer server (`stlb01`)
- 3 Apache application servers
- Nginx as reverse proxy/load balancer

The goal was to configure the LBR server so users could access the website through:

## What I Did

### Step 1 — Verified nginx installation

```bash
nginx -v
```

### Output

```bash
nginx version: nginx/1.20.1
```

Meaning nginx was already installed.

---

## Step 2 — Verified Apache backend servers

Checked Apache service and listening ports on all app servers.

```bash
sudo ss -tlnp | grep httpd
```

### Output showed Apache running on:

```text
*:5000
```

on:

- stapp01
- stapp02
- stapp03

This was important because the task specifically said:

> Do not modify existing Apache ports.

---

## Step 3 — Configured nginx load balancing

Updated only:

```bash
/etc/nginx/nginx.conf
```

Added backend upstream pool:

```nginx
upstream app_servers {
    server stapp01:5000;
    server stapp02:5000;
    server stapp03:5000;
}
```

Configured reverse proxy server block:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name _;

    location / {
        proxy_pass http://app_servers;
    }
}
```

---

## Step 4 — Tested configuration

Validated nginx syntax:

```bash
sudo nginx -t
```

Restarted nginx:

```bash
sudo systemctl restart nginx
```

---

## Step 5 — Final validation

```bash
curl http://stlb01:80
```

### Output

```text
Welcome to xFusionCorp Industries!
```

Successfully confirmed:

```text
Client → Nginx Load Balancer → Apache Backend Servers
```

---

# Breakdown

| Configuration | Purpose |
|---|---|
| upstream app_servers | Defines backend server pool |
| proxy_pass | Forwards client requests to backend servers |
| listen 80 | Accepts HTTP traffic |
| nginx -t | Validates syntax before restart |
| systemctl restart nginx | Applies configuration changes |

---

# What I Learned

- Difference between nginx as:
  - Web Server
  - Reverse Proxy
  - Load Balancer

- How upstream backend pools work
- How reverse proxying distributes traffic
- Importance of preserving existing application ports
- Real-world High Availability architecture concepts
- Traffic flow between frontend and backend infrastructure
- Why validation (`nginx -t`) matters before production restart

---

# Previous vs Updated Architecture

## Before

```text
User → Nginx → Local Static Files
```

nginx served files from:

```nginx
root /usr/share/nginx/html;
```

---

## After

```text
User → Nginx Load Balancer → Multiple Apache Servers
```

nginx now forwards traffic using:

```nginx
proxy_pass http://app_servers;
```

This converted nginx from a simple web server into a production-style load balancer.

---

# Real-World Use Case

This exact architecture is used in:

- E-commerce platforms
- Banking applications
- SaaS platforms
- Kubernetes ingress controllers
- Cloud-native environments

Load balancers help achieve:

- High Availability
- Scalability
- Fault Tolerance
- Better Performance
- Traffic Distribution

---

# Key Takeaway

A load balancer is more than just traffic forwarding — it's the entry point of reliability in modern infrastructure.

Instead of one server handling everything, requests are intelligently distributed across multiple backend systems, reducing downtime risk and improving scalability.

This challenge helped me understand how real production environments handle growing traffic before moving into advanced orchestration platforms like Kubernetes.

---

# Environment

| Component | Details |
|---|---|
| OS | Linux |
| Load Balancer | nginx |
| Backend Servers | Apache HTTPD |
| Port Used | 5000 |
| Validation Tool | curl |
| Skill Area | Load Balancing, Reverse Proxy, High Availability, Linux Administration |

---

Part of my #100DaysOfDevOps journey 🚀

```bash
curl http://stlb01:80
