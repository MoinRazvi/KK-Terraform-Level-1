# Terraform Level-1 Task-03: Create a VPC

## 📌 Task Objective

Create an AWS VPC with:

* **Name:** `xfusion-vpc`
* **Region:** `us-east-1`
* **IPv4 CIDR Block:** Any valid private CIDR block

Terraform working directory:

```bash id="6tl0vk"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="2d8q92"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Use this exact Terraform configuration:

```hcl id="mxn5f8"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "xfusion-vpc"
  }
}
```

Why `10.0.0.0/16`?

It is a valid private IPv4 CIDR block commonly used for VPCs.

---

## Step 3: Initialize Terraform

```bash id="d32p6j"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="w0x8fa"
terraform validate
```

Expected output:

```bash id="x2ub4q"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="t12b45"
terraform apply -auto-approve
```

---

## Step 6: Verify VPC Creation

```bash id="q5z7pl"
aws ec2 describe-vpcs --region us-east-1
```

Look for:

* VPC Name: `xfusion-vpc`
* CIDR: `10.0.0.0/16`

---

## ⚠ Common Issues & Fixes

### Issue 1: Missing Tags Block

Wrong:

```hcl id="shmzsw"
name = "xfusion-vpc"
```

Correct:

```hcl id="a4rk0m"
tags = {
  Name = "xfusion-vpc"
}
```

AWS uses **tags** for naming resources.

---

### Issue 2: Invalid CIDR Block

Wrong:

```hcl id="1qz4ei"
cidr_block = "10.0.0.0"
```

Correct:

```hcl id="g4j1s6"
cidr_block = "10.0.0.0/16"
```

CIDR must include subnet mask.

---

### Issue 3: Wrong Region

Ensure:

```hcl id="gpcf6e"
region = "us-east-1"
```

---

## ✅ Key Concepts Learned

### Provider Block

Defines cloud provider and region.

### Resource Block

Creates AWS infrastructure resource.

### CIDR Block

Defines private IP address range for the VPC.

### Tags

Used to assign names and metadata.

---

## 🎯 Outcome

Successfully created:

✔ AWS VPC
✔ Named `xfusion-vpc`
✔ Deployed in `us-east-1`
✔ Provisioned using Terraform Infrastructure as Code

This task strengthens understanding of foundational AWS networking automation using Terraform.
