# Day 68: Create a Public VPC, Public Subnet, and Launch an EC2 Instance

**Date:** 2026-07-20  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS VPC & Amazon EC2

---

## Challenge

The Nautilus DevOps Team was tasked with setting up a new public VPC to host internet-facing applications. The infrastructure required a public subnet with automatic public IP assignment and an EC2 instance that could be accessed over SSH.

### Requirements

- Create a VPC named **nautilus-pub-vpc**.
- Create a subnet named **nautilus-pub-subnet** within the VPC.
- Enable **Auto-assign Public IPv4 Address** for the subnet.
- Create an EC2 instance named **nautilus-pub-ec2**.
- Use instance type **t2.micro**.
- Launch the instance inside the newly created VPC and subnet.
- Configure the Security Group to allow **SSH (Port 22)** access from the Internet.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials.
- Selected the **us-east-1** region.

---

## Step 2: Create the VPC

Navigate to:

```text
VPC → Your VPCs
```

Click **Create VPC** and configure:

| Property | Value |
|----------|-------|
| Name | `nautilus-pub-vpc` |
| IPv4 CIDR Block | `10.0.0.0/16` |
| Tenancy | Default |

Click **Create VPC**.

---

## Step 3: Create the Public Subnet

Navigate to:

```text
VPC → Subnets
```

Click **Create Subnet** and configure:

| Property | Value |
|----------|-------|
| VPC | `nautilus-pub-vpc` |
| Subnet Name | `nautilus-pub-subnet` |
| Availability Zone | Any available AZ |
| IPv4 CIDR Block | `10.0.1.0/24` |

Click **Create Subnet**.

---

## Step 4: Enable Auto-Assign Public IP

Select the subnet:

```text
nautilus-pub-subnet
```

Choose:

```text
Actions → Edit Subnet Settings
```

Enable:

```text
Auto-assign Public IPv4 Address
```

Save the changes.

---

## Step 5: Create an Internet Gateway

Navigate to:

```text
VPC → Internet Gateways
```

- Create a new Internet Gateway.
- Attach it to:

```text
nautilus-pub-vpc
```

---

## Step 6: Configure the Route Table

Navigate to:

```text
VPC → Route Tables
```

- Select the route table associated with **nautilus-pub-vpc**.
- Add the following route:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | Internet Gateway |

Associate the route table with:

```text
nautilus-pub-subnet
```

---

## Step 7: Create the Security Group

Navigate to:

```text
EC2 → Security Groups
```

Create a Security Group with:

| Property | Value |
|----------|-------|
| Name | `nautilus-pub-sg` |
| VPC | `nautilus-pub-vpc` |

### Inbound Rule

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| SSH | TCP | 22 | Anywhere (0.0.0.0/0) |

Leave outbound rules as default.

---

## Step 8: Launch the EC2 Instance

Navigate to:

```text
EC2 → Instances
```

Click **Launch Instance** and configure:

| Property | Value |
|----------|-------|
| Instance Name | `nautilus-pub-ec2` |
| AMI | Ubuntu Server |
| Instance Type | `t2.micro` |
| Network | `nautilus-pub-vpc` |
| Subnet | `nautilus-pub-subnet` |
| Auto-Assign Public IP | Enabled |
| Security Group | `nautilus-pub-sg` |

Click **Launch Instance**.

---

# Steps Performed

```text
AWS Console
      │
      ▼
VPC
      │
      ▼
Create VPC
      │
      ▼
nautilus-pub-vpc
      │
      ▼
Create Subnet
      │
      ▼
nautilus-pub-subnet
      │
      ▼
Enable Auto-Assign Public IP
      │
      ▼
Create Internet Gateway
      │
      ▼
Attach to VPC
      │
      ▼
Configure Route Table
      │
      ▼
0.0.0.0/0 → Internet Gateway
      │
      ▼
Create Security Group
      │
      ▼
Allow SSH (22)
      │
      ▼
Launch EC2
      │
      ▼
nautilus-pub-ec2
```

---

# Verification

### Verify the VPC

```text
VPC
└── Your VPCs
    └── nautilus-pub-vpc
```

Confirm the VPC is created successfully.

---

### Verify the Public Subnet

```text
VPC
└── Subnets
    └── nautilus-pub-subnet
```

Verify:

- Auto-assign Public IPv4 Address: **Enabled**

---

### Verify the Internet Gateway

```text
VPC
└── Internet Gateways
```

Confirm it is attached to **nautilus-pub-vpc**.

---

### Verify the Route Table

Ensure the route table contains:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | Internet Gateway |

---

### Verify the EC2 Instance

```text
EC2
└── Instances
    └── nautilus-pub-ec2
```

Verify:

- State: **Running**
- Instance Type: **t2.micro**
- Public IPv4 Address: Assigned

---

### Verify the Security Group

Confirm the inbound rule:

| Protocol | Port | Source |
|----------|------|--------|
| SSH | 22 | 0.0.0.0/0 |

---

### Test SSH Connectivity

Using the instance's public IP:

```bash
ssh -i <key-pair>.pem ubuntu@<Public-IP>
```

The connection should be established successfully.

---

# Key Concepts Learned

### Amazon VPC

A Virtual Private Cloud (VPC) is an isolated virtual network where AWS resources can be securely deployed.

### Public Subnet

A public subnet is associated with a route table that directs internet-bound traffic through an Internet Gateway and automatically assigns public IP addresses to resources.

### Internet Gateway

An Internet Gateway enables communication between resources inside a VPC and the public Internet.

### Route Table

A Route Table defines how network traffic is directed within a VPC.

### Security Group

A Security Group acts as a virtual firewall that controls inbound and outbound traffic for EC2 instances.

### Amazon EC2

Amazon EC2 provides scalable virtual servers that can be deployed inside a VPC to host applications and services.

---

# Outcome

Successfully created the **nautilus-pub-vpc** VPC, configured the **nautilus-pub-subnet** with automatic public IP assignment, attached an Internet Gateway, configured routing for internet access, created a Security Group allowing SSH access, and launched the **nautilus-pub-ec2** instance inside the public subnet. The instance is accessible from the Internet over **SSH (Port 22)**.
