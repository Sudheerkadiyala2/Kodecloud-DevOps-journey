# Apache Service Unreachable on Port 3003 — Troubleshooting & Fix 🔧

> **DevOps Incident Response Project** | Apache | iptables | Port Conflict Debugging | KodeKloud Lab

---

## What This Is About

The monitoring tool fired an alert: **Apache on App Server 1 (`stapp01`) is not reachable on port `3003`** from the jump host. Three words: find it, fix it.

What made this one interesting is that it wasn't a single problem — it was **three issues stacked on top of each other.** The service was down, a rogue process had hijacked the port, and iptables was silently rejecting all external traffic. Each one had to be peeled back like a layer before the next one appeared.

This is what real incident response looks like.

---

## Environment

| Role | Hostname | User | Password |
|------|----------|------|----------|
| **Jump Host** | `jump-host` | `thor` | `mjolnir123` |
| **App Server 1** | `stapp01` | `tony` | `Ir0nM@n` |

---

## Problem Statement

Apache is configured to run on **port 3003** on `stapp01`. The monitoring tool reports it is **not reachable** from the jump host.

```bash
# What the jump host sees
curl http://stapp01:3003
# curl: (7) Failed to connect to stapp01 port 3003: No route to host
```

---

## Root Cause Summary

This challenge had **3 separate issues** — all of which needed fixing:

| # | Layer | Issue | Fix |
|---|-------|-------|-----|
| 1 | **Process** | `sendmail` was occupying port 3003 | `systemctl stop sendmail` |
| 2 | **Service** | Apache failed to start due to port conflict | `systemctl start httpd` |
| 3 | **Firewall** | iptables was rejecting all traffic except port 22 | `iptables -I INPUT 5 -p tcp --dport 3003 -j ACCEPT` |

---

## Troubleshooting — Step by Step

### Step 1 — Connect to App Server 1

```bash
# From the Jump Host
ssh tony@stapp01
# password: Ir0nM@n
```

---

### Step 2 — Check Apache Status

```bash
sudo systemctl status httpd
```

**Output showed:**
```
Active: failed (Result: exit-code)
httpd[57213]: (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:3003
httpd[57213]: no listening sockets available, shutting down
```

Apache tried to start, saw port 3003 was already taken, and gave up. The key error is **(98) Address already in use.**

---

### Step 3 — Find What's Occupying Port 3003

```bash
sudo ss -tlnp | grep 3003
```

**Output:**
```
LISTEN 0   10   127.0.0.1:3003   0.0.0.0:*   users:(("sendmail",pid=56538,fd=4))
```

**`sendmail` had grabbed port 3003** before Apache even started. Linux doesn't allow two processes to bind to the same port — first one wins, second one fails.

---

### Step 4 — Stop Sendmail and Start Apache

```bash
# Free up the port
sudo systemctl stop sendmail

# Now Apache can bind to 3003
sudo systemctl start httpd

# Verify Apache is running
sudo systemctl status httpd
# Active: active (running) ✅
```

---

### Step 5 — Test Locally

```bash
curl http://localhost:3003
# Returns HTML ✅ — Apache is working on the server itself
```

At this point the service is healthy locally. But the jump host still gets "No route to host" — which means the firewall is the next layer to investigate.

---

### Step 6 — Check iptables Rules

```bash
sudo /sbin/iptables -L -n --line-numbers
```

**Output:**
```
Chain INPUT (policy ACCEPT)
num  target   prot  source      destination
1    ACCEPT   all   0.0.0.0/0   0.0.0.0/0    state RELATED,ESTABLISHED
2    ACCEPT   icmp  0.0.0.0/0   0.0.0.0/0
3    ACCEPT   all   0.0.0.0/0   0.0.0.0/0
4    ACCEPT   tcp   0.0.0.0/0   0.0.0.0/0    state NEW tcp dpt:22
5    REJECT   all   0.0.0.0/0   0.0.0.0/0    reject-with icmp-host-prohibited
```

**Found it.** Rule 5 rejects everything that isn't SSH (port 22). Port 3003 is nowhere in the allowlist — every request from the jump host hits Rule 5 and gets killed.

---

### Step 7 — Add iptables Rule for Port 3003

```bash
# Insert ACCEPT rule at position 5 — before the REJECT
sudo /sbin/iptables -I INPUT 5 -p tcp --dport 3003 -j ACCEPT
```

**Verify the rule is in place:**
```bash
sudo /sbin/iptables -L -n --line-numbers
```

**Expected output:**
```
num  target   prot  source      destination
1    ACCEPT   all   ...         state RELATED,ESTABLISHED
2    ACCEPT   icmp  ...
3    ACCEPT   all   ...
4    ACCEPT   tcp   ...         tcp dpt:22
5    ACCEPT   tcp   ...         tcp dpt:3003  ← NEW RULE ✅
6    REJECT   all   ...         reject-with icmp-host-prohibited
```

