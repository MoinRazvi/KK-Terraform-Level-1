## Terraform Level-1 Task-05: Create VPC with Amazon-Provided IPv6 CIDR

### 📌 Task Objective

Create an AWS VPC with:

* **Name:** `nautilus-vpc`
* **Region:** `us-east-1`
* **IPv6:** Amazon-provided IPv6 CIDR block

Terraform working directory:

```bash id="1fmp7l"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="14t4th"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Use this exact Terraform configuration:

```hcl id="6vnn3x"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "nautilus_vpc" {
  assign_generated_ipv6_cidr_block = true
  cidr_block                       = "10.0.0.0/16"

  tags = {
    Name = "nautilus-vpc"
  }
}
```

---

## Step 3: Initialize Terraform

```bash id="spv0w9"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="r8qv5j"
terraform validate
```

---

## Step 5: Apply Configuration

```bash id="3nvkbe"
terraform apply -auto-approve
```

---

## Step 6: Verify VPC Creation

```bash id="v0r4oz"
aws ec2 describe-vpcs --region us-east-1
```

Verify:

* VPC Name → `nautilus-vpc`
* IPv6 CIDR assigned
* Region → `us-east-1`

---

## ⚠ Common Issues & Solutions

### Issue 1: Missing IPv6 Assignment Attribute

Wrong:

```hcl id="stx7d9"
resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"
}
```

Correct:

```hcl id="y35w5s"
assign_generated_ipv6_cidr_block = true
```

**Reason:**
This enables AWS to automatically allocate an Amazon-provided IPv6 CIDR block.

---

### Issue 2: Missing IPv4 CIDR

Wrong:

```hcl id="vnpqef"
assign_generated_ipv6_cidr_block = true
```

Correct:

```hcl id="dk7j4n"
cidr_block = "10.0.0.0/16"
```

**Reason:**
AWS VPC requires an IPv4 CIDR even when enabling IPv6.

---

### Issue 3: Incorrect Naming Method

Wrong:

```hcl id="ic7c9j"
name = "nautilus-vpc"
```

Correct:

```hcl id="w4l8h2"
tags = {
  Name = "nautilus-vpc"
}
```

---

## 🔍 Key Concepts Learned

### Amazon-Provided IPv6 CIDR

AWS can automatically allocate an IPv6 CIDR block when:

```hcl id="95t8r5"
assign_generated_ipv6_cidr_block = true
```

---

### Dual Stack Networking

This VPC supports:

* IPv4
* IPv6

This is called **dual-stack networking**

---

### VPC Resource

Creates isolated AWS network infrastructure.

---

### Tags

Used for naming and identification.

---

## 🎯 Outcome

Successfully created:

✔ AWS VPC
✔ Named `nautilus-vpc`
✔ Region `us-east-1`
✔ IPv4 CIDR assigned
✔ Amazon-generated IPv6 CIDR assigned
✔ Managed via Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* IPv6-enabled VPC provisioning
* Dual-stack cloud networking
* Terraform advanced VPC configuration
* AWS automated IPv6 allocation

Task-05 introduces modern cloud networking concepts by combining Terraform automation with Amazon-provided IPv6 addressing.
