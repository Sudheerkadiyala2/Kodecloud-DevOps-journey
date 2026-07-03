# Task 62 — Launching an EC2 Instance with an Elastic IP on AWS ☁️
**100 Days of DevOps | Challenge #62**

## What's This About?

Cloud applications often require a **static public IP address** so they remain accessible even after instance restarts. Today, I provisioned an Amazon EC2 instance and associated an **Elastic IP (EIP)** to provide a permanent public endpoint.

This task demonstrated one of the fundamental AWS networking concepts used in production environments.

---

## The Problem

The **Nautilus DevOps Team** needed to provision a new server for the Development Team.

### Requirements

- Create an EC2 instance named **devops-ec2**
- Use a Linux AMI (Ubuntu)
- Instance type: **t2.micro**
- Allocate an Elastic IP
- Associate the Elastic IP with the EC2 instance
- Name the Elastic IP **devops-eip**
- Create all resources in the **us-east-1** region

---

## What I Did

### 1. Logged into AWS Console

- Accessed the AWS Management Console using the provided lab credentials.
- Selected the **us-east-1 (N. Virginia)** region.

### 2. Created an EC2 Instance

Configured the instance with:

| Property | Value |
|----------|-------|
| Name | `devops-ec2` |
| AMI | Ubuntu Linux |
| Instance Type | `t2.micro` |
| Region | `us-east-1` |

Launched the instance and waited until it reached the **Running** state.

---

### 3. Allocated an Elastic IP

From the **EC2 Dashboard**:

- Navigated to **Network & Security → Elastic IPs**
- Clicked **Allocate Elastic IP address**
- Used the default allocation settings
- Allocated the new Elastic IP

---

### 4. Associated the Elastic IP

- Selected the allocated Elastic IP
- Clicked **Associate Elastic IP**
- Chose the EC2 instance **devops-ec2**
- Completed the association

---

### 5. Tagged the Elastic IP

Added the following tag:

| Key | Value |
|-----|-------|
| Name | `devops-eip` |

---

## Breakdown

| AWS Service | Purpose |
|-------------|---------|
| Amazon EC2 | Virtual server for hosting applications |
| Elastic IP | Provides a static public IPv4 address |
| Resource Tags | Simplify identification and management |

---

## What I Learned

- How to launch an EC2 instance using the AWS Management Console.
- The difference between dynamic public IPs and Elastic IPs.
- How Elastic IPs provide a persistent public address.
- The importance of tagging AWS resources for easier management.
- Why region selection matters when provisioning cloud resources.

---

## Real-World Use Case

Elastic IPs are commonly used for:

- Web servers
- Bastion hosts
- Production applications
- APIs requiring fixed IP whitelisting
- DNS records pointing to stable infrastructure

Without an Elastic IP, restarting an EC2 instance can result in a new public IP address, potentially disrupting applications and client access.

---

## Key Takeaway

An Elastic IP ensures that applications remain reachable through a consistent public IP address, making it an essential networking feature for many production workloads hosted on Amazon EC2.

---

## Environment

**Platform:** KodeKloud Engineer

**Cloud Provider:** AWS

**Region:** us-east-1 (N. Virginia)

**Services Used:**

- Amazon EC2
- Elastic IP

**Skill Area:** AWS Compute, Networking, Cloud Infrastructure

---

Part of my **#100DaysOfDevOps** journey—building practical cloud and DevOps skills through hands-on challenges every day.

**#100DaysOfDevOps #AWS #AmazonEC2 #ElasticIP #CloudComputing #DevOps #CloudInfrastructure #KodeKloud #Networking #AWSCloud**
