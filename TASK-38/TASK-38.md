# Terraform Level-1 Task-38: Create AWS IAM User Using Variables

## 📌 Task Objective

Create an AWS IAM User using Terraform while storing configuration values in a separate variables file.

### Requirements

* IAM User Name: `iamuser_james`
* Variable Name: `KKE_user`
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
variable "KKE_user" {
  default = "iamuser_james"
}
```

---

## main.tf

```hcl
resource "aws_iam_user" "this" {
  name = var.KKE_user
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
aws_iam_user.this
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
aws_iam_user.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_iam_user.this
```

Expected:

```text
name = "iamuser_james"
```

---

## Verify Using AWS CLI

```bash
aws iam get-user --user-name iamuser_james
```

Expected output:

```text
User details displayed
```

---

# Terraform Concepts Learned

## Variables

Variables allow configuration values to be managed separately from infrastructure code.

Example:

```hcl
variable "KKE_user" {
  default = "iamuser_james"
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
var.KKE_user
```

---

## IAM User Resource

```hcl
resource "aws_iam_user" "this" {
}
```

Breakdown:

```text
aws_iam_user → Resource Type
this         → Resource Name
```

---

## Assigning Variable Values

```hcl
name = var.KKE_user
```

This assigns:

```text
iamuser_james
```

as the IAM user name.

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_iam_user.this
```

Verify IAM User:

```bash
aws iam get-user --user-name iamuser_james
```

---

# 🎯 Outcome

Successfully created:

* IAM User: `iamuser_james`
* Variable: `KKE_user`
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
variable "KKE_user" {
  default = "iamuser_james"
}
```

and reference it as:

```hcl
name = var.KKE_user
```

---

# 🚀 Key Takeaway

This task demonstrates how Terraform variables can be used to separate configuration values from infrastructure code. Using `variables.tf` improves maintainability, reusability, and makes Terraform configurations easier to manage across environments.
