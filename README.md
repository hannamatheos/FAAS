# Lambda & S3 Integration: Roles and Policies Overview

This document explains the roles and policies involved when using an AWS Lambda function that is triggered by the creation of files in an S3 bucket. It also covers what to configure if the Lambda function needs to upload files to an S3 bucket.

---

## 🔐 1. Execution Role (IAM Role)

The **execution role** is the IAM role assumed by the Lambda function during execution. It grants the function permission to access other AWS resources.

### Purpose:
- Allows the Lambda function to interact with services such as:
  - Amazon S3
  - Amazon CloudWatch Logs
  - Amazon SNS, etc.

### Example Use Case:
If the Lambda function needs to read or write to an S3 bucket, the execution role must include the appropriate `s3:*` permissions.

---

## 📛 2. Resource-Based Policy on the Lambda Function

The **resource-based policy** attached to the Lambda function controls **who or what can invoke the function**.

### Purpose:
- Allows **Amazon S3** to **invoke** the Lambda function when an event (e.g., file creation) occurs.
- Without this policy, the S3 event notification cannot trigger the Lambda.

---

## 📤 3. Lambda Uploading a File to S3

If the Lambda function needs to upload a file to an S3 bucket, the following changes are required:

### ✅ Update to Execution Role:
Add permissions to allow the Lambda function to upload to the S3 bucket:
- `s3:PutObject`
- (Optional) `s3:PutObjectAcl`

**Scope these permissions** to the specific bucket and key prefix as needed.

### 🚫 Resource-Based Policy Changes:
No new resource-based policy is needed **on the Lambda function itself**.

However, if the **S3 bucket has a restrictive bucket policy**, you may need to:
- Add permissions to the bucket policy to allow the Lambda’s execution role to perform `s3:PutObject`.

---


