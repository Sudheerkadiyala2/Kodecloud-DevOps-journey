# 🔐 SSH Root Login Security Hardening – Stratos Datacenter

## 📌 Overview
Implementation of a key security policy in the Stratos Datacenter environment of xFusionCorp Industries — disabling direct SSH root login across all application servers as part of a security audit.

---

## 🎯 Objective
Disable direct SSH root login on all application servers in the Stratos Datacenter to prevent unauthorized root access and improve overall system security posture.

---

## 🖥️ Affected Servers

| Server | User |
|--------|------|
| stapp01 | tony |
| stapp02 | steve |
| stapp03 | banner |

---

## ⚙️ Implementation Steps

### 1. Connect to each application server
```bash
ssh <user>@<server-name>
```

### 2. Open SSH configuration file
```bash
sudo vi /etc/ssh/sshd_config
```

### 3. Modify the PermitRootLogin parameter
Find and update:
```bash
PermitRootLogin no
```

### 4. Restart SSH service to apply changes
```bash
sudo systemctl restart sshd
```

---

## 🔍 Verification

Attempt root login to confirm it is blocked:
```bash
ssh root@<server-name>
```

**Expected result:**
```
Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
```
This confirms direct root SSH access is successfully disabled.

---

## 🔐 Security Impact

- Prevents direct root access over SSH
- Reduces attack surface for brute-force attempts on root account
- Enforces least-privilege access model
- Encourages use of `sudo` for all administrative tasks

---

## 🧠 What I Learned

- Linux SSH configuration management (`/etc/ssh/sshd_config`)
- Server hardening practices and security compliance
- Role-based server administration across multiple hosts
- Applying and verifying security policies in a simulated datacenter environment

---

## 📁 Related Concepts

- `PermitRootLogin` — controls whether root can log in via SSH
- `sshd_config` — main SSH daemon configuration file
- `systemctl restart sshd` — applies config changes without rebooting
- Least privilege principle — users should have minimum access required

---

*Part of KodeKloud Engineer Pro — Linux Administration tasks | Stratos Datacenter series*
