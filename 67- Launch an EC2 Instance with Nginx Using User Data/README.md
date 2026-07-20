# Task 67: Launch an EC2 Instance with Nginx Using User Data
  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS EC2

---

## Challenge

The Nautilus DevOps Team needed to provision an EC2 instance that would act as a web server for a critical application. The instance should automatically install and start the Nginx web server during launch and be accessible over HTTP.

### Requirements

- Launch an EC2 instance named **devops-ec2**.
- Use any available **Ubuntu AMI**.
- Configure a **User Data** script to:
  - Install the **Nginx** package.
  - Start the **Nginx** service.
- Configure the Security Group to allow **HTTP (Port 80)** traffic from the Internet.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials.
- Selected the **us-east-1** region.

---

## Step 2: Launch the EC2 Instance

Navigate to:

```text
EC2 → Instances
```

Click **Launch Instance** and configure the following:

| Property | Value |
|----------|-------|
| Instance Name | `devops-ec2` |
| AMI | Ubuntu Server |
| Instance Type | t2.micro (or default) |
| Key Pair | Default / None (Lab Environment) |
| Network | Default VPC |

---

## Step 3: Configure the Security Group

Create or select a Security Group with the following inbound rule:

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| HTTP | TCP | 80 | Anywhere (0.0.0.0/0) |

Leave outbound rules as default.

---

## Step 4: Add the User Data Script

Expand **Advanced Details** and paste the following script into the **User Data** section:

```bash
#!/bin/bash

apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

Continue with the remaining default settings.

Click **Launch Instance**.

---

# Steps Performed

```text
AWS Console
      │
      ▼
EC2
      │
      ▼
Launch Instance
      │
      ▼
Ubuntu Server AMI
      │
      ▼
Instance Name
devops-ec2
      │
      ▼
Configure Security Group
      │
      ▼
Allow HTTP (80)
      │
      ▼
Advanced Details
      │
      ▼
User Data Script
      │
      ▼
Install & Start Nginx
      │
      ▼
Launch Instance
```

---

# Verification

### Verify the EC2 Instance

Navigate to:

```text
EC2
└── Instances
```

Verify:

- Instance Name: `devops-ec2`
- Instance State: **Running**

---

### Verify the Security Group

Confirm the inbound rule:

| Protocol | Port | Source |
|----------|------|--------|
| HTTP | 80 | 0.0.0.0/0 |

---

### Verify the User Data Execution

Open the EC2 instance details and verify that the User Data script has been configured.

---

### Verify Nginx

Copy the **Public IPv4 Address** of the EC2 instance and open it in a web browser:

```text
http://<Public-IP>
```

Expected Output:

```text
Welcome to nginx!
```

or the default Nginx landing page.

---

# Key Concepts Learned

### Amazon EC2

Amazon EC2 provides scalable virtual servers that can host web applications and services.

### User Data

User Data is a startup script that automatically executes when an EC2 instance is launched, enabling automated software installation and configuration.

### Nginx

Nginx is a high-performance web server commonly used for serving websites, acting as a reverse proxy, and load balancing.

### Security Group

A Security Group acts as a virtual firewall that controls inbound and outbound traffic to an EC2 instance.

---

# Outcome

Successfully launched an Ubuntu EC2 instance named **devops-ec2**, configured a **User Data** script to automatically install and start the **Nginx** web server during instance initialization, and configured the Security Group to allow inbound **HTTP (Port 80)** traffic from the Internet. The instance is accessible through its public IP address and serves the default Nginx web page.
