# Day 72: Create an AWS Lambda Function Using the AWS Management Console

**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS Lambda

---

## Challenge

The Nautilus DevOps Team wanted to demonstrate AWS serverless computing by deploying a simple AWS Lambda function. The function should return a custom greeting message while using a dedicated IAM execution role.

### Requirements

- Create a Lambda function named **nautilus-lambda**.
- Use the **Python** runtime.
- Create and use an IAM role named **lambda_execution_role**.
- The Lambda function should return:
  - **Status Code:** `200`
  - **Body:** `Welcome to KKE AWS Labs!`
- Complete the task using the **AWS Management Console**.

---

# Solution

## Step 1: Log in to the AWS Console

- Logged in using the temporary AWS credentials provided by KodeKloud.
- Selected the **us-east-1** region.

---

## Step 2: Create the IAM Role

Navigate to:

```text
AWS Console
→ IAM
→ Roles
```

Click:

```text
Create Role
```

Configure the following:

| Property | Value |
|----------|-------|
| Trusted Entity | AWS Service |
| Use Case | Lambda |

Attach the policy:

```text
AWSLambdaBasicExecutionRole
```

Enter:

| Property | Value |
|----------|-------|
| Role Name | `lambda_execution_role` |

Click:

```text
Create Role
```

---

## Step 3: Create the Lambda Function

Navigate to:

```text
AWS Console
→ Lambda
→ Functions
```

Click:

```text
Create Function
```

Configure the following:

| Property | Value |
|----------|-------|
| Author From Scratch | Yes |
| Function Name | `nautilus-lambda` |
| Runtime | Python |
| Architecture | x86_64 (Default) |
| Execution Role | Use an existing role |
| Existing Role | `lambda_execution_role` |

Click:

```text
Create Function
```

---

## Step 4: Update the Lambda Function Code

Replace the default code with the following:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
```

Click:

```text
Deploy
```

---

## Step 5: Test the Lambda Function

Click:

```text
Test
```

Create a new test event using the default template and save it.

Click:

```text
Test
```

Expected Response:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# Steps Performed

```text
AWS Console
      │
      ▼
IAM
      │
      ▼
Create Role
      │
      ▼
AWS Service
      │
      ▼
Lambda
      │
      ▼
Attach Policy
AWSLambdaBasicExecutionRole
      │
      ▼
Role Name
lambda_execution_role
      │
      ▼
Create Role
      │
      ▼
Lambda
      │
      ▼
Create Function
      │
      ▼
Function Name
nautilus-lambda
      │
      ▼
Runtime
Python
      │
      ▼
Execution Role
lambda_execution_role
      │
      ▼
Deploy Function
      │
      ▼
Create Test Event
      │
      ▼
Invoke Function
```

---

# Verification

Navigate to:

```text
AWS Console
└── Lambda
    └── Functions
        └── nautilus-lambda
```

Verify:

| Property | Expected Value |
|----------|----------------|
| Function Name | nautilus-lambda |
| Runtime | Python |
| Execution Role | lambda_execution_role |

Run a test event and verify the response:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# Key Concepts Learned

### AWS Lambda

AWS Lambda is a serverless compute service that allows you to run code without provisioning or managing servers. AWS automatically handles scaling, availability, and infrastructure management.

### IAM Execution Role

Every Lambda function requires an IAM execution role that grants permissions to interact with AWS services. The **AWSLambdaBasicExecutionRole** policy provides permissions to write logs to Amazon CloudWatch.

### Lambda Handler

The Lambda handler is the entry point that AWS invokes whenever the function is executed. It processes the incoming event and returns a response.

### Serverless Computing

Serverless architecture allows developers to focus entirely on application code while AWS manages the underlying infrastructure, scaling, and maintenance.

---

# Outcome

Successfully created an AWS Lambda function named **nautilus-lambda** using the **Python** runtime and an IAM execution role named **lambda_execution_role**. The function was deployed through the AWS Management Console and successfully returned an HTTP **status code 200** with the response body **"Welcome to KKE AWS Labs!"**, demonstrating a basic serverless application.
