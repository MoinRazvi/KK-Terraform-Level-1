# Terraform Level-1 Task-39: Create AWS IAM Role Using Variables

## 📌 Task Objective

Create an AWS IAM Role using Terraform while storing configuration values in a separate variables file.

### Requirements

* IAM Role Name: `iamrole_kareem`
* Variable Name: `KKE_iamrole`
* Store configuration values in `variables.tf`
* Reference variables from `main.tf`

Terraform working directory:

```bash
/home/bob/terraform
```

---

# Solution Structure

## variables.tf

```hcl
variable "KKE_iamrole" {
  default = "iamrole_kareem"
}
```

---

## main.tf

```hcl
resource "aws_iam_role" "this" {
  name = var.KKE_iamrole

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
}
```

---

# Terraform Execution Steps

## Initialize Terraform

```bash
terraform init
```

---

## Validate Configuration

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```

---

## Review Execution Plan

```bash
terraform plan
```

Expected resource:

```text
aws_iam_role.this
```

---

## Apply Configuration

```bash
terraform apply -auto-approve
```

Expected output:

```text
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

# Verification

## Verify Terraform State

```bash
terraform state list
```

Expected:

```text
aws_iam_role.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_iam_role.this
```

Expected:

```text
name = "iamrole_kareem"
```

---

## Verify Using AWS CLI

```bash
aws iam get-role \
--role-name iamrole_kareem
```

Expected output:

```text
Role details displayed
```

---

# Terraform Concepts Learned

## Variables

Variables allow configuration values to be managed separately from infrastructure code.

Example:

```hcl
variable "KKE_iamrole" {
  default = "iamrole_kareem"
}
```

---

## Variable Reference

Terraform variables are referenced using:

```hcl
var.<variable_name>
```

Example:

```hcl
var.KKE_iamrole
```

---

## IAM Role Resource

```hcl
resource "aws_iam_role" "this" {
}
```

Breakdown:

```text
aws_iam_role → Resource Type
this         → Resource Name
```

---

## Assume Role Policy

Every IAM Role requires a trust policy (`assume_role_policy`).

Example:

```hcl
assume_role_policy = jsonencode({
  Version = "2012-10-17"
  Statement = [{
    Effect = "Allow"
    Principal = {
      Service = "ec2.amazonaws.com"
    }
    Action = "sts:AssumeRole"
  }]
})
```

This allows EC2 instances to assume the role.

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_iam_role.this
```

Verify IAM Role:

```bash
aws iam get-role \
--role-name iamrole_kareem
```

---

# 🎯 Outcome

Successfully created:

* IAM Role: `iamrole_kareem`
* Variable: `KKE_iamrole`
* Terraform-managed resource

using a modular structure with `main.tf` and `variables.tf`.

---

# ⚠ Issues Faced & Solution

## Issue

Terraform validation fails with:

```text
Reference to undeclared input variable
```

### Root Cause

Variable is referenced in `main.tf` but not declared in `variables.tf`.

### Solution

Ensure `variables.tf` contains:

```hcl
variable "KKE_iamrole" {
  default = "iamrole_kareem"
}
```

and reference it as:

```hcl
name = var.KKE_iamrole
```

---

# 🚀 Key Takeaway

This task demonstrates how Terraform variables can be used to separate configuration values from infrastructure code. IAM Roles require both a name and an assume role policy (trust policy), making them slightly different from IAM Users and IAM Groups.
