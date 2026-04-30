# Day 4 — Script Execution Permissions 📜

> **100 Days of DevOps** | Challenge #4 | April 29, 2026

---

## What's This About?

You've written a brilliant shell script. You run it. `Permission denied`. 

We've all been there. Day 4 is about truly understanding Linux file permissions — not just memorizing `chmod +x`, but knowing *what* you're granting, *to whom*, and *why it matters*.

---

## The Problem

Shell scripts don't execute by default after creation. They're just text files until you explicitly tell the OS they're executable. But blindly running `chmod 777` on everything is a terrible habit that'll get you fired (or hacked).

The challenge: understand and apply **precise, least-privilege permissions** to scripts.

---

## What I Did

### The Basics — Understanding Permission Bits

```bash
# Create a simple script
cat > health_check.sh << 'EOF'
#!/bin/bash
echo "Server is up: $(hostname)"
echo "Uptime: $(uptime -p)"
EOF

# Check current permissions
ls -la health_check.sh
# -rw-r--r-- 1 user group 68 Apr 29 health_check.sh
```

### Reading Permission Output

```
-rw-r--r--
│├┤├┤├┤
││ │ │ └── Others: read only
││ │ └──── Group: read only  
││ └────── Owner: read + write
│└──────── File type (- = regular file)
```

### Making It Executable (The Right Way)

```bash
# Only owner can execute
chmod u+x health_check.sh
# -rwxr--r--

# Owner + group can execute
chmod ug+x health_check.sh
# -rwxr-xr--

# Numeric equivalent — most common for scripts
chmod 750 health_check.sh
# -rwxr-x---  (owner: rwx, group: r-x, others: none)
```

### Verifying and Running

```bash
# Confirm permissions
ls -la health_check.sh

# Run it
./health_check.sh
bash health_check.sh  # This works even without +x (bash interprets it directly)
```

---

## The Numeric Cheat Sheet

| Number | Permission | Meaning |
|--------|-----------|---------|
| 7 | rwx | Read + Write + Execute |
| 6 | rw- | Read + Write |
| 5 | r-x | Read + Execute |
| 4 | r-- | Read only |
| 0 | --- | No permissions |

**Common patterns for scripts:**
- `755` — Owner full, everyone else read+execute (public scripts)
- `750` — Owner full, group read+execute, others nothing (team scripts)
- `700` — Owner only (sensitive scripts with credentials)

---

## What I Learned

- `chmod +x` vs `chmod 755` — the difference matters when you care about group/other access
- The `setuid` and `setgid` bits (advanced, but good to know they exist)
- Using `stat` for detailed permission info: `stat -c "%a %n" script.sh`
- Why scripts with hardcoded secrets should **never** be world-readable
- `umask` — the default permission filter applied at file creation

---

## Gotcha I Hit

```bash
# This works even without execute permission
bash myscript.sh

# But this requires +x
./myscript.sh

# Automating via cron? It needs +x. 
# Many cron job issues trace back to missing execute permissions.
```

---

## Security Note

```bash
# BAD — never do this for scripts (especially with secrets)
chmod 777 deploy.sh

# GOOD — owner executes, group can read, others can't touch it
chmod 740 deploy.sh
```

---

## Key Takeaway

> Permissions are your first line of defense on the filesystem. `chmod +x` is fine for learning — but in production, **always think about who should execute what, and grant exactly that, nothing more.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `chmod`, `chown`, `ls -la`, `stat`, `umask`
- **Skill Area:** Linux Permissions, Shell Scripting, Security Fundamentals

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
