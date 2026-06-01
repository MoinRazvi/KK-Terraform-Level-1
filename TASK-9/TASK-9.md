# Terraform Level-1 Task-09: Create AWS EBS Volume

## 📌 Task Objective

Create an AWS EBS volume using Terraform with the following requirements:

* **Volume Name:** `xfusion-volume`
* **Volume Type:** `gp3`
* **Volume Size:** `2 GiB`
* **Region:** `us-east-1`

Terraform working directory:

```bash id="2xk8ma"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="j4r8xp"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Add this Terraform configuration:

```hcl id="n6v4cz"
provider "aws" {
  region = "us-east-1"
}

resource "aws_ebs_volume" "xfusion_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"

  tags = {
    Name = "xfusion-volume"
  }
}
```

---

## Step 3: Initialize Terraform

```bash id="m2y9pk"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="u8q5jw"
terraform validate
```

Expected output:

```bash id="r3n7vb"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="x5l1df"
terraform apply -auto-approve
```

---

## Step 6: Verify EBS Volume Creation

```bash id="p7t6ko"
aws ec2 describe-volumes --region us-east-1
```

Verify:

* Volume Name → `xfusion-volume`
* Volume Type → `gp3`
* Size → `2 GiB`

---

## 🔍 Key Concepts Learned

### EBS Volume

Amazon Elastic Block Store (EBS) provides block-level storage for EC2 instances.

It is commonly used for:

* Persistent storage
* Application data
* Databases
* File systems

---

### Volume Type

```hcl id="s4v9xb"
type = "gp3"
```

`gp3` is a general-purpose SSD volume offering:

* Better performance
* Cost efficiency
* Flexible IOPS configuration

---

### Volume Size

```hcl id="k6j3yt"
size = 2
```

Defines storage capacity in GiB.

---

### Availability Zone

```hcl id="e2w8rn"
availability_zone = "us-east-1a"
```

EBS volumes must be created in a specific Availability Zone.

---

### Tags

Used for resource naming and management.

---

## ✅ Verification Command

```bash id="h1p9vu"
aws ec2 describe-volumes --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS EBS Volume
* Name: `xfusion-volume`
* Type: `gp3`
* Size: `2 GiB`
* Region: `us-east-1`

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS block storage provisioning
* Terraform storage resource creation
* Availability Zone dependency
* EBS configuration fundamentals

This task introduces persistent storage automation using Terraform, an essential concept for cloud infrastructure provisioning.
