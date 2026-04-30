# Day 1 — Linux User Setup with Non-Interactive Shell 🐧

> **100 days of DevOps** | Challenge #1 

---

## What's This About?

Ever wondered how servers handle service accounts — users that run background processes but should never be allowed to log in interactively? That's exactly what this challenge is about.

On Day 1, I tackled setting up a Linux user with a **non-interactive shell** (`/sbin/nologin` or `/bin/false`). It sounds simple, but understanding *why* this matters is what separates a good DevOps engineer from a great one.

---

## The Problem

In production environments, you often need users for:
- Running daemons (nginx, mysql, redis...)
- CI/CD pipeline agents
- Application service accounts

These users **should never have shell access**. Leaving them with a default shell is a security risk — if compromised, an attacker could potentially use that account to execute commands interactively.

---

## What I Did

```bash
# Create a user with no interactive shell
sudo useradd -s /sbin/nologin serviceuser

# Verify the shell is correctly set
grep serviceuser /etc/passwd

# Confirm login is blocked
sudo su - serviceuser
# Output: "This account is currently not available."
```

### Breakdown

| Flag | What It Does |
|------|-------------|
| `-s /sbin/nologin` | Assigns a non-interactive shell |
| `-r` | Creates a system account (no home dir by default) |
| `-M` | Explicitly skip home directory creation |

---

## What I Learned

- The difference between `/sbin/nologin` and `/bin/false` (both block login, but `nologin` gives a friendly message)
- How `/etc/passwd` stores user metadata — and how to read it
- Why service accounts should always follow the **principle of least privilege**
- The `id`, `whoami`, and `getent passwd` commands for user verification

---

## Real-World Use Case

This is standard practice when installing services like:
```bash
# When you install nginx, it creates its own nologin user
cat /etc/passwd | grep nginx
# nginx:x:998:996:nginx web server:/var/lib/nginx:/sbin/nologin
```

---

## Key Takeaway

> Small misconfigurations compound into big vulnerabilities. Locking down service accounts on Day 1 is the DevOps mindset in action — **security isn't an afterthought, it's a habit.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `useradd`, `usermod`, `passwd`, `getent`
- **Skill Area:** Linux Administration, Security Hardening

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
