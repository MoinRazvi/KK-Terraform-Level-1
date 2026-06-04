# Terraform Level-1 Task-15: Create IAM Group

## 📌 Task Objective

Create an AWS IAM group using Terraform.

### Requirements

* IAM Group Name: `iamgroup_rose`
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

resource "aws_iam_group" "iamgroup_rose" {
  name = "iamgroup_rose"
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

## Step 6: Verify IAM Group Creation

```bash
aws iam get-group --group-name iamgroup_rose
```

Verify:

```text
iamgroup_rose
```

---

## 🔍 Key Concepts Learned

### IAM Group

An IAM group is a collection of IAM users.

Groups help simplify permission management by allowing policies to be assigned to multiple users at once.

---

### IAM Group Resource

```hcl
resource "aws_iam_group"
```

Used to create and manage IAM groups through Terraform.

---

### Group Name

```hcl
name = "iamgroup_rose"
```

Defines the IAM group name in AWS.

---

### Infrastructure as Code

Terraform enables IAM resources to be provisioned, tracked, and managed consistently through code.

---

## ✅ Verification Command

```bash
aws iam get-group --group-name iamgroup_rose
```

---

## 🎯 Outcome

Successfully created:

* AWS IAM Group
* Group Name: `iamgroup_rose`
* Terraform-managed IAM resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS Identity and Access Management (IAM)
* IAM group provisioning
* Terraform IAM resources
* Access management automation

This task introduces IAM group management using Terraform, helping streamline user permission administration in AWS environments.
