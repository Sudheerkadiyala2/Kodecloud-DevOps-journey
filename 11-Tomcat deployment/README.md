# Tomcat Deployment of Java Web Application ☕

> **DevOps Project** | Apache Tomcat | Multi-Server Environment | KodeKloud Lab

---

## Let me be upfront about what this project actually taught me.

I went in thinking "deploy a WAR file, how hard can it be?" — and came out understanding how real enterprise Java deployments work: jump hosts, custom ports, XML configuration, and why `curl` is your best friend when you can't open a browser.

This project covers the full deployment lifecycle — from pulling an artifact off a jump host, to configuring a server, to verifying the app is actually live and serving traffic. That flow is the same whether you're deploying a hobby project or a production banking app.

---

## Project Overview

| Detail | Value |
|--------|-------|
| **App Server** | App Server 2 (`stapp02`) |
| **Web Server** | Apache Tomcat |
| **Custom Port** | `6400` (modified from default `8080`) |
| **App Artifact** | `ROOT.war` |
| **Artifact Source** | Jump Host server (via SCP) |
| **Deploy Directory** | Tomcat `webapps/` |
| **Verification URL** | `http://stapp02:6400` |
| **Platform** | KodeKloud — xFusionCorp Infrastructure |

---

## Architecture / Setup

Understanding the environment is half the battle. Here's how the pieces connect:

```
┌─────────────────────────────────────────────────────────────┐
│                     Jump Host (Thor)                        │
│                                                             │
│   ROOT.war  ◄── Artifact is staged here first              │
│       │                                                     │
└───────┼─────────────────────────────────────────────────────┘
        │
        │  scp (SSH file transfer)
        │
┌───────▼──────────────────────────────────────────────────────┐
│                   App Server 2 (stapp02)                     │
│                                                              │
│   /tmp/ROOT.war ──► /opt/tomcat/webapps/ROOT.war             │
│                                                              │
│   ┌──────────────────────────────────────────┐               │
│   │         Apache Tomcat (Port 6400)        │               │
│   │                                          │               │
│   │   webapps/                               │               │
│   │   └── ROOT.war  ──► Auto-deploys as /   │               │
│   │                                          │               │
│   │   conf/server.xml  ◄── Port changed here│               │
│   └──────────────────────────────────────────┘               │
│                              │                               │
└──────────────────────────────┼───────────────────────────────┘
                               │
                    curl http://stapp02:6400
                               │
                    ┌──────────▼──────────┐
                    │  HTTP 200 — App is  │
                    │  live ✅            │
                    └─────────────────────┘
```

**Why a Jump Host?**

In real infrastructure, application servers are usually on a **private network** — not directly reachable from the internet. A **Jump Host** (also called a Bastion Host) is a single, hardened entry point that sits between your workstation and the internal servers. You SSH into the jump host first, then hop to your target. It's a security boundary, not a complication.

In this lab, the WAR artifact lives on the Jump Host and needs to be `scp`'d to the App Server — mimicking how a real deployment pipeline would stage and transfer build artifacts.

---

## Prerequisites

Before starting, make sure these are in place:

**On the Jump Host:**
- [ ] `ROOT.war` file is present (confirm with `ls`)
- [ ] SSH access to App Server 2 is configured

**On App Server 2:**
- [ ] Java is installed (`java -version`)
- [ ] You have sudo access
- [ ] Port `6400` is not already in use

**Checking Java:**
```bash
java -version
# Expected: openjdk version "1.8.x" or higher
# If not installed:
sudo yum install -y java-1.8.0-openjdk   # RHEL/CentOS
sudo apt install -y default-jdk           # Ubuntu/Debian
```

**Checking if a port is already in use:**
```bash
ss -tlnp | grep 6400
# No output = port is free
```

---

## Installation Steps

### Step 1 — Download and Install Apache Tomcat

