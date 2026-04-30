# Day 9 — MariaDB Troubleshooting 🔧

> **100 Days of DevOps** | Challenge #9 | April 30, 2026

---

## What's This About?

Databases don't just break on schedule. They break at 2 AM, on a Friday, right before a release. Day 9 was about developing the troubleshooting instincts to **diagnose database issues fast** — reading logs, checking service state, and knowing where to look when MariaDB refuses to behave.

This isn't a tutorial on how to *use* MariaDB. It's about what you do when it's **broken**.

---

## The Problem

MariaDB (and MySQL) issues come in a few flavors:
- The service won't start
- It starts but refuses connections
- Performance degradation / queries hanging
- Authentication failures
- Disk space / InnoDB corruption

The challenge was to simulate, diagnose, and fix common MariaDB failures.

---

## Diagnostic Toolkit — Always Start Here

```bash
# 1. Is the service running?
sudo systemctl status mariadb

# 2. What do the logs say?
sudo journalctl -u mariadb -n 50 --no-pager
sudo tail -100 /var/log/mariadb/mariadb.log

# 3. Can you connect at all?
mysql -u root -p
mysql -u root -p -e "SELECT 1;"

# 4. What port is it listening on?
ss -tlnp | grep 3306
netstat -tlnp | grep mysql
```

---

## Common Failures I Investigated

### 🔴 Issue 1 — Service Fails to Start

```bash
# Check the error
sudo systemctl status mariadb
# Look for: "Job for mariadb.service failed"

# Read the full journal
sudo journalctl -xe | grep mariadb

# Common culprit: corrupted InnoDB files or wrong permissions
ls -la /var/lib/mysql/
sudo chown -R mysql:mysql /var/lib/mysql/

# Try starting with verbose output
sudo mysqld_safe --skip-grant-tables &
```

### 🔴 Issue 2 — Access Denied Errors

```bash
# "Access denied for user 'root'@'localhost'"
# Fix: reset root password without old password

sudo systemctl stop mariadb

# Start in safe mode (skips grant tables)
sudo mysqld_safe --skip-grant-tables --skip-networking &

# Connect without password
mysql -u root

# Reset the password
USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewSecurePassword!';
FLUSH PRIVILEGES;
EXIT;

# Stop safe mode, restart normally
sudo kill $(pidof mysqld)
sudo systemctl start mariadb
```

### 🔴 Issue 3 — Disk Space / InnoDB Issues

```bash
# Check if disk is full
df -h /var/lib/mysql

# Check InnoDB status for long-running transactions
mysql -u root -p -e "SHOW ENGINE INNODB STATUS\G"

# Check for locked tables
mysql -u root -p -e "SHOW PROCESSLIST;"

# Kill a stuck query (replace <ID> with process id)
mysql -u root -p -e "KILL <ID>;"
```

### 🔴 Issue 4 — Can't Connect Remotely

```bash
# MariaDB might be bound to 127.0.0.1 only
grep bind-address /etc/mysql/mariadb.conf.d/50-server.cnf
# or
grep bind-address /etc/my.cnf

# Change to allow remote connections
# bind-address = 0.0.0.0

# Also check firewall
sudo firewall-cmd --list-ports | grep 3306
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload

# And check user host permissions
mysql -u root -p -e "SELECT user, host FROM mysql.user;"
```

---

## My Diagnostic Checklist

```
[ ] systemctl status mariadb
[ ] journalctl -u mariadb (read the actual error, don't skip this)
[ ] Check /var/log/mariadb/mariadb.log
[ ] Check disk space: df -h
[ ] Check permissions: ls -la /var/lib/mysql/
[ ] Try connecting locally vs remotely
[ ] Check bind-address in config
[ ] Check firewall rules
[ ] Check SELinux denials: ausearch -m avc -ts recent
[ ] Run: mysqlcheck -u root -p --all-databases
```

---

## What I Learned

- **Always read the logs first** — MariaDB's error messages are actually pretty informative once you know where to look
- The `--skip-grant-tables` trick for password recovery (and why it should only be used with `--skip-networking`)
- How `SHOW PROCESSLIST` and `SHOW ENGINE INNODB STATUS` are your best friends for runtime issues
- SELinux can silently block MariaDB from accessing its data directory — always check `ausearch` when permissions look right but things still fail
- `mysqlcheck` for detecting and repairing corrupted tables

---

## Gotcha I Hit

```bash
# After changing bind-address, the service started but I still couldn't connect remotely.
# Root cause: the MySQL user was created with 'root'@'localhost' — localhost only!

# Fix: create a user that allows connections from anywhere (or specific IPs)
mysql -u root -p -e "CREATE USER 'admin'@'%' IDENTIFIED BY 'password';"
mysql -u root -p -e "GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%';"
mysql -u root -p -e "FLUSH PRIVILEGES;"
```

---

## Key Takeaway

> Troubleshooting isn't about knowing every answer — it's about **having a systematic process**. Check the service. Read the logs. Isolate the layer (network? auth? disk? config?). Fix and verify. **Panic is what happens when you skip the process.**

---

## Environment

- **OS:** RHEL / CentOS / Ubuntu
- **Tools:** `systemctl`, `journalctl`, `mysql`, `mysqld_safe`, `mysqlcheck`, `ss`, `firewall-cmd`
- **Skill Area:** Database Administration, Troubleshooting, MariaDB/MySQL

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
