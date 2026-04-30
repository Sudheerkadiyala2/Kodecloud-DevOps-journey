# Day 3 — Secure Root SSH Access 🔐

> **100 Days of DevOps** | Challenge #3 | April 29, 2026

---

## What's This About?

If there's one thing every security guide, every hardening checklist, and every SysAdmin mentor will tell you — it's this: **disable root SSH login.** Day 3 was all about understanding why, and actually doing it.

This isn't just checkbox security. It's one of the first things attackers probe when they hit a new server.

---

## The Problem

By default on many systems, root can log in directly over SSH. That means:

- Brute-force attacks target the **known username** (`root`) directly
- A compromised key = instant full system access
- No audit trail of *who* actually logged in as root (just that "root" did)

The goal is to **eliminate direct root SSH access** while still preserving the ability to do admin work via `sudo`.

---

## What I Did

### Step 1 — Edit the SSH Daemon Config

```bash
sudo vim /etc/ssh/sshd_config
```

Find and update this line:
```
# Before
PermitRootLogin yes

# After
PermitRootLogin no
```

### Step 2 — Restart SSH Service

```bash
sudo systemctl restart sshd

# Verify it's running clean
sudo systemctl status sshd
```

### Step 3 — Test Before Closing Your Session!

```bash
# In a SEPARATE terminal (don't close your existing session)
ssh root@your-server-ip
# Expected: "Permission denied, please try again."
```

> ⚠️ **Always test in a new terminal before closing your existing SSH session.** Locking yourself out is a rite of passage — but let's skip that part.

---

## What I Learned

- The difference between `PermitRootLogin no`, `PermitRootLogin prohibit-password`, and `PermitRootLogin forced-commands-only`
- Why `prohibit-password` is sometimes the safer middle ground (blocks password auth, allows key-based for automation)
- How to verify active SSH config without restarting the service: `sshd -T | grep permitroot`
- The importance of **keeping a session open while testing** SSH config changes

---

## Bonus Hardening

While I had the config open, I also reviewed these:

```bash
# Disable password authentication entirely (keys only)
PasswordAuthentication no

# Limit which users can SSH in
AllowUsers youruser deployuser

# Change default SSH port (minor obscurity, but reduces noise)
Port 2222
```

---

## Gotcha I Hit

On some cloud providers (AWS, GCP), the default AMI already has `PermitRootLogin` set to `prohibit-password` — root login via password is blocked, but key-based is allowed for initial setup. Know your baseline before you start.

```bash
# Check current effective SSH config
sudo sshd -T | grep -E "permitroot|passwordauth|port"
```

---

## Real-World Context

Most compliance frameworks (CIS Benchmarks, PCI-DSS, SOC2) explicitly require disabling root SSH. This isn't just good practice — in many industries, it's a **hard requirement** for passing audits.

---

## Key Takeaway

> Root SSH access is a loaded gun sitting on the table. You don't have to pull the trigger for it to be dangerous — **remove it from the equation entirely.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `sshd_config`, `systemctl`, `sshd -T`
- **Skill Area:** SSH Hardening, Linux Security, Server Administration

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
