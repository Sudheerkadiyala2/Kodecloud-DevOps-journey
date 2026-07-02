# Task 58: Attach an IAM Policy to an Existing IAM User

**Date:** 2026-07-02  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS IAM

---

## Challenge

The Nautilus DevOps team is configuring AWS Identity and Access Management (IAM) resources as part of their cloud migration.

### Requirements

- An IAM user named **iamuser_mariyam** already exists.
- An IAM policy named **iampolicy_mariyam** already exists.
- Attach the IAM policy **iampolicy_mariyam** to the IAM user **iamuser_mariyam**.

---

# Solution

## Step 1: Log in to the AWS Console

- Sign in using the temporary AWS credentials provided in the lab.
- Select the **us-east-1** region.

---

## Step 2: Open IAM

- Navigate to **IAM**.
- Click **Users** from the left navigation pane.

---

## Step 3: Open the IAM User

- Select the user **iamuser_mariyam**.
- Open the **Permissions** tab.

---

## Step 4: Attach the Existing Policy

- Click **Add permissions**.
- Select **Attach policies directly**.
- Search for **iampolicy_mariyam**.
- Select the policy.
- Click **Next**.
- Review the changes.
- Click **Add permissions**.

---

# Steps Performed

```text
AWS Console
      │
      ▼
IAM
      │
      ▼
Users
      │
      ▼
iamuser_mariyam
      │
      ▼
Permissions
      │
      ▼
Add Permissions
      │
      ▼
Attach Policies Directly
      │
      ▼
Select
iampolicy_mariyam
      │
      ▼
Review
      │
      ▼
Add Permissions
```

---

# Verification

Navigate to:

```text
IAM
└── Users
    └── iamuser_mariyam
        └── Permissions
```

Verify:

- User Name: `iamuser_mariyam`
- Attached Policy: `iampolicy_mariyam`
- Permission Status: Successfully Attached

---

# Key Concepts Learned

### IAM User

An IAM User represents an individual identity that can authenticate and access AWS resources.

### IAM Policy

An IAM Policy is a set of permissions that defines what actions a user, group, or role is allowed to perform.

### Attaching a Policy

Attaching a policy to a user grants the permissions defined in that policy without modifying the policy itself. Multiple users, groups, or roles can share the same policy.

---

# Outcome

Successfully attached the existing IAM policy **iampolicy_mariyam** to the IAM user **iamuser_mariyam** using the AWS Management Console. The user now inherits all permissions defined in the attached policy.
