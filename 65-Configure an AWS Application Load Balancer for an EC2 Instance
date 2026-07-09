# Day 65: Configure an AWS Application Load Balancer for an EC2 Instance

**Date:** 2026-07-09  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS Elastic Load Balancing (ALB)

---

## Challenge

The Nautilus DevOps team needed to deploy an **Application Load Balancer (ALB)** in front of an existing EC2 instance running an Nginx web server.

### Requirements

- Create an **Application Load Balancer** named **nautilus-alb**.
- Create a **Target Group** named **nautilus-tg**.
- Create a **Security Group** named **nautilus-sg**.
- Allow inbound **HTTP (Port 80)** traffic from the Internet.
- Attach the security group to the ALB.
- Register the **nautilus-ec2** instance in the target group.
- Configure the ALB listener to forward HTTP traffic (Port 80) to the target group.
- Update the EC2 instance security group, if required, to allow traffic from the ALB.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials.
- Selected the **us-east-1** region.

---

## Step 2: Create a Security Group

Navigate to:

```text
VPC → Security Groups
```

Create a security group with the following details:

| Property | Value |
|----------|-------|
| Security Group Name | `nautilus-sg` |
| Description | Security group for Application Load Balancer |
| VPC | Default VPC |

### Inbound Rule

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| HTTP | TCP | 80 | Anywhere (0.0.0.0/0) |

Leave the outbound rules as default.

---

## Step 3: Create the Target Group

Navigate to:

```text
EC2 → Target Groups
```

Create a target group with:

| Property | Value |
|----------|-------|
| Target Type | Instances |
| Target Group Name | `nautilus-tg` |
| Protocol | HTTP |
| Port | 80 |
| VPC | Default VPC |
| Health Check Protocol | HTTP |

Register the instance:

```text
nautilus-ec2
```

Click **Create Target Group**.

---

## Step 4: Create the Application Load Balancer

Navigate to:

```text
EC2 → Load Balancers
```

Click **Create Load Balancer**.

Select:

```text
Application Load Balancer
```

Configure:

| Property | Value |
|----------|-------|
| Load Balancer Name | `nautilus-alb` |
| Scheme | Internet-facing |
| IP Address Type | IPv4 |
| Listener | HTTP : 80 |
| Availability Zones | Default |
| Security Group | `nautilus-sg` |

For the default listener action:

```text
Forward to:
nautilus-tg
```

Click **Create Load Balancer**.

---

## Step 5: Update the EC2 Security Group

Navigate to:

```text
EC2 → Instances
```

Select:

```text
nautilus-ec2
```

Open the attached Security Group.

Add the following inbound rule if it does not already exist:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80 | nautilus-sg |

Save the rule.

---

# Steps Performed

```text
AWS Console
      │
      ▼
VPC
      │
      ▼
Security Groups
      │
      ▼
Create Security Group
      │
      ▼
nautilus-sg
      │
      ▼
Allow HTTP (80)
      │
      ▼
EC2
      │
      ▼
Target Groups
      │
      ▼
Create
nautilus-tg
      │
      ▼
Register
nautilus-ec2
      │
      ▼
Load Balancers
      │
      ▼
Create Application Load Balancer
      │
      ▼
nautilus-alb
      │
      ▼
Attach Security Group
      │
      ▼
Forward Requests
      │
      ▼
nautilus-tg
      │
      ▼
Update EC2 Security Group
```

---

# Verification

Verify the following:

### Application Load Balancer

```text
EC2
└── Load Balancers
    └── nautilus-alb
```

Status:

```text
Active
```

---

### Target Group

```text
EC2
└── Target Groups
    └── nautilus-tg
```

Verify:

- Registered Target:
  - `nautilus-ec2`
- Health Status:
  - **Healthy**

---

### Security Group

Verify:

```text
nautilus-sg
```

Inbound Rules:

| Protocol | Port | Source |
|----------|------|--------|
| HTTP | 80 | 0.0.0.0/0 |

---

### EC2 Security Group

Verify that the EC2 instance allows inbound HTTP traffic from:

```text
nautilus-sg
```

---

### Test the Load Balancer

Open the ALB DNS name in a browser:

```text
http://<ALB-DNS-Name>
```

Expected Output:

```text
Welcome to nginx!
```

or the default Nginx landing page.

---

# Key Concepts Learned

### Application Load Balancer (ALB)

An Application Load Balancer distributes incoming HTTP/HTTPS traffic across multiple targets, improving scalability and high availability.

### Target Group

A Target Group contains one or more backend resources (EC2 instances, IP addresses, or Lambda functions) that receive traffic from the load balancer.

### Security Group

A Security Group acts as a virtual firewall controlling inbound and outbound traffic for AWS resources.

### Health Checks

The ALB continuously checks the health of registered targets and routes traffic only to healthy instances.

---

# Outcome

Successfully configured an **Application Load Balancer** named **nautilus-alb**, created the **nautilus-tg** target group, configured the **nautilus-sg** security group to allow public HTTP access, registered the **nautilus-ec2** instance as the backend target, and verified that HTTP traffic is successfully routed from the ALB to the Nginx server running on the EC2 instance.
