# Day 6 — Create a Cron Job ⏰

> **100 Days of DevOps** | Challenge #6 | April 29, 2026

---

## What's This About?

Automation is the soul of DevOps. And before we talk about fancy schedulers like Kubernetes CronJobs or Airflow — there's the humble, reliable, always-there `cron`. It's been scheduling tasks since 1975 and it's not going anywhere.

Day 6 was about getting hands-on with cron: the syntax, the gotchas, and building something actually useful.

---

## The Problem

Manual tasks that need to happen regularly are a liability — they get forgotten, done inconsistently, or skipped when the team is busy. Whether it's log rotation, backups, health checks, or cleanup scripts, **scheduled automation removes the human bottleneck.**

---

## What I Did

### Opening the Crontab

```bash
# Edit crontab for current user
crontab -e

# List existing cron jobs
crontab -l

# Edit crontab for a specific user (as root)
sudo crontab -u username -e
```

### The Cron Syntax

```
* * * * * /path/to/command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sun=0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Real Examples I Created

```bash
# Run a disk check every day at 8 AM
0 8 * * * df -h >> /var/log/disk_usage.log 2>&1

# Clear temp files every Sunday at midnight
0 0 * * 0 find /tmp -type f -mtime +7 -delete

# Backup a config file every 6 hours
0 */6 * * * cp /etc/nginx/nginx.conf /backup/nginx.conf.$(date +\%Y\%m\%d\%H)

# Run a health check every 5 minutes
*/5 * * * * /home/user/scripts/health_check.sh >> /var/log/health.log 2>&1
```

---

## The Script I Automated

```bash
#!/bin/bash
# system_report.sh — Daily system health snapshot

DATE=$(date '+%Y-%m-%d %H:%M:%S')
LOGFILE="/var/log/system_report.log"

echo "=============================" >> $LOGFILE
echo "Report generated: $DATE" >> $LOGFILE
echo "Uptime: $(uptime -p)" >> $LOGFILE
echo "Disk Usage:" >> $LOGFILE
df -h / >> $LOGFILE
echo "Memory:" >> $LOGFILE
free -h >> $LOGFILE
echo "=============================" >> $LOGFILE
```

```bash
# Make it executable
chmod +x /home/user/scripts/system_report.sh

# Schedule it — every day at 7 AM
0 7 * * * /home/user/scripts/system_report.sh
```

---

## What I Learned

- Why environment variables in cron are limited — `$PATH` in cron is NOT the same as your interactive shell. Always use **absolute paths** in cron scripts.
- The `2>&1` redirect — separating stdout and stderr in logs
- Using `MAILTO=""` to suppress email output from cron
- The `/etc/cron.d/`, `/etc/cron.daily/`, `/etc/cron.hourly/` directories — system-level alternatives to user crontab
- How to verify cron is running: `systemctl status crond` or `systemctl status cron`

---

## Gotcha I Hit

```bash
# This FAILS in cron (PATH doesn't include /usr/local/bin)
* * * * * python3 myscript.py

# This WORKS
* * * * * /usr/bin/python3 /home/user/myscript.py

# Pro tip: find the full path of any command
which python3
# /usr/bin/python3
```

---

## Cron Quick Reference

| Expression | Meaning |
|------------|---------|
| `0 * * * *` | Every hour |
| `0 0 * * *` | Every day at midnight |
| `*/15 * * * *` | Every 15 minutes |
| `0 9 * * 1-5` | Weekdays at 9 AM |
| `0 0 1 * *` | First day of every month |

---

## Key Takeaway

> If you're doing the same task manually more than once a week, it should be a cron job. **Repetition is a human problem; automation is the DevOps solution.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `crontab`, `crond`, `cron`, `/etc/cron.d/`
- **Skill Area:** Task Automation, Linux Scheduling, Shell Scripting

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
