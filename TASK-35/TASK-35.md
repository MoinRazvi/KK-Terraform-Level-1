# Terraform Level-1 Task-35: Create AWS VPC Using Variables

## 📌 Task Objective

Create an AWS VPC using Terraform while storing configuration values in a separate variables file.

### Requirements

* VPC Name: `datacenter-vpc`
* Variable Name: `KKE_vpc`
* CIDR Block: `10.0.0.0/16`
* Store variables in `variables.tf`
* Reference variables from `main.tf`

Terraform working directory:

```bash
/home/bob/terraform
```

---

# Solution Structure

## variables.tf

```hcl
variable "KKE_vpc" {
  default = "datacenter-vpc"
}

variable "vpc_cidr" {
  default = "10.0.0.0/16"
}
```

---

## main.tf

```hcl
resource "aws_vpc" "this" {
  cidr_block = var.vpc_cidr

  tags = {
    Name = var.KKE_vpc
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
aws_vpc.this
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
aws_vpc.this
```

---

## Inspect Resource Details

```bash
terraform state show aws_vpc.this
```

Expected values:

```text
cidr_block = 10.0.0.0/16

tags = {
  Name = "datacenter-vpc"
}
```

---

## Verify Using AWS CLI

```bash
aws ec2 describe-vpcs \
--filters "Name=tag:Name,Values=datacenter-vpc" \
--region us-east-1
```

Expected:

```text
VPC details displayed
```

---

# Terraform Concepts Learned

## Variables

Variables allow configuration values to be reused and managed separately from infrastructure code.

Example:

```hcl
variable "KKE_vpc" {
  default = "datacenter-vpc"
}
```

---

## Variable Reference

Terraform variables are referenced using:

```hcl
var.<variable_name>
```

Examples:

```hcl
var.KKE_vpc
var.vpc_cidr
```

---

## Resource Creation

```hcl
resource "aws_vpc" "this" {
}
```

Breakdown:

```text
aws_vpc → Resource Type
this    → Resource Name
```

---

## Tag Assignment Using Variables

```hcl
tags = {
  Name = var.KKE_vpc
}
```

This assigns:

```text
Name = datacenter-vpc
```

to the VPC using the variable value.

---

# Verification Commands

Check Terraform State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_vpc.this
```

Verify VPC:

```bash
aws ec2 describe-vpcs \
--filters "Name=tag:Name,Values=datacenter-vpc" \
--region us-east-1
```

---

# 🎯 Outcome

Successfully created:

* VPC: `datacenter-vpc`
* CIDR Block: `10.0.0.0/16`
* Variable: `KKE_vpc`
* Terraform-managed infrastructure

using a modular structure with `main.tf` and `variables.tf`.

---

# ⚠ Issues Faced & Solution

## Issue

VPC not visible when filtering using AWS CLI.

Example:

```bash
aws ec2 describe-vpcs \
--filters "Name=tag:Name,Values=datacenter-vpc" \
--region us-east-1
```

Output:

```text
Vpcs: []
```

## Verification

Check Terraform state:

```bash
terraform state list
```

Inspect resource:

```bash
terraform state show aws_vpc.this
```

## Solution

Verify:

1. Terraform apply completed successfully.
2. VPC exists in Terraform state.
3. AWS CLI filter syntax is correct.
4. Region is set to `us-east-1`.

---

# 🚀 Key Takeaway

This task demonstrates how to separate configuration values from infrastructure code using Terraform variables. Storing values in `variables.tf` improves reusability, maintainability, and makes infrastructure definitions more flexible across environments.
