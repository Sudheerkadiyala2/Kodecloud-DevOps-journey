# Website Backup Automation Script 🗄️

> **DevOps Bash Scripting Project** | KodeKloud Lab Environment

---

## Hey, let me tell you what this project is really about.

Backups are one of those things everyone knows they should do — and nobody actually does consistently until something breaks. This project is about **removing the human from that equation entirely.**

I built a bash script that automatically zips a live website directory, stores it locally, and ships it off to a remote storage server — all without a single password prompt. Once it's set up, it just works. Every time.

This is a foundational DevOps pattern: **automate the boring stuff before it becomes the critical stuff.**

---

## Project Overview

| Detail | Value |
|--------|-------|
| **Script Name** | `news_backup.sh` |
| **Script Location** | `/scripts/news_backup.sh` on App Server 1 |
| **Source Directory** | `/var/www/html/news` |
| **Archive Name** | `xfusioncorp_news.zip` |
| **Local Backup Path** | `/backup/` on App Server 1 |
| **Remote Backup Path** | `/backup/` on Nautilus Storage Server |
| **Auth Method** | Passwordless SSH (key-based) |
| **Runs as** | Regular user — no `sudo` inside the script |

---

## Architecture / Workflow

Here's the full picture of what happens when this script runs:

```
┌─────────────────────────────────────────────────────┐
│                   App Server 1                      │
│                                                     │
│  /var/www/html/news/          /backup/              │
│  ┌─────────────────┐          ┌──────────────────┐  │
│  │  Static Website │ ──zip──► │xfusioncorp_      │  │
│  │  Files          │          │news.zip          │  │
│  └─────────────────┘          └────────┬─────────┘  │
│                                        │             │
└────────────────────────────────────────┼─────────────┘
                                         │
                                    scp (SSH key)
                                    no password
                                         │
┌────────────────────────────────────────┼─────────────┐
│              Nautilus Storage Server   │             │
│                                        ▼             │
│                               /backup/               │
│                               ┌──────────────────┐   │
│                               │ xfusioncorp_     │   │
│                               │ news.zip  ✅     │   │
│                               └──────────────────┘   │
└─────────────────────────────────────────────────────┘
```

**Step-by-step flow:**
1. Script runs on App Server 1
2. Zips `/var/www/html/news` into `xfusioncorp_news.zip`
3. Saves the zip to local `/backup/` directory
4. Uses `scp` over SSH (key-based, no password) to push the zip to the Storage Server
5. Done — backup exists in two places

---

## Prerequisites

Before running the script, make sure these are in place:

**On App Server 1:**
- [ ] `zip` package installed
- [ ] `/scripts/` directory exists
- [ ] `/backup/` directory exists
- [ ] SSH key pair generated
- [ ] Script has execute permission

**On Nautilus Storage Server:**
- [ ] `/backup/` directory exists and is writable
- [ ] Public key of App Server 1 is in `~/.ssh/authorized_keys`

**Installing `zip` manually (required — do not put this in the script):**
```bash
# RHEL / CentOS
sudo yum install -y zip

# Ubuntu / Debian
sudo apt install -y zip

# Verify
zip --version
```

> ⚠️ The script assumes `zip` is already installed. Installing packages is a setup step — not something a backup script should be doing.

---

## Setup Steps

### Step 1 — Create Required Directories

```bash
# On App Server 1
sudo mkdir -p /scripts
sudo mkdir -p /backup

# Set ownership so your user can write to them
sudo chown $(whoami):$(whoami) /scripts /backup
```

### Step 2 — Set Up Passwordless SSH to Storage Server

This is the most important step. `scp` inside a script can't accept password prompts — so we set up key-based authentication first.

```bash
# 1. Generate an SSH key pair on App Server 1 (skip if you already have one)
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
#  -N "" means no passphrase — required for non-interactive scripts

# 2. Copy the public key to the Storage Server
ssh-copy-id user@nautilus-storage-server
# Enter the password once — this is the last time you'll need it

# 3. Test that it works without a password
ssh user@nautilus-storage-server "echo SSH is working"
# You should see: SSH is working  (no password prompt)
```

> ✅ If that last command returned output without asking for a password, you're good to go.

### Step 3 — Create the Backup Directory on Storage Server

```bash
# Run this from App Server 1 (uses the SSH key you just set up)
ssh user@nautilus-storage-server "mkdir -p /backup"
```

### Step 4 — Create the Script

```bash
# Create and open the script file
vim /scripts/news_backup.sh
```

Paste the script content (see next section), save and exit.

```bash
# Make it executable
chmod +x /scripts/news_backup.sh
```

---

## The Script