---

### Step 8 — Final Verification from Jump Host

```bash
exit  # back to jump host

curl http://stapp01:3003
# Returns CentOS test page HTML ✅
```

---

## The Networking Concept Behind This

### Why `curl localhost:3003` worked but jump host failed

This confused me at first — the service was working locally but not externally. Here's why:

```
localhost request  →  hits loopback interface (Rule 3 - ACCEPT)  →  reaches Apache ✅
jump host request  →  hits network interface  →  Rule 1,2,3,4 no match  →  Rule 5 REJECT ❌
```

iptables rules apply differently to loopback vs external network traffic. **Always test from outside the server** — local success means nothing if the firewall blocks external access.

---

### Why `-I INPUT 5` and Not `-A` (append)

This is critical. `-A` appends to the **end** of the chain:

```
# WRONG — appending after the REJECT rule
Rule 4: ACCEPT port 22
Rule 5: REJECT all      ← traffic dies here
Rule 6: ACCEPT port 3003  ← never reached!
```

`-I INPUT 5` **inserts** at position 5, pushing REJECT down to position 6:

```
# RIGHT — inserting before the REJECT rule
Rule 4: ACCEPT port 22
Rule 5: ACCEPT port 3003  ← matched here ✅
Rule 6: REJECT all
```

**iptables reads rules top to bottom and acts on the first match. Order is everything.**

---

### The Full Traffic Flow After Fix

```
Jump Host (thor)
      │
      │  curl http://stapp01:3003
      │
      ▼
iptables INPUT chain (stapp01)
      │
      ├── Rule 1: established? NO
      ├── Rule 2: ICMP? NO
      ├── Rule 3: loopback? NO (external traffic)
      ├── Rule 4: port 22? NO
      ├── Rule 5: port 3003? YES ✅ → ACCEPT
      │
      ▼
Apache httpd (listening on 3003)
      │
      ▼
HTML response → Jump Host ✅
```

---

## Troubleshooting Quick Reference

**🔴 Apache fails with "Address already in use"**
```bash
sudo ss -tlnp | grep <port>
# Find what process is using it, then stop it
sudo systemctl stop <process>
```

**🔴 `curl localhost` works but jump host fails**
```bash
# Firewall is the culprit — check iptables
sudo /sbin/iptables -L -n --line-numbers
# Look for a REJECT rule near the bottom of INPUT chain
```

**🔴 `firewall-cmd: command not found`**
```bash
# Server uses iptables, not firewalld
sudo /sbin/iptables -L -n --line-numbers
```

**🔴 iptables rule added but still blocked**
```bash
# Check rule order — ACCEPT must come BEFORE the REJECT
sudo /sbin/iptables -L -n --line-numbers
# If ACCEPT is after REJECT, delete it and re-insert correctly
sudo /sbin/iptables -D INPUT <wrong-rule-number>
sudo /sbin/iptables -I INPUT <position-before-reject> -p tcp --dport <port> -j ACCEPT
```

---

## Key Learnings

**1. Always read the actual error message**
`(98) Address already in use` told the entire story of Issue 1. The error message pointed straight to the cause — a port conflict, not a broken config.

**2. Two processes cannot share a port**
Linux enforces this at the kernel level. Whichever process binds first wins. Any process that tries to bind to an occupied port will fail immediately. This is why startup order matters in service dependencies.

**3. iptables rules are evaluated top to bottom — first match wins**
This is the single most important thing to understand about iptables. A well-placed REJECT at the end is a common hardening pattern — it blocks everything not explicitly allowed above it. To add exceptions, always insert before the REJECT, never append after it.

**4. `-I` (insert) vs `-A` (append) is not interchangeable**
Using `-A` when a REJECT rule exists at the end of the chain means your new rule will never be evaluated. Always use `-I INPUT <position>` to place the rule exactly where it needs to be.

**5. Local success ≠ external reachability**
`curl localhost` bypasses the network firewall entirely via the loopback interface. A service can be perfectly healthy locally while being completely unreachable from outside. Always verify from the external vantage point — in this case, the jump host.

**6. Layered debugging saves time**
Going layer by layer — service → port conflict → firewall — means you never miss a cause and you understand exactly why each fix worked. Jumping straight to iptables would have fixed external access but Apache would still have been down.

---

## Environment

- **Platform:** KodeKloud — xFusionCorp / Stratos Datacenter
- **Server:** App Server 1 (`stapp01`)
- **OS:** CentOS / RHEL
- **Tools Used:** `systemctl`, `ss`, `curl`, `iptables`, `grep`
- **Skill Area:** Incident Response, Apache Administration, iptables Firewall, Port Conflict Resolution, Linux Networking

---

*Part of my hands-on DevOps learning journey on [KodeKloud](https://kodekloud.com). Debugged and resolved in a live multi-server lab — every output in this README is real.*
