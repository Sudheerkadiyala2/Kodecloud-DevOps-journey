# KodeKloud AWS DevOps Labs – Task 63

## Configure Passwordless SSH Access to an EC2 Instance

## 📌 Objective

The goal of this task is to create an Ubuntu EC2 instance named **`xfusion-ec2`** with the **`t2.micro`** instance type and configure **passwordless SSH authentication** from the **`aws-client`** host by generating an RSA key pair and adding the public key to the EC2 instance's `authorized_keys` file.

---

## 🛠️ AWS Services Used

- Amazon EC2
- Amazon VPC
- Security Groups
- OpenSSH

---

## 🌍 Region

- **us-east-1 (N. Virginia)**

---

## 📋 EC2 Configuration

| Property | Value |
|----------|-------|
| Instance Name | `xfusion-ec2` |
| AMI | Ubuntu Server |
| Instance Type | `t2.micro` |
| Region | `us-east-1` |
| Public IP | Enabled |

---

## 🔒 Security Group Configuration

### Inbound Rules

| Type | Protocol | Port | Source |
|------|----------|------|--------|
| SSH | TCP | 22 | `0.0.0.0/0` |

---

# 🚀 Implementation Steps

## Step 1: Launch the EC2 Instance

1. Open the AWS Management Console.
2. Select the **us-east-1** region.
3. Launch a new Ubuntu EC2 instance.
4. Configure the instance with:
   - **Name:** `xfusion-ec2`
   - **Instance Type:** `t2.micro`
   - **Public IP:** Enabled
5. Attach a Security Group allowing SSH (Port 22).
6. Launch the instance.

---

## Step 2: Generate an SSH Key Pair on the `aws-client` Host

```bash
mkdir -p /root/.ssh

if [ ! -f /root/.ssh/id_rsa ]; then
    ssh-keygen -t rsa -N "" -f /root/.ssh/id_rsa
fi
```

Verify that the key pair has been created:

```bash
ls -l /root/.ssh
```

Expected output:

```
id_rsa
id_rsa.pub
```

---

## Step 3: Display the Public Key

```bash
cat /root/.ssh/id_rsa.pub
```

Copy the complete output.

---

## Step 4: Add the Public Key to the EC2 Instance

On the EC2 instance, create the `.ssh` directory if it doesn't exist:

```bash
sudo mkdir -p /root/.ssh
```

Append the copied public key to the `authorized_keys` file:

```bash
echo "<PASTE_PUBLIC_KEY_HERE>" | sudo tee -a /root/.ssh/authorized_keys
```

Set the appropriate permissions:

```bash
sudo chmod 700 /root/.ssh
sudo chmod 600 /root/.ssh/authorized_keys
```

---

## Step 5: Verify Passwordless SSH Authentication

From the `aws-client` host:

```bash
ssh root@<EC2_PUBLIC_IP>
```

If the login succeeds without prompting for a password, passwordless SSH authentication has been configured successfully.

---

# 📂 Project Structure

```
Task-63/
└── README.md
```

---

# ✅ Outcome

- Successfully launched an Ubuntu EC2 instance.
- Configured the instance with the name **`xfusion-ec2`**.
- Generated an RSA SSH key pair on the `aws-client` host.
- Added the public key to the EC2 instance.
- Enabled secure passwordless SSH authentication.
- Verified SSH connectivity from the `aws-client` host.

---

# 📚 Key Learnings

- Launching Amazon EC2 instances
- Configuring Security Groups
- SSH key generation using OpenSSH
- Passwordless SSH authentication
- Managing Linux file permissions
- Secure remote administration of EC2 instances

---

## 🏷️ Lab Information

| Item | Value |
|------|-------|
| Platform | KodeKloud AWS DevOps Labs |
| Task Number | 63 |
| Cloud Provider | AWS |
| Service | Amazon EC2 |
| Difficulty | Beginner |

---

### ⭐ If you found this repository helpful, consider giving it a star!
