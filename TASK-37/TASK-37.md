# Terraform Level-1 Task-37: Create AWS Elastic IP Using Variables

## 📌 Task Objective

Create an AWS Elastic IP using Terraform while storing configuration values in a separate variables file.

### Requirements

* Elastic IP Name: `devops-eip`
* Variable Name: `KKE_eip`
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
variable "KKE_eip" {
  default = "devops-eip"
}
```

---

## main.tf

```hcl
resource "aws_eip" "this" {
  domain = "vpc"

  tags = {
    Name = var.KKE_eip
  }
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
aws_eip.this
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
aws_eip.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_eip.this
```

Expected:

```text
public_ip = x.x.x.x

tags = {
  Name = "devops-eip"
}
```

---

## Verify Using AWS CLI

```bash
aws ec2 describe-addresses \
--filters "Name=tag:Name,Values=devops-eip" \
--region us-east-1
```

Expected:

```text
Elastic IP details displayed
```

---

# Terraform Concepts Learned

## Variables

Variables allow configuration values to be managed separately from infrastructure code.

Example:

```hcl
variable "KKE_eip" {
  default = "devops-eip"
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
var.KKE_eip
```

---

## Elastic IP Resource

```hcl
resource "aws_eip" "this" {
}
```

Breakdown:

```text
aws_eip → Resource Type
this    → Resource Name
```

---

## Tag Assignment Using Variables

```hcl
tags = {
  Name = var.KKE_eip
}
```

This assigns:

```text
Name = devops-eip
```

to the Elastic IP using the variable value.

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_eip.this
```

Verify Elastic IP:

```bash
aws ec2 describe-addresses \
--filters "Name=tag:Name,Values=devops-eip" \
--region us-east-1
```

---

# 🎯 Outcome

Successfully created:

* Elastic IP: `devops-eip`
* Variable: `KKE_eip`
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
variable "KKE_eip" {
  default = "devops-eip"
}
```

and reference it as:

```hcl
tags = {
  Name = var.KKE_eip
}
```

---

# 🚀 Key Takeaway

This task demonstrates how Terraform variables can be used to separate configuration values from infrastructure code. Using `variables.tf` improves maintainability, reusability, and makes Terraform configurations easier to manage across environments.
