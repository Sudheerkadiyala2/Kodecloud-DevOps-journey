# Day 2 — Temporary User Setup with Expiry ⏳

> **100 Days of DevOps** | Challenge #2 | April 27, 2026

---

## What's This About?

Contractors. Interns. Temporary vendors. Real-world systems deal with users who only need access for a limited time — and forgetting to remove them later is one of the most common (and embarrassing) security oversights in IT.

On Day 2, I explored how to create **time-limited Linux user accounts** that automatically expire — no manual cleanup required.

---

## The Problem

Imagine you're onboarding a contractor for a 2-week sprint. You create their account, they do their work, and then... you forget to remove it. Three months later, that dormant account is a door left unlocked.

**The fix?** Set an expiry date at account creation. Let the system enforce it for you.

---

## What I Did

```bash
# Create a user with an account expiry date
sudo useradd -e 2026-05-15 tempuser

# Set a password
sudo passwd tempuser

# Verify the expiry date
sudo chage -l tempuser
```

### Modifying an Existing User's Expiry

```bash
# Update expiry on an existing account
sudo usermod --expiredate 2026-05-15 tempuser

# Or use chage interactively
sudo chage tempuser

# Check what the account looks like in /etc/shadow
sudo grep tempuser /etc/shadow
```

### Key `chage` Flags

| Flag | Purpose |
|------|---------|
| `-l` | List account aging details |
| `-E` | Set account expiry date (YYYY-MM-DD) |
| `-M` | Maximum days before password change required |
| `-I` | Days inactive before account locks |

---

## What I Learned

- The difference between **account expiry** (`-e`) and **password expiry** (`-M`) — they're not the same thing
- How `/etc/shadow` stores password aging info (and how to read it)
- Using `chage` as a cleaner interface over raw `/etc/shadow` edits
- How to simulate expiry locally to test behavior

---

## Gotcha I Hit

When testing, I noticed the account expiry works at **login time** — the user's existing sessions don't get killed immediately when the date passes. For that, you'd pair this with a cron job or a session management tool.

```bash
# Force-expire a user immediately (useful for offboarding)
sudo usermod --expiredate 1 username
# Setting date to "1" (Jan 2, 1970) locks them out instantly
```

---

## Real-World Scenario

```bash
# Onboarding a contractor for 30 days
EXPIRY=$(date -d "+30 days" +%Y-%m-%d)
sudo useradd -e $EXPIRY -c "Contractor - John Doe" jdoe_contractor
echo "Account expires: $EXPIRY"
```

---

## Key Takeaway

> Access that expires on its own is infinitely better than access you have to remember to revoke. **Automate the cleanup before it becomes a problem.**

---

## Environment

- **OS:** Linux (RHEL/CentOS/Ubuntu compatible)
- **Tools:** `useradd`, `usermod`, `chage`, `/etc/shadow`
- **Skill Area:** Linux Administration, Identity Management, Security

---

*Part of my [#100DaysOfDevOps](https://github.com) journey. Follow along as I go from fundamentals to full-stack infrastructure!*
