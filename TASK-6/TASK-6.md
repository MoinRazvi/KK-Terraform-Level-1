# Terraform Level-1 Task-06: Allocate Elastic IP Address

## 📌 Task Objective

Allocate an AWS Elastic IP address named **xfusion-eip** using Terraform.

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

Create the Terraform configuration:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eip" "xfusion_eip" {
  tags = {
    Name = "xfusion-eip"
  }
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

## Step 6: Verify Elastic IP Allocation

```bash
aws ec2 describe-addresses --region us-east-1
```

Verify:

* Elastic IP allocated successfully
* Resource name: **xfusion-eip**

---

## ⚠ Issues Faced & Solutions

### Issue 1: Adding Unnecessary Parameters

### Problem

Initially added:

```hcl
domain = "vpc"
```

### Why It Was Not Required

The lab only requested:

* Allocate an Elastic IP
* Name it `xfusion-eip`

No VPC-specific configuration was mentioned.

### Solution

Removed the unnecessary parameter.

Correct configuration:

```hcl
resource "aws_eip" "xfusion_eip" {
  tags = {
    Name = "xfusion-eip"
  }
}
```

### Learning

For Nautilus Terraform labs, always stick to the exact task requirements unless Terraform validation requires additional configuration.

---

### Issue 2: Incorrect Resource Naming

### Problem

Used:

```hcl
name = "xfusion-eip"
```

### Solution

AWS resource naming is done through tags:

```hcl
tags = {
  Name = "xfusion-eip"
}
```

### Learning

AWS resources are commonly identified using the **Name tag**.

---

### Issue 3: Missing AWS Region

### Problem

Provider region was omitted.

### Solution

Added:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Learning

Terraform requires region configuration for AWS resource provisioning.

---

## 🔍 Key Concepts Learned

### Elastic IP (EIP)

An Elastic IP is a static public IPv4 address provided by AWS.

It is commonly used for:

* Stable public access
* Server endpoint consistency
* Infrastructure remapping

---

### Resource Block

Defines the AWS infrastructure component to create.

---

### Tags

Used for naming and organizing AWS resources.

---

### Provider Block

Specifies the target AWS region.

---

## ✅ Verification Command

```bash
aws ec2 describe-addresses --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS Elastic IP
* Named **xfusion-eip**
* Deployed in **us-east-1**
* Managed using Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Elastic IP provisioning
* AWS public networking
* Terraform resource creation
* Writing minimal task-specific Terraform configurations

This task reinforced the importance of following lab requirements precisely and avoiding unnecessary configuration additions.
