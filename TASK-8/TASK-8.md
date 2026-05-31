# Terraform Level-1 Task-08: Create AMI from Existing EC2 Instance

## 📌 Task Objective

Create an Amazon Machine Image (AMI) from an existing EC2 instance using Terraform.

### Requirements

* Source EC2 Instance: `datacenter-ec2`
* AMI Name: `datacenter-ec2-ami`
* AMI must reach **available** state
* Region: `us-east-1`

Terraform working directory:

```bash id="2b7dka"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="j8r7za"
cd /home/bob/terraform
```

---

## Step 2: Update `main.tf`

Add this Terraform configuration:

```hcl id="6r9mqp"
provider "aws" {
  region = "us-east-1"
}

data "aws_instance" "datacenter_ec2" {
  filter {
    name   = "tag:Name"
    values = ["datacenter-ec2"]
  }
}

resource "aws_ami_from_instance" "datacenter_ami" {
  name               = "datacenter-ec2-ami"
  source_instance_id = data.aws_instance.datacenter_ec2.id
}
```

---

## Step 3: Initialize Terraform

```bash id="h1pk3x"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="j3t8wy"
terraform validate
```

Expected output:

```bash id="f7n9sl"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="l6wq8d"
terraform apply -auto-approve
```

---

## Step 6: Verify AMI Creation

```bash id="n4u2yc"
aws ec2 describe-images --owners self --region us-east-1
```

Verify:

* AMI Name → `datacenter-ec2-ami`
* State → `available`

---

## 🔍 Key Concepts Learned

### Data Source

Used to fetch an existing EC2 instance.

```hcl id="g7p3mb"
data "aws_instance"
```

---

### AMI Creation Resource

Creates an image from an existing instance.

```hcl id="x9c5wa"
resource "aws_ami_from_instance"
```

---

### Source Instance Reference

Links AMI creation to the existing EC2 instance.

---

### Terraform Resource Dependency

Terraform automatically creates the AMI after locating the source instance.

---

## ✅ Verification Command

```bash id="m8v1rp"
aws ec2 describe-images --owners self --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AMI from existing EC2 instance
* AMI Name: `datacenter-ec2-ami`
* Region: `us-east-1`
* AMI reached available state

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Terraform data sources
* AWS AMI creation
* Existing resource referencing
* Infrastructure image management

This task introduces machine image automation, an essential concept for scalable infrastructure provisioning and reusable deployments.
