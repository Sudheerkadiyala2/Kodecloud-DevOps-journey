# 19 — Hosting Multiple Static Websites on Apache HTTP Server 🌐

100 Days of DevOps | Challenge #19

## What's This About?

In real production environments, a single web server often hosts multiple websites or applications simultaneously.

In this challenge, I configured Apache HTTP Server to host two separate static websites on a custom port using Linux and Apache configuration fundamentals.

This task covered:

- Apache installation
- Port configuration
- Static website hosting
- File transfers between servers
- Linux permissions
- Web server validation

---

# The Problem

xFusionCorp Industries planned to host two static websites on their infrastructure in Stratos Datacenter.

The websites were still under development, but infrastructure preparation needed to be completed beforehand.

Requirements:

- Install Apache (`httpd`)
- Configure Apache to run on port `5003`
- Deploy two static websites:
  - `/beta`
  - `/games`
- Ensure both websites are accessible through:
  - `http://localhost:5003/beta/`
  - `http://localhost:5003/games/`

---

# What I Did

## Step 1 — Installed Apache HTTP Server

```bash
sudo yum install -y httpd
```

Started and enabled Apache service:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

## Step 2 — Changed Apache listening port

Opened Apache configuration:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Modified:

```apache
Listen 80
```

to:

```apache
Listen 5003
```

Restarted Apache:

```bash
sudo systemctl restart httpd
```

Verified configured port:

```bash
sudo grep -i listen /etc/httpd/conf/httpd.conf
```

Output:

```text
Listen 5003
```

---

# Why This Was Important

The task required Apache to serve content specifically on:

```text
Port 5003
```

instead of the default HTTP port `80`.

---

## Step 3 — Copied website backups from jump host

Copied website directories securely using `scp`.

Initially direct copy failed due to permissions on `/var/www/html`.

So I copied them to `/tmp` first.

### Copy beta website

```bash
scp -r thor@jump_host:/home/thor/beta /tmp/
```

### Copy games website

```bash
scp -r thor@jump_host:/home/thor/games /tmp/
```

Moved them into Apache web root:

```bash
sudo mv /tmp/beta /var/www/html/
sudo mv /tmp/games /var/www/html/
```

---

# What `scp -r` Means

```bash
scp -r
```

- `scp` → Secure Copy over SSH
- `-r` → Recursive copy for directories

Used to securely transfer files/folders between Linux servers.

---

## Step 4 — Configured permissions

Changed ownership:

```bash
sudo chown -R apache:apache /var/www/html/beta /var/www/html/games
```

Changed permissions:

```bash
sudo chmod -R 755 /var/www/html/beta /var/www/html/games
```

---

# Why Permissions Matter

Apache runs using the:

```text
apache
```

system user.

Without proper:
- ownership
- read permissions
- execute permissions

the websites would fail to load.

---

# Understanding 755 Permissions

| User Type | Permissions |
|---|---|
| Owner | rwx |
| Group | r-x |
| Others | r-x |

This allows:
- Apache to read/access website files
- only owner to modify content

A very common production web server permission setup.

---

## Step 5 — Verified website deployment

Tested beta website:

```bash
curl http://localhost:5003/beta/
```

Output:

```html
<h1>KodeKloud</h1>
<p>This is a sample page for our beta website</p>
```

Tested games website:

```bash
curl http://localhost:5003/games/
```

Output:

```html
<h1>KodeKloud</h1>
<p>This is a sample page for our games website</p>
```

Successfully confirmed both websites were hosted correctly.

---

# Breakdown

| Command | Purpose |
|---|---|
| yum install httpd | Install Apache |
| systemctl start httpd | Start Apache service |
| Listen 5003 | Configure custom Apache port |
| scp -r | Securely copy website files |
| chown | Change ownership |
| chmod 755 | Set website permissions |
| curl | Validate website accessibility |

---

# What I Learned

- Apache HTTP Server installation and setup
- How Apache serves static content
- Custom web server port configuration
- Secure file transfer using `scp`
- Linux ownership and permission management
- How Apache maps URLs to filesystem directories
- Importance of proper web server permissions
- Website deployment validation using `curl`

---

# Architecture

```text
User
   |
   v
Apache Server :5003
   |
   +--> /var/www/html/beta
   |
   +--> /var/www/html/games
```

---

# Real-World Use Case

This setup pattern is used in:

- Internal company portals
- Static documentation hosting
- Development/staging environments
- Multi-site hosting
- Enterprise intranet systems

Apache remains one of the most widely used web servers in enterprise Linux environments.

---

# Key Takeaway

Hosting websites is not just about copying files into a folder.

A production-ready setup requires:

- correct server configuration
- proper networking
- secure file transfers
- ownership management
- permission handling
- validation and troubleshooting

This challenge strengthened my understanding of how Linux permissions and Apache configuration work together in real infrastructure environments.

---

# Environment

| Component | Details |
|---|---|
| OS | Linux |
| Web Server | Apache HTTP Server |
| Port Used | 5003 |
| Websites Hosted | beta, games |
| Tools Used | httpd, scp, curl, chmod, chown |
| Skill Area | Linux Administration, Apache, Static Website Hosting, Permissions Management |

---

Part of my #100DaysOfDevOps journey 🚀
