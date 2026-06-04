# Terraform Level-1 Task-14: Create IAM User

## 📌 Task Objective

Create an AWS IAM user using Terraform.

### Requirements

* IAM User Name: `iamuser_kirsty`
* Region: `us-east-1`

Terraform working directory:

```bash
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "iamuser_kirsty" {
  name = "iamuser_kirsty"
}
```

---

## Step 3: Initialize Terraform

```bash
terraform init
```

---

## Step 4: Validate Configuration

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 6: Verify IAM User Creation

```bash
aws iam get-user --user-name iamuser_kirsty
```

Verify:

```text
iamuser_kirsty
```

---

## 🔍 Key Concepts Learned

### IAM User

An IAM user represents an identity within an AWS account.

IAM users can be assigned:

* Passwords
* Access keys
* Permissions
* Policies

---

### IAM Resource

```hcl
resource "aws_iam_user"
```

Used to create and manage IAM users through Terraform.

---

### User Name

```hcl
name = "iamuser_kirsty"
```

Defines the IAM user name in AWS.

---

### Infrastructure as Code

Terraform enables IAM resources to be version-controlled and managed consistently.

---

## ✅ Verification Command

```bash
aws iam get-user --user-name iamuser_kirsty
```

---

## 🎯 Outcome

Successfully created:

* AWS IAM User
* User Name: `iamuser_kirsty`
* Terraform-managed IAM resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS Identity and Access Management (IAM)
* Terraform IAM resources
* User provisioning through Infrastructure as Code
* Identity management automation

This task introduces foundational IAM resource management using Terraform, which is essential for secure AWS access control and governance.
