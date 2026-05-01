# 🔐 Nginx HTTPS Setup with Self-Signed SSL (App Server 3) 🌐

> **DevOps Project** | Nginx | SSL Configuration | Linux Server Setup | KodeKloud Lab

---

## Let me be upfront about what this project actually taught me.

I went in thinking “install Nginx and add SSL” — and came out understanding how HTTPS actually works at the server level: certificates, private keys, secure communication, and why testing from another machine matters.

This project simulates a real-world scenario where a server must be prepared for **secure application deployment using HTTPS**.

---

## 🧭 Project Overview

| Detail | Value |
|--------|------|
| **Target Server** | App Server 3 (stapp03) |
| **Web Server** | Nginx |
| **Protocol** | HTTPS (SSL/TLS) |
| **Certificate Type** | Self-signed |
| **Platform** | KodeKloud — xFusionCorp Infrastructure |

---

## 🏗️ Architecture / Setup

```
Jump Host (thor) → HTTPS Request → App Server 3 (stapp03)
```

- Nginx serves traffic over **HTTPS (port 443)**  
- SSL certificate ensures encrypted communication  
- Access is tested remotely using `curl`  

---

## 📦 Prerequisites

Files already available on server:

```
/tmp/nautilus.crt
/tmp/nautilus.key
```

These represent:
- `.crt` → SSL Certificate  
- `.key` → Private Key  

---

## ⚙️ Implementation Steps

### Step 1 — Install Nginx

```bash
sudo yum install -y nginx
```

---

### Step 2 — Move SSL Certificate & Key

```bash
sudo mkdir -p /etc/nginx/ssl

sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

---

### Step 3 — Configure Nginx for HTTPS

Updated `/etc/nginx/nginx.conf`:

```nginx
server {
    listen       443 ssl;
    listen       [::]:443 ssl;
    server_name  stapp03;

    root   /usr/share/nginx/html;
    index  index.html;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

### Step 4 — Create Web Page

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

---

### Step 5 — Start & Enable Nginx

```bash
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl enable nginx
```

---

## 🧪 Testing & Verification

### Local Test (on server)

```bash
curl -k https://localhost
```

✔ Expected:
```
Welcome!
```

---

### Remote Test (from Jump Host)

```bash
curl -Ik https://stapp03
```

---

### 🔍 Understanding the Test Command

- `curl` → HTTP client  
- `-I` → fetch headers only  
- `-k` → ignore SSL validation (self-signed cert)  

---

### ✅ Expected Output

```
HTTP/1.1 200 OK
```

---

## ⚠️ Common Issues Faced

- Missing SSL files in `/etc/nginx/ssl`  
- Syntax errors in `nginx.conf` (missing braces)  
- Port not listening due to service not started  
- SSL errors without using `-k`  

---

## 🧠 Key Learnings

1. HTTPS requires both certificate and private key  
2. Nginx configuration must be syntactically correct (`nginx -t`)  
3. Self-signed certificates require `-k` in curl  
4. Always test from another system (real-world validation)  
5. Secure communication is a core part of production systems  

---

## 📁 Environment

- Platform: KodeKloud — xFusionCorp Infrastructure  
- Server: stapp03  
- Tools Used: `nginx`, `curl`, `systemctl`, `vi`  
- Skill Area: Web Server Setup, SSL Configuration, Linux Admin  

---

## 🎯 Final Outcome

- Nginx successfully installed and configured ✅  
- SSL certificate deployed correctly ✅  
- HTTPS working on port 443 ✅  
- Remote access verified from jump host ✅  

---

## 💬 Final Thought

This task shows how real systems are prepared before deployment:

> Install → Secure → Configure → Verify

Understanding this workflow is essential for DevOps and Cloud roles 🚀
