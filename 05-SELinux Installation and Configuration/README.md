# Day 5 — SELinux Installation and Configuration 🛡️

> **100 Days of DevOps** | Challenge #5 | April 29, 2026

---

## What's This About?

SELinux is the one thing most Linux admins secretly Google every time they touch it. It's confusing, the errors are cryptic, and the first instinct of many engineers is to just... disable it.

Day 5 was about breaking that habit. **Understanding SELinux instead of fighting it.**

---

## The Problem

Traditional Linux permissions (DAC — Discretionary Access Control) let file owners decide who accesses what. SELinux adds **Mandatory Access Control (MAC)** — even root is subject to SELinux policies.

The challenge: get comfortable with SELinux modes, contexts, and troubleshooting — because in enterprise environments, you can't just turn it off.

---

## What I Did

### Check Current SELinux Status

```bash
# Get the current mode
sestatus

# Quick one-liner
getenforce
# Output: Enforcing / Permissive / Disabled
```

### SELinux Modes Explained

| Mode | What It Does |
|------|-------------|
| **Enforcing** | Policies are enforced. Violations are blocked AND logged. |
| **Permissive** | Violations are logged but NOT blocked. Great for debugging. |
| **Disabled** | SELinux is completely off. Not recommended on prod. |

### Switching Modes (Temporarily)

```bash
# Switch to permissive (survives until reboot)
sudo setenforce 0

# Switch back to enforcing
sudo setenforce 1
```

### Making Changes Permanent

```bash
sudo vim /etc/selinux/config

# Change this line:
SELINUX=enforcing   # or permissive, or disabled
```

> ⚠️ Disabling SELinux requires a reboot and a full filesystem relabel. Don't do it on a whim in production.

### Installing SELinux Tools (if not present)

```bash
# RHEL/CentOS
sudo yum install -y policycoreutils policycoreutils-python-utils setools-console

# Ubuntu/Debian
sudo apt install -y selinux-basics selinux-policy-default auditd
sudo selinux-activate
```

---

## Practical — Reading SELinux Contexts

```bash
# View file security context
ls -Z /etc/passwd
# system_u:object_r:passwd_file_t:s0 /etc/passwd

# View process context
ps auxZ | grep nginx
# system_u:system_r:httpd_t:s0 ... nginx
```

The context format: `user:role:type:level`  
The **type** is what most policies care about — it defines what the process/file is allowed to do.

---

## Troubleshooting SELinux Denials

```bash
# Check audit log for denials
sudo ausearch -m avc -ts recent

# Use audit2why for human-readable explanation
sudo ausearch -m avc -ts recent | audit2why

# Generate a custom policy module to allow a specific action
sudo ausearch -m avc -ts recent | audit2allow -M mypolicy
sudo semodule -i mypolicy.pp
```

---

## What I Learned

- Why "just disable SELinux" is a red flag in any serious DevOps role
- The difference between **targeted** and **strict** SELinux policies
- How to use `restorecon` to fix mislabeled files (huge for web server issues)
- Reading `/var/log/audit/audit.log` to diagnose denials
- The `sealert` command for friendly denial analysis

---

## A Real-World Scenario I Simulated

```bash
# Classic problem: you move a file instead of copying it — SELinux context breaks
cp /var/www/html/index.html /tmp/
mv /tmp/index.html /var/www/html/

# The moved file retains its /tmp context — nginx can't read it
# Fix:
sudo restorecon -v /var/www/html/index.html
```

---

## Key Takeaway

> SELinux isn't your enemy — it's the last line of defense between a misconfigured service and full system compromise. **Learn to work with it, not around it.** Your future self (and your security team) will thank you.

---

## Environment

- **OS:** RHEL / CentOS / Fedora (SELinux native), Ubuntu (with selinux-basics)
- **Tools:** `sestatus`, `setenforce`, `ausearch`, `audit2allow`, `restorecon`, `chcon`
- **Skill Area:** Linux Security, SELinux, Mandatory Access Control

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