```bash
#!/bin/bash
# ============================================================
# news_backup.sh
# Automates backup of the /var/www/html/news website directory
# Backs up locally and ships to remote storage via SCP
# ============================================================

# --- Configuration ---
SOURCE_DIR="/var/www/html/news"
ARCHIVE_NAME="xfusioncorp_news.zip"
LOCAL_BACKUP="/backup"
REMOTE_USER="natasha"
REMOTE_HOST="ststor01"
REMOTE_PATH="/backup"

# --- Create the zip archive ---
zip -r "${LOCAL_BACKUP}/${ARCHIVE_NAME}" "${SOURCE_DIR}"

# --- Copy to remote storage server ---
scp "${LOCAL_BACKUP}/${ARCHIVE_NAME}" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/"
```

### Breaking It Down

| Line | What It Does |
|------|-------------|
| `#!/bin/bash` | Shebang — tells the OS to run this with bash |
| Variables block | Centralizes config — change host/paths in one place |
| `zip -r` | Recursively zips the entire `news` directory |
| `"${LOCAL_BACKUP}/${ARCHIVE_NAME}"` | Full path to where the zip is saved locally |
| `scp` | Secure copy over SSH — uses key auth, no password |
| `${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/` | Destination: `user@host:/path/` |

**Why no `sudo` in the script?**  
The script runs as a user who already has permission to read the source and write to `/backup/`. Using `sudo` inside scripts is bad practice — it assumes escalation will always be available and can cause issues in automated environments (cron, CI/CD pipelines, etc.).

---

## How to Run the Script

```bash
# Run manually
/scripts/news_backup.sh

# Or with bash explicitly
bash /scripts/news_backup.sh

# To schedule it (e.g., daily at 2 AM) — add to crontab
crontab -e
# Add this line:
0 2 * * * /scripts/news_backup.sh >> /var/log/news_backup.log 2>&1
```

---

## Verification Steps

After running the script, verify both the local and remote backups exist:

```bash
# 1. Check local backup was created
ls -lh /backup/xfusioncorp_news.zip
# Should show the file with a non-zero size

# 2. Verify the zip is valid (not corrupted)
zip -T /backup/xfusioncorp_news.zip
# Expected: test of /backup/xfusioncorp_news.zip OK

# 3. List contents of the zip
unzip -l /backup/xfusioncorp_news.zip

# 4. Check remote backup on Storage Server
ssh user@nautilus-storage-server "ls -lh /backup/xfusioncorp_news.zip"
# Should match the local file size
```

**All four checks passing = backup is working correctly ✅**

---

## Key Learnings

**1. Variables make scripts maintainable**  
Putting `REMOTE_HOST`, `REMOTE_USER`, and paths as variables at the top means the script can be adapted for a different server or path in seconds — without digging through the logic.

**2. Passwordless SSH is a prerequisite, not an afterthought**  
`scp` inside a script fails silently (or hangs waiting for input) if SSH key auth isn't set up first. Always configure and *test* key auth before writing the script that depends on it.

**3. No `sudo` in automated scripts**  
Scripts should be designed to run with the exact permissions they need — not rely on privilege escalation. If a script needs `sudo`, that's a signal that directory permissions or ownership should be fixed instead.

**4. `zip -r` vs `tar`**  
Both work for archiving. `zip` is more portable and easier to inspect/extract on any OS. `tar + gzip` is more common in Linux-native workflows. For a web backup that might be accessed on multiple systems, `zip` is the friendlier choice.

**5. Always verify both ends**  
A backup script that silently fails is worse than no backup script — it gives you false confidence. Always check local AND remote after a run, and eventually add logging or error handling for production use.

---

## Project Structure

```
/
├── scripts/
│   └── news_backup.sh        ← The backup script
├── backup/
│   └── xfusioncorp_news.zip  ← Local backup archive
└── var/
    └── www/
        └── html/
            └── news/         ← Source website directory
```

---

## What I'd Add in a Production Setup

This script is intentionally simple for the lab environment. In a real production scenario, I'd extend it with:

```bash
# Timestamped backups (so you don't overwrite yesterday's)
ARCHIVE_NAME="xfusioncorp_news_$(date +%Y%m%d_%H%M%S).zip"

# Exit on error — stop the script if zip fails
set -e

# Logging
echo "[$(date)] Backup started" >> /var/log/news_backup.log

# Retention cleanup — delete local backups older than 7 days
find /backup -name "xfusioncorp_news_*.zip" -mtime +7 -delete
```

---

## Environment

- **Platform:** KodeKloud Lab — xFusionCorp Infrastructure
- **App Server:** App Server 1 (Centos/RHEL based)
- **Storage Server:** Nautilus Storage Server
- **Tools Used:** `bash`, `zip`, `scp`, `ssh-keygen`, `ssh-copy-id`, `crontab`
- **Skill Area:** Bash Scripting, SSH, Backup Automation, Linux Administration

---

*Part of my hands-on DevOps learning journey on [KodeKloud](https://kodekloud.com). Every project here was built and tested in a live lab environment — not just read about.*
