# Project 02 – IAM Role vs IAM User on EC2 (DevOps Security Comparison)

## 📌 Overview

This project demonstrates a **real-world security comparison** between using an **IAM User with access keys** and an **IAM Role** for allowing an EC2 instance to access AWS services.

The goal is to prove **why DevOps engineers never use access keys on servers** and always prefer **IAM Roles** in production environments.

---

## 🎯 Objective

* Test EC2 access to S3 using **IAM User credentials**
* Test EC2 access to S3 using **IAM Role**
* Compare both approaches from a **security and DevOps perspective**
* Identify the correct production-ready method

---

## 🏗️ Architecture

```
IAM User (Access Keys)  ---> EC2-A ---> S3   ❌ Insecure
IAM Role (Temporary)   ---> EC2-B ---> S3   ✅ Secure
```

---

## ❌ Method 1: IAM User with Access Keys (EC2-A)

### Steps Performed

* Created IAM user: `ec2-s3-user-test`
* Attached policy: `AmazonS3ReadOnlyAccess`
* Generated access keys
* Configured AWS CLI on EC2 using:

```bash
aws configure
```

* Successfully accessed S3 using:

```bash
aws s3 ls
```

### Why This Is Insecure

* Access keys stored on disk (`~/.aws/credentials`)
* High risk if EC2 is compromised
* Manual credential rotation required
* Violates AWS and DevOps security best practices

---

## ✅ Method 2: IAM Role for EC2 (EC2-B – Recommended)

### Steps Performed

* Created IAM Role: `EC2-S3-Read-Only-Role`
* Attached role directly to EC2
* Installed AWS CLI (no configuration required)
* Verified role assumption:

```bash
aws sts get-caller-identity
```

* Accessed S3 securely:

```bash
aws s3 ls
```

### Why This Is Secure

* No credentials stored on the server
* AWS provides temporary credentials automatically
* Reduced blast radius in case of compromise
* Fully aligns with AWS and DevOps standards

---

## 🔐 Security Comparison

| Aspect                    | IAM User (❌) | IAM Role (✅) |
| ------------------------- | ------------ | ------------ |
| Credentials stored on EC2 | Yes          | No           |
| Risk if EC2 compromised   | High         | Low          |
| Credential rotation       | Manual       | Automatic    |
| AWS recommended           | No           | Yes          |
| Production usage          | Never        | Always       |
| DevOps best practice      | ❌            | ✅            |

---

## 🧹 Cleanup Performed

* Deleted IAM user with access keys
* Terminated insecure EC2 instance
* Retained only IAM Role–based architecture

---

## 🧠 Key Learnings

* IAM Users should never be used on EC2
* IAM Roles are mandatory for production workloads
* Security-first thinking is critical in DevOps
* Working solutions are not always secure solutions

---

## 💬 Interview-Ready Statement

> “I compared IAM User–based access and IAM Role–based access on EC2 and demonstrated why DevOps teams always use IAM Roles instead of access keys.”

---

## 📌 Project Status

✅ Completed
📅 January 2026

---

## 🔜 Next Project

**Project 03 – IAM Least Privilege Policy Design**
