# Day 17 — PostgreSQL User & Database Provisioning 🐘

100 Days of DevOps | Challenge #17

## What's This About?

Modern applications rarely work without databases. Before deploying an application, infrastructure teams must prepare database environments securely and correctly.

In this challenge, I configured a PostgreSQL database server by:

- Creating a dedicated database user
- Creating a new application database
- Granting proper access permissions

This is a very common real-world DevOps and infrastructure administration task during application onboarding.

---

# The Problem

The Nautilus application development team planned to deploy a new application using PostgreSQL on Nautilus infrastructure.

As a prerequisite, the PostgreSQL server needed to be configured with:

- A dedicated database user
- A dedicated application database
- Proper permissions between them

Important constraint:

> Do not restart PostgreSQL service.

---

# What I Did

## Step 1 — Switched to postgres system user

```bash
sudo su - postgres
```

This switched from the normal Linux user to the PostgreSQL administrative user.

---

## Step 2 — Opened PostgreSQL shell

```bash
psql
```

This opened the PostgreSQL interactive terminal.

---

## Step 3 — Created database user

```sql
CREATE USER kodekloud_roy WITH PASSWORD 'TmPcZjtRQx';
```

### Output

```text
CREATE ROLE
```

This created a PostgreSQL database user named:

```text
kodekloud_roy
```

with the required password.

---

## Step 4 — Created application database

```sql
CREATE DATABASE kodekloud_db6;
```

### Output

```text
CREATE DATABASE
```

This created a dedicated database for the application.

---

## Step 5 — Granted permissions

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_roy;
```

### Output

```text
GRANT
```

This granted full access permissions on the database to the application user.

---

# Verification Commands

## List PostgreSQL users

```sql
\du
```

## List databases

```sql
\l
```

## Exit PostgreSQL shell

```sql
\q
```

---

# Breakdown

| Command | Purpose |
|---|---|
| sudo su - postgres | Switch to PostgreSQL admin user |
| psql | Open PostgreSQL interactive shell |
| CREATE USER | Create database login user |
| CREATE DATABASE | Create application database |
| GRANT ALL PRIVILEGES | Assign database permissions |
| \du | View PostgreSQL users |
| \l | View databases |
| \q | Exit PostgreSQL shell |

---

# What I Learned

- Difference between Linux users and PostgreSQL database users
- How PostgreSQL authentication works
- How applications use dedicated database accounts
- Importance of least privilege access
- Basic PostgreSQL administration workflow
- PostgreSQL interactive shell (`psql`) commands
- Why database services should not be restarted unnecessarily in production

---

# Real-World Use Case

This is standard practice when deploying:

- Web applications
- Backend APIs
- SaaS platforms
- ERP systems
- Internal enterprise tools

Each application typically gets:

- its own database
- its own database user
- isolated permissions

This improves:

- Security
- Access control
- Auditing
- Operational safety

---

# Key Takeaway

Database provisioning is one of the foundational responsibilities in DevOps and infrastructure operations.

Properly separating database users and permissions ensures applications remain secure, maintainable, and production-ready.

Even a small task like creating a database user reflects larger production concepts around security, isolation, and controlled access.

---

# Environment

| Component | Details |
|---|---|
| OS | Linux |
| Database | PostgreSQL |
| Tool Used | psql |
| Skill Area | Database Administration, PostgreSQL, Access Control, Linux Administration |

---

Part of my #100DaysOfDevOps journey 🚀
