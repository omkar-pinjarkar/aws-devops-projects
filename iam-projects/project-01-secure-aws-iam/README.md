# Project 01 – Secure AWS Account Setup with IAM (DevOps Best Practices)

## Objective
Secure an AWS account using real DevOps IAM best practices and validate IAM Role–based access from EC2 without using access keys.

---

## What This Project Covers
- Root user security with MFA
- IAM Users, Groups, and Roles
- IAM Role attached to EC2
- Secure access to AWS services without access keys

---

## Root User Security
- Enabled MFA on root user
- Root user restricted to billing and emergency access only

---

## IAM Setup

### IAM Admin User
- User: devops-admin
- Permissions: AdministratorAccess
- Used for daily AWS operations

### IAM Groups
| Group        | Permission              |
|--------------|-------------------------|
| Admins       | AdministratorAccess     |
| Developers   | AmazonEC2ReadOnlyAccess |
| Interns      | AmazonS3ReadOnlyAccess  |

---

## IAM Role for EC2
- Role Name: EC2-S3-Read-Only-Role
- Policy: AmazonS3ReadOnlyAccess
- Attached directly to EC2

---

## EC2 Role Validation

### AWS CLI Installation (Ubuntu)
```bash
sudo snap install aws-cli --classic
aws --version

### Verify Role Assumption
aws sts get-caller-identity

Test S3 Access
aws s3 ls

### Security Best Practices Followed
-No access keys on EC2
-IAM Roles used instead of IAM Users
-Least privilege access
-Group-based permissions

### Common Mistakes Avoided
-Using root for daily work
-Running aws configure on EC2
-Storing AWS access keys on servers
