# Day 7 — Linux SSH Authentication 🔑

> **100 Days of DevOps** | Challenge #7 | April 29, 2026

---

## What's This About?

Passwords are convenient. SSH keys are secure. In production infrastructure, there's really only one acceptable answer — and it's not passwords.

Day 7 was about setting up **SSH key-based authentication** from scratch, understanding how it works under the hood, and locking down the configuration properly.

---

## The Problem

Password-based SSH has several problems in modern infrastructure:
- Vulnerable to brute-force attacks
- Hard to rotate across many servers
- No easy way to revoke a single user's access without changing shared passwords
- Doesn't scale when you have 100+ servers

SSH keys solve all of this. They're cryptographically strong, individually revocable, and can be automated safely.

---

## How SSH Key Auth Works (The Short Version)

```
Client                          Server
  │                               │
  │── Hey, I have a key ─────────►│
  │                               │ (checks ~/.ssh/authorized_keys)
  │◄─ Here's an encrypted         │
  │   challenge ──────────────────│
  │                               │
  │── Decrypted with my           │
  │   private key ───────────────►│
  │                               │ ✅ Access granted
```

Your **private key** never leaves your machine. The server only holds your **public key**. The math does the rest.

---

## What I Did

### Step 1 — Generate an SSH Key Pair

```bash
# Generate a modern Ed25519 key (faster and more secure than RSA)
ssh-keygen -t ed25519 -C "yourname@devops-challenge" -f ~/.ssh/devops_key

# Or RSA 4096 for broader compatibility
ssh-keygen -t rsa -b 4096 -C "yourname@devops-challenge" -f ~/.ssh/devops_key

# You'll get two files:
# ~/.ssh/devops_key       ← Private key (NEVER share this)
# ~/.ssh/devops_key.pub   ← Public key (safe to share)
```

### Step 2 — Copy Public Key to Server

```bash
# The easy way (if password auth is still enabled)
ssh-copy-id -i ~/.ssh/devops_key.pub user@server-ip

# Manual way (good to understand)
cat ~/.ssh/devops_key.pub
# Copy the output, then on the server:
mkdir -p ~/.ssh
echo "paste-your-public-key-here" >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### Step 3 — Test Key-Based Login

```bash
ssh -i ~/.ssh/devops_key user@server-ip
```

### Step 4 — Disable Password Authentication

```bash
sudo vim /etc/ssh/sshd_config

# Make sure these are set:
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

sudo systemctl restart sshd
```

---

## SSH Config File — Work Smarter

Instead of typing `ssh -i ~/.ssh/devops_key -p 22 user@192.168.1.100` every time:

```bash
# ~/.ssh/config
Host myserver
    HostName 192.168.1.100
    User devopsuser
    IdentityFile ~/.ssh/devops_key
    Port 22

Host staging
    HostName staging.mycompany.com
    User deploy
    IdentityFile ~/.ssh/deploy_key
```

Now you just type:
```bash
ssh myserver
```

---

## What I Learned

- `Ed25519` vs `RSA` — Ed25519 is the modern standard (smaller keys, faster, equally secure)
- Why file permissions on `~/.ssh/` matter — SSH will refuse to use keys if permissions are too open
- How `ssh-agent` works for passphrase management in automation
- `ssh-add` for loading keys into the agent during a session
- Auditing authorized keys across multiple servers — this is where tools like Ansible shine

---

## Permissions That Must Be Correct

```bash
# If these are wrong, SSH silently ignores your keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/private_key
chmod 644 ~/.ssh/public_key.pub
```

---

## Key Takeaway

> SSH key authentication isn't just "more secure" — it's the foundation of everything that follows: Ansible playbooks, CI/CD deployments, cloud automation. **Get comfortable with keys early. They're everywhere.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `ssh-keygen`, `ssh-copy-id`, `ssh-agent`, `sshd_config`
- **Skill Area:** SSH, Authentication, Security, Infrastructure Access

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
