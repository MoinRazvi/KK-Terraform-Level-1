# Terraform Level-1 Task-16: Create IAM Policy

## 📌 Task Objective

Create an AWS IAM policy using Terraform.

### Requirements

* IAM Policy Name: `iampolicy_mariyam`
* Region: `us-east-1`
* Policy Type: Read-only access to Amazon EC2
* Users should be able to view:

  * EC2 Instances
  * AMIs
  * Snapshots
  * Related EC2 resources in the AWS Console

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

resource "aws_iam_policy" "iampolicy_mariyam" {
  name        = "iampolicy_mariyam"
  description = "Read-only access to EC2 console"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:Describe*"
        ]
        Resource = "*"
      }
    ]
  })
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

## Step 6: Verify IAM Policy Creation

```bash
aws iam list-policies --scope Local
```

Verify:

```text
iampolicy_mariyam
```

---

## Step 7: View Policy Details

```bash
aws iam get-policy \
--policy-arn <policy-arn>
```

Verify the policy grants:

```text
ec2:Describe*
```

permissions.

---

## 🔍 Key Concepts Learned

### IAM Policy

An IAM policy is a JSON document that defines permissions for AWS resources and services.

---

### Read-Only EC2 Access

```json
{
  "Effect": "Allow",
  "Action": "ec2:Describe*",
  "Resource": "*"
}
```

Provides read-only access to EC2 resources.

---

### EC2 Describe Permissions

The following resources can be viewed:

* EC2 Instances
* Amazon Machine Images (AMIs)
* EBS Snapshots
* Security Groups
* VPC Information
* Volumes

---

### jsonencode()

```hcl
policy = jsonencode({...})
```

Converts Terraform configuration into valid IAM JSON policy syntax.

---

## ✅ Verification Commands

```bash
aws iam list-policies --scope Local
```

```bash
aws iam get-policy \
--policy-arn <policy-arn>
```

---

## 🎯 Outcome

Successfully created:

* AWS IAM Policy
* Policy Name: `iampolicy_mariyam`
* Read-only EC2 Console Access
* Terraform-managed IAM resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* IAM policy creation
* EC2 read-only permissions
* JSON policy documents
* Terraform IAM automation

This task introduces permission management using Terraform, an essential component of AWS security and access control.
