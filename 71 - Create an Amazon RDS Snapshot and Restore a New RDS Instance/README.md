# Day 71: Create an Amazon RDS Snapshot and Restore a New RDS Instance

**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS RDS

---

## Challenge

The Nautilus Development Team needed to create a backup of an existing Amazon RDS instance before performing a major database upgrade. The backup would be used to restore a new RDS instance for testing and validation purposes.

### Requirements

- Wait until the existing RDS instance **datacenter-rds** is in the **Available** state.
- Create a manual snapshot named **datacenter-snapshot**.
- Restore the snapshot to a new RDS instance named **datacenter-snapshot-restore**.
- Configure the restored instance with the **db.t3.micro** instance class.
- Ensure the restored RDS instance reaches the **Available** state.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials provided by KodeKloud.
- Selected the **us-east-1** region.

---

## Step 2: Verify the Existing RDS Instance

Navigate to:

```text
AWS Console
→ Amazon RDS
→ Databases
```

Verify that the following RDS instance exists and is in the **Available** state:

```text
datacenter-rds
```

---

## Step 3: Create a Manual Snapshot

Select the RDS instance:

```text
datacenter-rds
```

Choose:

```text
Actions
→ Take Snapshot
```

Configure:

| Property | Value |
|----------|-------|
| Snapshot Identifier | `datacenter-snapshot` |

Click:

```text
Create Snapshot
```

Wait until the snapshot status changes to:

```text
Available
```

---

## Step 4: Restore the Snapshot

Navigate to:

```text
Amazon RDS
→ Snapshots
```

Select:

```text
datacenter-snapshot
```

Choose:

```text
Actions
→ Restore Snapshot
```

Configure the following:

| Property | Value |
|----------|-------|
| DB Instance Identifier | `datacenter-snapshot-restore` |
| DB Instance Class | `db.t3.micro` |

Keep all remaining settings as their default values.

Click:

```text
Restore DB Instance
```

---

## Step 5: Wait for the Restore Process

Navigate to:

```text
Amazon RDS
→ Databases
```

Wait until the restored database status changes from:

```text
Creating
```

to:

```text
Available
```

---

# Steps Performed

```text
AWS Console
      │
      ▼
Amazon RDS
      │
      ▼
Databases
      │
      ▼
Verify
datacenter-rds
(Status: Available)
      │
      ▼
Actions
      │
      ▼
Take Snapshot
      │
      ▼
Snapshot Name
datacenter-snapshot
      │
      ▼
Wait Until
Snapshot Available
      │
      ▼
Snapshots
      │
      ▼
Select
datacenter-snapshot
      │
      ▼
Restore Snapshot
      │
      ▼
DB Identifier
datacenter-snapshot-restore
      │
      ▼
Instance Class
db.t3.micro
      │
      ▼
Restore DB Instance
      │
      ▼
Status
Available
```

---

# Verification

Navigate to:

```text
Amazon RDS
└── Databases
```

Verify the restored instance:

| Property | Expected Value |
|----------|----------------|
| DB Instance Identifier | datacenter-snapshot-restore |
| Status | Available |
| Instance Class | db.t3.micro |

---

Navigate to:

```text
Amazon RDS
└── Snapshots
```

Verify:

| Property | Expected Value |
|----------|----------------|
| Snapshot Identifier | datacenter-snapshot |
| Status | Available |

---

# Key Concepts Learned

### Amazon RDS Snapshot

An RDS Snapshot is a point-in-time backup of an RDS database instance. It can be used to restore the database whenever needed for backup, disaster recovery, or testing purposes.

### Manual Snapshot

A manual snapshot is created by the user and is retained until it is explicitly deleted, unlike automated backups that expire based on the configured retention period.

### Restore from Snapshot

Restoring from a snapshot creates a new RDS instance containing the same data and configuration as the original database at the time the snapshot was taken.

### DB Instance Class

The DB instance class defines the compute and memory resources allocated to an RDS instance. In this task, the restored database uses the **db.t3.micro** instance class.

---

# Outcome

Successfully created a manual snapshot named **datacenter-snapshot** from the existing **datacenter-rds** database instance and restored it to a new RDS instance named **datacenter-snapshot-restore**. Configured the restored instance with the **db.t3.micro** instance class and verified that both the snapshot and restored database reached the **Available** state.
