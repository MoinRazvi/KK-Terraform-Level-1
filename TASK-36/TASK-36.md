# Terraform Level-1 Task-36: Create AWS Security Group Using Variables

## 📌 Task Objective

Create an AWS Security Group using Terraform while storing configuration values in a separate variables file.

### Requirements

* Security Group Name: `xfusion-sg`
* Variable Name: `KKE_sg`
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
variable "KKE_sg" {
  default = "xfusion-sg"
}
```

---

## main.tf

```hcl
resource "aws_security_group" "this" {
  name = var.KKE_sg
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
aws_security_group.this
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
aws_security_group.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_security_group.this
```

Expected:

```text
name = "xfusion-sg"
```

---

## Verify Using AWS CLI

```bash
aws ec2 describe-security-groups \
--filters "Name=group-name,Values=xfusion-sg" \
--region us-east-1
```

Expected:

```text
Security Group details displayed
```

---

# Terraform Concepts Learned

## Variables

Variables allow configuration values to be managed separately from infrastructure code.

Example:

```hcl
variable "KKE_sg" {
  default = "xfusion-sg"
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
var.KKE_sg
```

---

## Resource Creation

```hcl
resource "aws_security_group" "this" {
}
```

Breakdown:

```text
aws_security_group → Resource Type
this               → Resource Name
```

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_security_group.this
```

Verify Security Group:

```bash
aws ec2 describe-security-groups \
--filters "Name=group-name,Values=xfusion-sg" \
--region us-east-1
```

---

# 🎯 Outcome

Successfully created:

* Security Group: `xfusion-sg`
* Variable: `KKE_sg`
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
variable "KKE_sg" {
  default = "xfusion-sg"
}
```

and reference it as:

```hcl
name = var.KKE_sg
```

---

# 🚀 Key Takeaway

This task demonstrates how Terraform variables can be used to separate configuration values from infrastructure code. Using `variables.tf` improves maintainability, reusability, and makes Terraform configurations easier to manage across environments.
