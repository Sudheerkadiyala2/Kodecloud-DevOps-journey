# 🛡️ Firewall Configuration Using iptables (Port Restriction) 🔒

> **DevOps Project** | Linux Security | iptables | Multi-Server Environment | KodeKloud Lab

---

## 🚀 What this project taught me

I initially thought this task was just about blocking a port — but it turned out to be about **real-world infrastructure security**.

This project helped me understand:
- How to restrict access using firewall rules
- How Load Balancers act as trusted entry points
- Why rule order matters in iptables
- How to verify network behavior using `curl`
- How to make configurations persistent across reboots

---

## 🧭 Project Overview

| Detail | Value |
|--------|------|
| App Servers | stapp01, stapp02, stapp03 |
| Load Balancer | stlb01 |
| Port Restricted | 3003 |
| Firewall Tool | iptables |
| Access Policy | Allow only Load Balancer |
| Platform | KodeKloud — xFusionCorp Infrastructure |

---

## 🏗️ Architecture

```
User → Load Balancer (stlb01) → App Servers (stapp01 / stapp02 / stapp03)
```

- Only **Load Balancer → App Servers (port 3003)** is allowed  
- All other traffic is blocked ❌  

---

## ⚙️ Implementation Steps

### 1️⃣ Install iptables

Run on all app servers:

```bash
sudo yum install -y iptables iptables-services
```

---

### 2️⃣ Configure Firewall Rules

Load Balancer IP:

```bash
10.244.244.179
```

Apply rules:

```bash
sudo iptables -F

sudo iptables -A INPUT -p tcp -s 10.244.244.179 --dport 3003 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 3003 -j DROP
```

---

### 3️⃣ Make Rules Persistent

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
sudo systemctl enable iptables
sudo systemctl restart iptables
```

---

## 🧪 Testing & Verification

### ✅ From Load Balancer (Allowed)

```bash
curl http://10.244.234.249:3003
```

✔ Expected: Connection successful

---

### ❌ From Jump Host / Other Servers (Blocked)

```bash
curl -v http://10.244.234.249:3003
```

✔ Expected:
- Connection timeout
- No response

---

## 🔍 Verify Rules

```bash
sudo iptables -L INPUT -n --line-numbers
```

Expected output:

```
1 ACCEPT tcp -- 10.244.244.179  0.0.0.0/0 tcp dpt:3003
2 DROP   tcp -- 0.0.0.0/0       0.0.0.0/0 tcp dpt:3003
```

---

## ⚠️ Common Mistakes Avoided

- Using incorrect Load Balancer IP  
- Placing DROP rule before ACCEPT  
- Not saving rules (`iptables-save`)  
- Not enabling service (`systemctl enable iptables`)  
- Testing from only one system  

---

## 🧠 Key Learnings

- Firewall rules are **order-sensitive**
- Always **allow trusted sources first**
- Persistence is critical for real systems
- `curl` is essential for network testing
- Security is implemented in **layers**

---

## 📁 Environment

- Platform: KodeKloud (xFusionCorp)
- Servers: Multi-server architecture
- Tools Used: `iptables`, `curl`, `ssh`, `systemctl`
- Focus Area: Linux Security, Networking, DevOps Fundamentals

---

## 🎯 Final Outcome

- Port **3003 secured successfully**  
- Access allowed only from Load Balancer ✅  
- All other access blocked ❌  
- Rules persist after reboot 🔁  

---

## 💬 Final Thought

This task may look simple, but it's a core part of **real production security**.

Understanding this builds a strong foundation in:
> DevOps + System Security + Networking
