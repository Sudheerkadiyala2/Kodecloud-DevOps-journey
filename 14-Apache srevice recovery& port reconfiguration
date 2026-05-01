# 🔧 Apache Service Recovery & Port Configuration (6200) 🌐

> **DevOps Project** | Apache HTTP Server | Linux Troubleshooting | Multi-Server Environment | KodeKloud Lab

---

## Let me be upfront about what this project actually taught me.

I went in thinking “just start Apache and change a port” — and came out understanding how real production troubleshooting works: service failures, port conflicts, system-level debugging, and why checking the right logs and ports matters more than guessing.

This project is a small but realistic simulation of what happens in production:  
A monitoring system detects a failure, and it’s up to you to **identify the issue, fix it, and ensure consistency across all servers**.

---

## 🧭 Project Overview

| Detail | Value |
|--------|------|
| **App Servers** | stapp01, stapp02, stapp03 |
| **Service** | Apache HTTP Server (`httpd`) |
| **Issue** | Apache unavailable on one server |
| **Required Port** | 6200 |
| **Platform** | KodeKloud — xFusionCorp Infrastructure |

---

## 🏗️ Architecture / Setup

Understanding the flow is key:

```
Monitoring System → Detects Apache Failure → App Servers
                                      │
                                      ▼
                          Troubleshoot → Fix → Verify
```

- One server had Apache down  
- All servers needed Apache running  
- Apache had to run on a **custom port (6200)**  

---

## 🔍 Problem Identification

First step was checking Apache status on all app servers:

```bash
sudo systemctl status httpd
```

- One server showed: ❌ **failed**
- Others were running but required port validation

---

## ⚠️ Root Cause Analysis

Apache configuration was correct:

```bash
sudo httpd -t
# Output: Syntax OK
```

But service still failed — which pointed to a runtime issue.

Checking port usage:

```bash
sudo ss -tlnp | grep 6200
```

Revealed:

```
sendmail was already using port 6200
```

👉 Apache failed because it could not bind to the required port.

---

## ⚙️ Implementation Steps

### Step 1 — Install Apache (if needed)

```bash
sudo yum install -y httpd
```

---

### Step 2 — Configure Apache to use Port 6200

Edit configuration:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Find:

```
Listen 80
```

Change to:

```
Listen 6200
```

---

### Step 3 — Resolve Port Conflict

Free the port:

```bash
sudo fuser -k 6200/tcp
```

---

### Step 4 — Start and Enable Apache

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

---

### Step 5 — Verify Service

Check Apache status:

```bash
sudo systemctl status httpd
```

Check port binding:

```bash
sudo ss -tlnp | grep 6200
```

---

## 🧪 Testing & Verification

### Check Apache is running

```bash
sudo systemctl status httpd
```

Expected:

```
active (running)
```

---

### Verify Apache is listening on port 6200

```bash
sudo ss -tlnp | grep 6200
```

Expected:

```
httpd listening on :6200
```

---

### Test locally

```bash
curl http://localhost:6200
```

✔ Any response confirms Apache is working

---

## 🔁 Applied Across All Servers

Same steps repeated on:

- `stapp01`
- `stapp02`
- `stapp03`

Ensuring consistency across the environment.

---

## 🔴 Troubleshooting Insights

### Apache fails even when config is correct

- `httpd -t` checks syntax only  
- Does NOT detect runtime issues like port conflicts  

---

### Port already in use

```bash
ss -tlnp | grep <port>
```

- Shows which service is blocking the port  

---

### Freeing a port quickly

```bash
fuser -k <port>/tcp
```

- Kills the process using that port  

---

## 🧠 Key Learnings

1. Monitoring tools only tell you *something is wrong* — not *what*  
2. Service failures are often caused by **resource conflicts (like ports)**  
3. `ss` is one of the most powerful debugging tools in Linux  
4. Always verify both:
   - Service status  
   - Port binding  
5. Consistency across servers is critical in production environments  

---

## 📁 Environment

- Platform: KodeKloud — xFusionCorp Infrastructure  
- Servers: Multi-server setup  
- Tools Used: `systemctl`, `ss`, `fuser`, `curl`, `vi`  
- Skill Area: Linux Troubleshooting, Service Management, Networking  

---

## 🎯 Final Outcome

- Apache running on all app servers ✅  
- Apache configured on port 6200 ✅  
- Port conflict resolved successfully ✅  
- Service enabled and stable across environment ✅  

---

## 💬 Final Thought

This task may look simple — but it reflects a real production workflow:

> Monitoring detects → Engineer investigates → Root cause fixed → Service restored

Mastering this flow is what turns commands into real DevOps skills 🚀
