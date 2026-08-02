# Day 73: Create and Deploy an AWS Lambda Function Using AWS CLI

**Date:** 2026-08-02  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** AWS Lambda

---

## Challenge

The Nautilus DevOps Team continued exploring serverless architecture by deploying an AWS Lambda function using the AWS CLI. The objective was to create a Python Lambda function, package it into a ZIP archive, and deploy it using an existing IAM execution role.

### Requirements

- Create a Python script named **lambda_function.py**.
- The function should return:
  - **Status Code:** `200`
  - **Body:** `Welcome to KKE AWS Labs!`
- Compress the script into a ZIP file named **function.zip**.
- Create a Lambda function named **nautilus-lambda-cli** using the ZIP file.
- Use the **Python** runtime.
- Use the existing IAM role **lambda_execution_role**.
- Complete the task using the **AWS CLI**.

---

# Solution

## Step 1: Connect to the AWS Client

Logged in to the **aws-client** host where the AWS CLI was already configured.

---

## Step 2: Create the Python Script

Create a file named:

```text
lambda_function.py
```

Add the following code:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
```

---

## Step 3: Create the ZIP Package

Compress the Python script into a deployment package.

```bash
zip function.zip lambda_function.py
```

---

## Step 4: Retrieve the IAM Role ARN

List the IAM role information:

```bash
aws iam get-role --role-name lambda_execution_role
```

Copy the **Role ARN** returned by the command.

Example:

```text
arn:aws:iam::<account-id>:role/lambda_execution_role
```

---

## Step 5: Create the Lambda Function

Run the following command:

```bash
aws lambda create-function \
  --function-name nautilus-lambda-cli \
  --runtime python3.13 \
  --role <ROLE_ARN> \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip
```

Replace:

```text
<ROLE_ARN>
```

with the ARN copied in the previous step.

---

## Step 6: Verify the Lambda Function

Run:

```bash
aws lambda get-function \
  --function-name nautilus-lambda-cli
```

Verify that the function is created successfully.

---

# Steps Performed

```text
aws-client
      │
      ▼
Create
lambda_function.py
      │
      ▼
Write Python Function
      │
      ▼
Create ZIP
function.zip
      │
      ▼
Get IAM Role ARN
lambda_execution_role
      │
      ▼
AWS CLI
      │
      ▼
Create Lambda Function
      │
      ▼
Function Name
nautilus-lambda-cli
      │
      ▼
Verify Deployment
```

---

# Verification

Verify the Lambda function:

```bash
aws lambda get-function \
  --function-name nautilus-lambda-cli
```

Expected Result:

- Function Name: `nautilus-lambda-cli`
- Runtime: Python
- Execution Role: `lambda_execution_role`
- State: Active

---

# Key Concepts Learned

### AWS Lambda

AWS Lambda is a serverless compute service that automatically runs application code without requiring server provisioning or management.

### Deployment Package

Lambda functions can be deployed using a ZIP archive containing the application code and required dependencies.

### Handler

The Lambda handler specifies the entry point that AWS invokes when the function executes.

Example:

```text
lambda_function.lambda_handler
```

where:

- `lambda_function` → Python file name.
- `lambda_handler` → Function name.

### IAM Execution Role

An execution role provides the Lambda function with the permissions required to interact with AWS services such as CloudWatch Logs.

### AWS CLI

The AWS Command Line Interface (CLI) enables users to create and manage AWS resources directly from the terminal using commands.

---

# Outcome

Successfully created a Python script named **lambda_function.py**, packaged it into **function.zip**, and deployed it as an AWS Lambda function named **nautilus-lambda-cli** using the **AWS CLI**. The function uses the existing **lambda_execution_role** IAM role and returns an HTTP **status code 200** with the response body **"Welcome to KKE AWS Labs!"**, demonstrating a complete serverless deployment workflow using the command line.
