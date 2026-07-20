# Day 69: Configure a NAT Instance for Internet Access from a Private Subnet

**Date:** 2026-07-20  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS VPC & Amazon EC2

---

## Challenge

The Nautilus DevOps Team needed to provide internet access to an EC2 instance running in a private subnet using a **NAT Instance** instead of a NAT Gateway. The private instance was already configured with a cron job that uploads a test file to an Amazon S3 bucket every minute. The task was considered successful once the file appeared in the S3 bucket.

### Requirements

- Create a public subnet named **nautilus-pub-subnet** in the existing VPC.
- Launch a NAT Instance named **nautilus-nat-instance** using **Amazon Linux 2023**.
- Configure the NAT Instance to provide internet access for resources in the private subnet.
- Create and attach a custom Security Group to the NAT Instance.
- Disable **Source/Destination Check** on the NAT Instance.
- Configure routing so that the private subnet uses the NAT Instance for internet access.
- Verify that the file **nautilus-test.txt** is successfully uploaded to the S3 bucket **nautilus-nat-22534**.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials.
- Selected the **us-east-1** region.

---

## Step 2: Create a Public Subnet

Navigate to:

```text
VPC → Subnets
```

Create a subnet with the following details:

| Property | Value |
|----------|-------|
| Subnet Name | `nautilus-pub-subnet` |
| VPC | `nautilus-priv-vpc` |
| IPv4 CIDR | Available subnet within the VPC |

Enable:

```text
Auto-assign Public IPv4 Address
```

---

## Step 3: Configure Internet Access

- Create and attach an **Internet Gateway** to the VPC (if not already attached).
- Update the public subnet route table:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | Internet Gateway |

---

## Step 4: Launch the NAT Instance

Navigate to:

```text
EC2 → Instances
```

Launch an EC2 instance with the following configuration:

| Property | Value |
|----------|-------|
| Instance Name | `nautilus-nat-instance` |
| AMI | Amazon Linux 2023 |
| Instance Type | t2.micro |
| VPC | `nautilus-priv-vpc` |
| Subnet | `nautilus-pub-subnet` |
| Auto-Assign Public IP | Enabled |
| Security Group | Custom Security Group |

---

## Step 5: Configure the Security Group

Inbound Rules:

| Type | Source |
|------|--------|
| SSH (22) | Anywhere (0.0.0.0/0) |
| All Traffic | VPC CIDR |

Outbound Rules:

| Type | Destination |
|------|-------------|
| All Traffic | 0.0.0.0/0 |

---

## Step 6: Disable Source/Destination Check

Navigate to:

```text
EC2 → nautilus-nat-instance
```

Choose:

```text
Actions
→ Networking
→ Change Source/Destination Check
```

Set it to:

```text
Disabled
```

---

## Step 7: Configure the NAT Instance

Connect to the NAT Instance using SSH.

Update the system:

```bash
sudo dnf update -y
```

Install iptables:

```bash
sudo dnf install iptables-services -y
```

Enable IPv4 forwarding:

```bash
sudo sysctl -w net.ipv4.ip_forward=1

echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p
```

Configure NAT using the correct network interface (`ens5`):

```bash
sudo iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE

sudo iptables -A FORWARD -i ens5 -o ens5 -m state --state RELATED,ESTABLISHED -j ACCEPT

sudo iptables -A FORWARD -i ens5 -o ens5 -j ACCEPT
```

Save the firewall rules:

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

Enable and start the iptables service:

```bash
sudo systemctl enable iptables

sudo systemctl restart iptables
```

---

## Step 8: Update the Private Route Table

Navigate to:

```text
VPC → Route Tables
```

Update the route table associated with **nautilus-priv-subnet**:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | `nautilus-nat-instance` |

---

# Steps Performed

```text
AWS Console
      │
      ▼
Create Public Subnet
      │
      ▼
Enable Auto Assign Public IP
      │
      ▼
Launch NAT Instance
      │
      ▼
Amazon Linux 2023
      │
      ▼
Create Custom Security Group
      │
      ▼
Disable Source/Destination Check
      │
      ▼
Configure iptables & IP Forwarding
      │
      ▼
Update Private Route Table
      │
      ▼
Private EC2
      │
      ▼
Internet Access via NAT Instance
      │
      ▼
Upload File to Amazon S3
```

---

# Verification

### Verify NAT Instance

```text
EC2
└── Instances
    └── nautilus-nat-instance
```

Verify:

- Running
- Public IPv4 Address Assigned
- Source/Destination Check Disabled

---

### Verify Route Tables

Private Route Table:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | NAT Instance |

Public Route Table:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | Internet Gateway |

---

### Verify NAT Configuration

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Expected Output:

```text
1
```

Verify the NAT rule:

```bash
sudo iptables -t nat -L -n -v
```

Expected Output:

```text
Chain POSTROUTING
MASQUERADE
```

---

### Verify S3 Upload

Navigate to:

```text
Amazon S3
└── nautilus-nat-22534
```

Verify the object:

```text
nautilus-test.txt
```

Its presence confirms that the private EC2 instance successfully accessed the internet through the NAT Instance.

---

# Key Concepts Learned

### NAT Instance

A NAT Instance enables EC2 instances in private subnets to access the internet while preventing inbound internet connections.

### IP Forwarding

IP forwarding allows the EC2 instance to forward packets between network interfaces, enabling it to function as a router.

### iptables MASQUERADE

The `MASQUERADE` rule performs source network address translation (SNAT), allowing private IP addresses to communicate with external networks using the NAT Instance's public IP address.

### Source/Destination Check

Source/Destination Check must be disabled on a NAT Instance so that it can forward traffic for other instances.

### Route Tables

Private subnet traffic destined for the internet must be routed through the NAT Instance, while the public subnet routes internet-bound traffic through the Internet Gateway.

---

# Outcome

Successfully configured a **NAT Instance** using **Amazon Linux 2023** to provide internet access for resources in the private subnet. After configuring IP forwarding, `iptables` NAT rules, disabling Source/Destination Check, and updating the route tables, the private EC2 instance successfully uploaded **nautilus-test.txt** to the **nautilus-nat-22534** Amazon S3 bucket, confirming successful internet connectivity through the NAT Instance.
