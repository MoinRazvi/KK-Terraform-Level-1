# Terraform Level-1 Task-40: Create AWS IAM Policy Using Variables

## 📌 Task Objective

Create an AWS IAM Policy using Terraform while storing configuration values in a separate variables file.

### Requirements

* IAM Policy Name: `iampolicy_kirsty`
* Variable Name: `KKE_iampolicy`
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
variable "KKE_iampolicy" {
  default = "iampolicy_kirsty"
}
```

---

## main.tf

```hcl
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy
  description = "IAM policy created using Terraform"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = "*"
        Resource = "*"
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
aws_iam_policy.this
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
aws_iam_policy.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_iam_policy.this
```

Expected:

```text
name = "iampolicy_kirsty"
```

---

## Verify Using AWS CLI

```bash
aws iam list-policies \
--scope Local \
--query "Policies[?PolicyName=='iampolicy_kirsty']"
```

Expected output:

```text
Policy details displayed
```

---

# Terraform Concepts Learned

## Variable Declaration

```hcl
variable "KKE_iampolicy" {
  default = "iampolicy_kirsty"
}
```

Stores configuration values separately from infrastructure code.

---

## Variable Reference

```hcl
var.KKE_iampolicy
```

Terraform variable references follow:

```text
var.<variable_name>
```

---

## IAM Policy Resource

```hcl
resource "aws_iam_policy" "this"
```

Breakdown:

```text
aws_iam_policy → Resource Type
this           → Resource Name
```

---

## Policy Document

IAM Policies require a JSON policy document.

Terraform converts HCL to JSON using:

```hcl
jsonencode({...})
```

Example:

```hcl
policy = jsonencode({
  Version = "2012-10-17"
  Statement = [{
    Effect   = "Allow"
    Action   = "*"
    Resource = "*"
  }]
})
```

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_iam_policy.this
```

Verify IAM Policy:

```bash
aws iam list-policies \
--scope Local \
--query "Policies[?PolicyName=='iampolicy_kirsty']"
```

---

# 🎯 Outcome

Successfully created:

* IAM Policy: `iampolicy_kirsty`
* Variable: `KKE_iampolicy`
* Terraform-managed resource

using a modular structure with:

* `variables.tf`
* `main.tf`

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
variable "KKE_iampolicy" {
  default = "iampolicy_kirsty"
}
```

and reference it as:

```hcl
name = var.KKE_iampolicy
```

---

# 🚀 Key Takeaway

This task demonstrates how Terraform variables can be used to separate configuration values from infrastructure code. IAM Policies are unique because they require a policy document, which is typically created using Terraform's `jsonencode()` function.
