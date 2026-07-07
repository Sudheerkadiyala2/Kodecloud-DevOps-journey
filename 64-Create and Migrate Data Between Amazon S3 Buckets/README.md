# Task 64: Create and Migrate Data Between Amazon S3 Buckets

**Date:** 2026-07-02  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS S3

---

## Challenge

The Nautilus DevOps team was tasked with migrating data from an existing Amazon S3 bucket to a new S3 bucket as part of a data migration project.

### Requirements

- Create a new **private** S3 bucket named **devops-sync-22796**.
- Migrate all data from the existing bucket **devops-s3-13218**.
- Ensure that both buckets contain identical data after migration.
- Verify that the migration completed successfully.

> **Note:** Although the task suggested using the AWS CLI, this task was completed using the **AWS Management Console (AWS UI)**.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in to the AWS Management Console using the provided temporary credentials.
- Selected the **us-east-1** region.

---

## Step 2: Open Amazon S3

- Navigate to **Amazon S3**.
- Open the **Buckets** page.

---

## Step 3: Create the Destination Bucket

- Click **Create bucket**.
- Enter the bucket name:

```text
devops-sync-22796
```

- Keep **Object Ownership** as the default.
- Leave **Block Public Access** enabled (Private Bucket).
- Keep the remaining settings as default.
- Click **Create bucket**.

---

## Step 4: Copy Data from the Source Bucket

Open the source bucket:

```text
devops-s3-13218
```

- Select all objects.
- Click **Copy**.
- Navigate to the destination bucket:

```text
devops-sync-22796
```

- Click **Paste**.
- Wait for the copy operation to complete successfully.

---

# Steps Performed

```text
AWS Console
      │
      ▼
Amazon S3
      │
      ▼
Buckets
      │
      ▼
Create Bucket
      │
      ▼
Bucket Name
devops-sync-22796
      │
      ▼
Create Bucket
      │
      ▼
Open Source Bucket
devops-s3-13218
      │
      ▼
Select All Objects
      │
      ▼
Copy
      │
      ▼
Open Destination Bucket
devops-sync-22796
      │
      ▼
Paste
```

---

# Verification

Verify the destination bucket:

```text
Amazon S3
└── Buckets
    └── devops-sync-22796
```

Confirm:

- Bucket exists.
- Bucket is private.
- All files from **devops-s3-13218** are present.
- File count matches the source bucket.
- Folder structure is identical.
- Objects are accessible without errors.

---

# Key Concepts Learned

### Amazon S3 Bucket

An S3 bucket is a scalable object storage container used to store files, backups, logs, and application data.

### Private Bucket

A private bucket blocks public access, ensuring that only authorized AWS users or roles can access the stored objects.

### Data Migration

Migrating data between S3 buckets involves copying all objects while preserving the folder hierarchy and object integrity.

### Data Verification

After migration, verify:

- Number of objects
- Folder structure
- Object names
- File accessibility

to ensure the destination bucket contains the same data as the source bucket.

---

# Outcome

Successfully created the private Amazon S3 bucket **devops-sync-22796** and migrated all objects from **devops-s3-13218** using the **AWS Management Console**. Verified that both buckets contained identical data, ensuring a successful and complete data migration.
