# Day 57: Create an AWS IAM Policy for Read-Only EC2 Access

**Date:** 2026-07-02  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS IAM

---

## Challenge

The Nautilus DevOps team needed to configure an AWS Identity and Access Management (IAM) policy that provides read-only access to Amazon EC2 resources.

### Requirements

- Create an IAM policy named **iampolicy_james**.
- Configure the policy in the **us-east-1** region.
- Allow users to:
  - View EC2 Instances
  - View Amazon Machine Images (AMIs)
  - View EBS Snapshots

---

# Solution

## Step 1: Log in to the AWS Console

- Sign in using the temporary AWS credentials provided in the lab.
- Select the **us-east-1** region.

---

## Step 2: Open IAM

- Navigate to **IAM**.
- Click **Policies** from the left navigation pane.
- Click **Create policy**.

---

## Step 3: Create the Policy

- Choose **Visual editor**.
- Select **EC2** as the service.
- Under **Read** access level, select:
  - DescribeInstances
  - DescribeImages
  - DescribeSnapshots
- Keep **Resources** as **All resources**.
- Click **Next**.

---

## Step 4: Review and Create

Enter the following details:

| Field | Value |
|-------|-------|
| Policy Name | `iampolicy_james` |

Review the policy and click **Create policy**.

---

# Steps Performed

```text
AWS Console
      │
      ▼
IAM
      │
      ▼
Policies
      │
      ▼
Create Policy
      │
      ▼
Visual Editor
      │
      ▼
Service → EC2
      │
      ▼
Permissions
├── DescribeInstances
├── DescribeImages
└── DescribeSnapshots
      │
      ▼
Policy Name
iampolicy_james
      │
      ▼
Create Policy