```bash
# Move to a working directory
cd /opt

# Download Tomcat (check https://tomcat.apache.org for latest version)
sudo wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.x/bin/apache-tomcat-9.0.x.tar.gz

# Extract it
sudo tar -xzf apache-tomcat-9.0.x.tar.gz

# Rename for convenience
sudo mv apache-tomcat-9.0.x tomcat

# Verify the structure
ls /opt/tomcat/
# bin/  conf/  lib/  logs/  temp/  webapps/  work/
```

### Step 2 — Set Permissions

```bash
# Give your user ownership (so you don't need sudo for every Tomcat operation)
sudo chown -R $(whoami):$(whoami) /opt/tomcat
```

### Step 3 — Make Startup Scripts Executable

```bash
chmod +x /opt/tomcat/bin/*.sh
```

---

## Configuration Changes — Custom Port

By default, Tomcat listens on port **8080**. This project requires port **6400**. Here's where and why that change lives.

### Editing `server.xml`

```bash
vim /opt/tomcat/conf/server.xml
```

Find the HTTP Connector block — it looks like this:

```xml
<!-- BEFORE — Default configuration -->
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Change `8080` to `6400`:

```xml
<!-- AFTER — Custom port -->
<Connector port="6400" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Save and exit.

> **Why change the port?**  
> In multi-application environments, multiple services often run on the same server. Port conflicts are common. Using a custom port (`6400`) avoids clashing with other services and demonstrates you understand how to adapt Tomcat beyond its defaults — a practical skill in any enterprise setup.

**Verify your edit looks correct before moving on:**
```bash
grep -n "6400" /opt/tomcat/conf/server.xml
# Should return the line number and the Connector config
```

---

## Deployment Steps — WAR File

### Step 1 — Locate the WAR File on the Jump Host

```bash
# On the Jump Host (run this first to confirm the file exists)
ls -lh ROOT.war
```

### Step 2 — Transfer WAR File to App Server 2

```bash
# Run this from the Jump Host
scp ROOT.war steve@stapp02:/tmp/

# Syntax breakdown:
# scp           → secure copy over SSH
# ROOT.war      → source file (on jump host)
# steve@stapp02 → username@destination-server
# :/tmp/        → destination path on the remote server
```

> If you see `Permission denied`, check that SSH key auth is set up from the Jump Host to App Server 2. Refer to the SSH key setup section in the [Website Backup project](../website-backup/README.md).

### Step 3 — Move WAR to Tomcat's webapps Directory

```bash
# Now SSH into App Server 2
ssh steve@stapp02

# Move the WAR file into Tomcat's deployment directory
mv /tmp/ROOT.war /opt/tomcat/webapps/

# Verify it's there
ls -lh /opt/tomcat/webapps/
```

> **Why `ROOT.war` specifically?**  
> Tomcat has a naming convention for WAR files. A file named `ROOT.war` deploys to the **root context** (`/`) of the server. So `http://stapp02:6400/` serves this app directly. A file named `myapp.war` would deploy to `http://stapp02:6400/myapp/`. Naming matters.

### Step 4 — Start (or Restart) Tomcat

```bash
# If Tomcat isn't running yet — start it
/opt/tomcat/bin/startup.sh

# If it was already running — restart to apply the port change
/opt/tomcat/bin/shutdown.sh
sleep 3
/opt/tomcat/bin/startup.sh

# Watch the startup logs in real time
tail -f /opt/tomcat/logs/catalina.out
# Look for: "Server startup in [X] milliseconds"
# Press Ctrl+C to stop watching
```

---

## Testing & Verification

The moment of truth. Let's confirm the app is actually running and serving traffic.

```bash
# Basic check — are we getting a response?
curl http://stapp02:6400

# Cleaner output — just the HTTP status code
curl -o /dev/null -s -w "%{http_code}" http://stapp02:6400
# Expected: 200

# Verbose mode — see headers, redirect info, everything
curl -v http://stapp02:6400
```

**What to look for:**

| Result | Meaning |
|--------|---------|
| `200 OK` | ✅ App is deployed and serving traffic |
| `404 Not Found` | ROOT.war may not have deployed correctly |
| `Connection refused` | Tomcat isn't running or wrong port |
| `403 Forbidden` | Tomcat is running but app has access restrictions |

