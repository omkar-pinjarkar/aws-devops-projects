# Project 03 – IAM Least Privilege Policy

## Overview

This project demonstrates how to design and implement an **IAM Least Privilege policy** in AWS using a real DevOps scenario.

Instead of using broad AWS managed policies, a **custom IAM policy** is created and attached to an **IAM Role** to ensure an EC2 instance has only the permissions it actually needs.

This approach is required in **production DevOps environments** to reduce security risk.

---

## Objective

- Understand the principle of least privilege
- Avoid over-permissioned IAM policies
- Design a custom IAM policy
- Restrict access by action and resource
- Validate allowed and denied permissions
- Follow security-first DevOps practices

---

## Why Least Privilege Matters

In real-world systems:
- Servers can be compromised
- Credentials can leak
- Human errors happen

Least privilege helps:
- Limit blast radius
- Reduce attack impact
- Prevent accidental data loss
- Meet security and compliance standards

**DevOps rule:**  
Over-permission is a security vulnerability.

---

## Project Scenario

An EC2 instance needs to:

- Allow:
  - List a specific S3 bucket
  - Read objects from that bucket
- Deny:
  - Deleting objects
  - Uploading objects
  - Accessing any other S3 bucket

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

## Step 1: Create a Dedicated S3 Bucket

Bucket created for testing:

devops-least-privilege-demo


A test file was uploaded:



test.txt


This bucket is the only allowed resource.

---

## Step 2: Custom IAM Policy

A custom IAM policy was created instead of using AWS managed policies.

### IAM Policy JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowListSpecificBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::devops-least-privilege-demo"
    },
    {
      "Sid": "AllowReadObjectsFromBucket",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::devops-least-privilege-demo/*"
    }
  ]
}
```
Why This Policy Is Least Privilege

Access limited to one bucket
Only read operations allowed
No delete or write permissions
No access to other buckets
The policy grants only what is required.

## Step 3: IAM Role Creation

IAM Policy Name:
EC2-S3-Least-Privilege-Policy

IAM Role Name:
EC2-S3-Least-Privilege-Role

Trusted Entity:
EC2
Policy attached to the role

No IAM users or access keys were used.

## Step 4: Attach Role to EC2

IAM Role attached to an existing EC2 instance
No instance reboot required
EC2 automatically assumed the role

## Step 5: Permission Validation
Allowed Actions (Expected Success)
aws s3 ls s3://devops-least-privilege-demo
aws s3 cp s3://devops-least-privilege-demo/test.txt .

Denied Actions (Expected Failure)
aws s3 rm s3://devops-least-privilege-demo/test.txt
aws s3 ls s3://some-other-bucket


AccessDenied errors confirm the policy is working correctly.

Security Validation Summary
Action	Result
List allowed bucket	Allowed
Read object	Allowed
Delete object	Denied
Access other buckets	Denied
Security Hygiene

No access keys created
No IAM users used
Permissions restricted by resource

Only secure configuration retained

Key Learnings

Managed policies are convenient but unsafe for production
Least privilege must be designed intentionally
Always restrict permissions by resource
Validation is mandatory in security work
Security is a core DevOps responsibility

Interview-Ready Explanation-
"I designed and tested a least-privilege IAM policy that allowed an EC2 instance to read from only one S3 bucket while explicitly denying all other access."

Project Status

Completed
January 2026

Next Project

Project 04 – IAM Policy Conditions (Time-based and IP-based access control)
