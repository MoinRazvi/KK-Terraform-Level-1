## Terraform Level-1 Task-04: Create VPC with Specific CIDR

### 📌 Task Objective

Create an AWS VPC with:

* **Name:** `xfusion-vpc`
* **Region:** `us-east-1`
* **CIDR Block:** `192.168.0.0/24`

Terraform working directory:

```bash id="iyw80m"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="jv7zlf"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Add this exact Terraform code:

```hcl id="wqqs41"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "xfusion-vpc"
  }
}
```

---

## Step 3: Initialize Terraform

```bash id="hygrvi"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="m57rhz"
terraform validate
```

---

## Step 5: Apply Configuration

```bash id="j2gqne"
terraform apply -auto-approve
```

---

## Step 6: Verify VPC Creation

```bash id="mb53w0"
aws ec2 describe-vpcs --region us-east-1
```

Verify:

* VPC Name → `xfusion-vpc`
* CIDR → `192.168.0.0/24`

---

## ⚠ Common Issues & Solutions

### Issue 1: Incorrect CIDR Block

Wrong:

```hcl id="x5f30s"
cidr_block = "192.168.0.0"
```

Correct:

```hcl id="ehz3pk"
cidr_block = "192.168.0.0/24"
```

**Reason:** CIDR must include subnet mask.

---

### Issue 2: Incorrect Resource Naming

Wrong:

```hcl id="m4rrja"
name = "xfusion-vpc"
```

Correct:

```hcl id="xfg43e"
tags = {
  Name = "xfusion-vpc"
}
```

AWS resource names are assigned using tags.

---

### Issue 3: Wrong AWS Region

Ensure:

```hcl id="7aq3nq"
region = "us-east-1"
```

---

## 🔍 Key Concepts Learned

### Provider Block

Specifies AWS region for deployment.

---

### Resource Block

Creates infrastructure resource.

---

### CIDR Block

Defines available IP address range for the VPC.

`192.168.0.0/24` provides **256 IP addresses**

---

### Tags

Used to label and identify AWS resources.

---

## 🎯 Outcome

Successfully created:

✔ AWS VPC
✔ Named `xfusion-vpc`
✔ Region `us-east-1`
✔ CIDR `192.168.0.0/24`
✔ Managed using Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* CIDR block assignment
* VPC provisioning
* Terraform AWS resource creation
* Infrastructure as Code fundamentals

Task-04 reinforces precision in Terraform networking configurations, especially when exact CIDR ranges are required.
