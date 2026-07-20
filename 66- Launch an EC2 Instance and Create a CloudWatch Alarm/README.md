# Task 66: Launch an EC2 Instance and Create a CloudWatch Alarm
  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS EC2 & Amazon CloudWatch

---

## Challenge

The Nautilus DevOps team needed to launch an EC2 instance and configure a CloudWatch alarm to monitor its CPU utilization. The alarm should notify the team whenever CPU utilization exceeds the defined threshold.

### Requirements

- Launch an EC2 instance named **devops-ec2** using any Ubuntu AMI.
- Create a CloudWatch alarm named **devops-alarm**.
- Configure the alarm with:
  - **Metric:** CPUUtilization
  - **Statistic:** Average
  - **Threshold:** Greater than or equal to 90%
  - **Evaluation Period:** 1 consecutive 5-minute period
- Send alarm notifications to the existing SNS topic **devops-sns-topic**.

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
| Security Group | Default |

Click **Launch Instance**.

---

## Step 3: Open CloudWatch

Navigate to:

```text
CloudWatch → Alarms
```

Click **Create Alarm**.

---

## Step 4: Select the Metric

- Click **Select Metric**.
- Navigate to:

```text
EC2 → Per-Instance Metrics
```

- Select the metric:

```text
CPUUtilization
```

for the **devops-ec2** instance.

Click **Select Metric**.

---

## Step 5: Configure the Alarm

Configure the alarm with the following values:

| Setting | Value |
|---------|-------|
| Statistic | Average |
| Period | 5 Minutes |
| Threshold Type | Static |
| Condition | Greater than or equal to |
| Threshold Value | 90 |

Evaluation:

```text
1 out of 1 datapoints
```

---

## Step 6: Configure Notifications

Under **Notification**:

- Choose **In Alarm**.
- Select the existing SNS topic:

```text
devops-sns-topic
```

Click **Next**.

---

## Step 7: Name the Alarm

Enter:

| Field | Value |
|-------|-------|
| Alarm Name | `devops-alarm` |

Review the configuration and click **Create Alarm**.

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
Ubuntu Server
      │
      ▼
Instance Name
devops-ec2
      │
      ▼
Launch
      │
      ▼
CloudWatch
      │
      ▼
Alarms
      │
      ▼
Create Alarm
      │
      ▼
EC2 Metrics
      │
      ▼
CPUUtilization
      │
      ▼
Average
Period: 5 Minutes
Threshold: >= 90%
      │
      ▼
SNS Topic
devops-sns-topic
      │
      ▼
Alarm Name
devops-alarm
      │
      ▼
Create Alarm
```

---

# Verification

### Verify EC2 Instance

Navigate to:

```text
EC2
└── Instances
```

Verify:

- Instance Name: `devops-ec2`
- State: **Running**

---

### Verify CloudWatch Alarm

Navigate to:

```text
CloudWatch
└── Alarms
```

Verify:

- Alarm Name: `devops-alarm`
- Metric: CPUUtilization
- Statistic: Average
- Threshold: >= 90%
- Period: 5 Minutes
- SNS Topic: `devops-sns-topic`

---

### Verify SNS Notification

Open:

```text
Amazon SNS
└── Topics
    └── devops-sns-topic
```

Confirm that **devops-alarm** is configured to publish notifications to this topic.

---

# Key Concepts Learned

### Amazon EC2

Amazon EC2 provides scalable virtual servers in the AWS Cloud for hosting applications.

### Amazon CloudWatch

CloudWatch monitors AWS resources and applications by collecting metrics, logs, and events.

### CloudWatch Alarm

A CloudWatch Alarm continuously monitors a metric and changes state when a defined threshold is reached.

### Amazon SNS

Amazon Simple Notification Service (SNS) is used to send notifications when CloudWatch alarms are triggered.

---

# Outcome

Successfully launched an Ubuntu EC2 instance named **devops-ec2** and configured a CloudWatch alarm named **devops-alarm** to monitor CPU utilization. The alarm is configured to trigger when CPU utilization reaches **90% or higher** for one consecutive **5-minute** period and sends notifications through the existing **devops-sns-topic** SNS topic.
