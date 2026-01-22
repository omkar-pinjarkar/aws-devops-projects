# Project 03 – IAM Least Privilege (Clean Implementation)

## Overview

This project demonstrates a **clean and minimal implementation of the Principle of Least Privilege** using AWS IAM.

An EC2 instance is allowed to **read objects from only one specific S3 bucket**, while all other actions and resources are denied by default.

The focus of this project is **security, clarity, and correctness**, following real DevOps best practices.

---

## Objective

- Implement least privilege access using IAM
- Avoid over-permissioned AWS managed policies
- Use IAM Role instead of IAM User
- Restrict access to one S3 bucket only
- Validate permissions through real testing

---

## Why Least Privilege Matters

In real-world DevOps environments:
- Servers can be compromised
- Credentials can be exposed
- Mistakes can cause data loss

Least privilege helps:
- Reduce blast radius
- Improve security posture
- Prevent accidental damage

Over-permission is a security risk.

---

## Project Scenario

An EC2 instance should:

- ✅ Read objects from **one specific S3 bucket**
- ❌ Not list buckets
- ❌ Not delete objects
- ❌ Not upload objects
- ❌ Not access any other S3 bucket

---

## Architecture

EC2 Instance
|
| (Assumes IAM Role)
v
IAM Role (Least Privilege Policy)
|
v
Amazon S3 (Single Bucket Only)

---

## S3 Resource Used

- Bucket name:
devops-least-privilege-demo

- Test file: test.txt

---

## IAM Policy (Least Privilege)

```json
{
"Version": "2012-10-17",
"Statement": [
  {
    "Sid": "AllowReadObjectsOnly",
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::devops-least-privilege-demo/*"
  }
]
}
```
IAM Role

Policy name: EC2-S3-Read-One-Bucket-Only

Role name: EC2-S3-Read-Only-One-Bucket-Role

Trusted service: EC2

No IAM users or access keys were used

Validation
Allowed Action (Success)
aws s3 cp s3://devops-least-privilege-demo/test.txt .

Denied Actions (Expected Failure)
aws s3 rm s3://devops-least-privilege-demo/test.txt
aws s3 ls

-AccessDenied confirms correct least-privilege enforcement.

------------------------------------------------------------------------------

Key Learnings

-Least privilege should start minimal
-IAM Roles are mandatory for EC2
-Object-level permissions are predictable
-Security must be validated, not assumed

Project Status

-Completed
-January 2026