**Also check Tomcat's own logs:**
```bash
# Main Tomcat log
cat /opt/tomcat/logs/catalina.out | tail -30

# Access log (shows incoming requests)
ls /opt/tomcat/logs/
cat /opt/tomcat/logs/localhost_access_log.*.txt
```

---

## Troubleshooting Tips

Things will go wrong. Here's where to look when they do.

**🔴 Tomcat won't start**
```bash
# Read the logs — the error is almost always in here
cat /opt/tomcat/logs/catalina.out | tail -50

# Check if something else is already using port 6400
ss -tlnp | grep 6400

# Check Java is installed
java -version
```

**🔴 `curl` says "Connection refused"**
```bash
# Is Tomcat actually running?
ps aux | grep tomcat

# Is it listening on 6400?
ss -tlnp | grep 6400

# Did the port change save correctly?
grep "6400" /opt/tomcat/conf/server.xml
```

**🔴 App deployed but getting 404**
```bash
# Check that ROOT.war is in the right place
ls -lh /opt/tomcat/webapps/

# Tomcat should auto-extract it into a ROOT/ folder
# If you don't see ROOT/ directory, the WAR hasn't deployed yet
ls /opt/tomcat/webapps/ROOT/

# Check for deployment errors
grep -i "error\|exception" /opt/tomcat/logs/catalina.out
```

**🔴 `scp` fails with "Permission denied"**
```bash
# Test SSH connectivity first
ssh steve@stapp02 "echo connection works"

# If that fails, check SSH keys
ls ~/.ssh/
# You need id_rsa (private key) and it must be in authorized_keys on stapp02
```

**🔴 Changes to `server.xml` not taking effect**
```bash
# You must restart Tomcat after any config change
/opt/tomcat/bin/shutdown.sh && sleep 2 && /opt/tomcat/bin/startup.sh
```

---

## Key Learnings

**1. Jump Hosts are a security pattern, not a barrier**  
They exist to protect internal servers from direct internet exposure. Learning to work with them — staging files, hopping between hosts — is essential in any enterprise environment.

**2. WAR files are the deployment unit for Java apps**  
Java web apps get packaged as `.war` (Web Application Archive) files. Tomcat's `webapps/` directory is a hot-deploy zone — drop a WAR in, and Tomcat unpacks and deploys it automatically. No restart needed for the app itself (though config changes like port require a restart).

**3. `server.xml` is Tomcat's brain**  
Almost every important Tomcat configuration — ports, connectors, virtual hosts, SSL — lives in `conf/server.xml`. Understanding its structure is what separates someone who "can install Tomcat" from someone who can actually administer it.

**4. The `ROOT` context is special**  
Naming your WAR `ROOT.war` deploys it at the server root (`/`). This is intentional — it's how you make an app the "default" application on a Tomcat instance. Any other name becomes a sub-path.

**5. Always check logs before assuming**  
`catalina.out` tells you almost everything. Startup errors, deployment failures, Java exceptions — it's all there. Checking logs first is a habit that saves enormous time.

**6. `curl` is your remote browser**  
When you're on a server with no GUI, `curl` is how you verify HTTP. `curl -o /dev/null -s -w "%{http_code}"` is a clean way to get just the status code — useful in scripts and health checks.

---

## Project File Structure

```
tomcat-deployment/
├── README.md                  ← You are here
└── configs/
    └── server.xml.snippet     ← The Connector block change (reference)
```

---

## Environment

- **Platform:** KodeKloud — xFusionCorp Infrastructure
- **Jump Host:** Thor (bastion/jump server)
- **App Server:** stapp02 (App Server 2)
- **Tools Used:** `scp`, `ssh`, `curl`, `wget`, `tar`, `vim`, Apache Tomcat
- **Skill Area:** Java App Deployment, Tomcat Administration, Linux Server Config, Artifact Transfer

---

*Part of my hands-on DevOps learning journey on [KodeKloud](https://kodekloud.com). Built and verified in a live multi-server lab environment.*
