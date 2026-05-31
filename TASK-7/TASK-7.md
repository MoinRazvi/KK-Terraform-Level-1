# Terraform Level-1 Task-07: Create EC2 Instance with RSA Key Pair

## 📌 Task Objective

Provision an AWS EC2 instance using Terraform with the following requirements:

* **Instance Name:** `xfusion-ec2`
* **AMI:** `ami-0c101f26f147fa7fd`
* **Instance Type:** `t2.micro`
* **Key Pair:** `xfusion-kp` (RSA)
* **Security Group:** Default security group
* **Region:** `us-east-1`

Terraform working directory:

```bash id="c8ifmu"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="7zq8ku"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Add this Terraform configuration:

```hcl id="q0c76x"
provider "aws" {
  region = "us-east-1"
}

resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_key.public_key_openssh
}

data "aws_security_group" "default" {
  name = "default"
}

resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

---

## Step 3: Initialize Terraform

```bash id="4x45aq"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="q3ljzs"
terraform validate
```

Expected output:

```bash id="6x55ka"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="x8j8ml"
terraform apply -auto-approve
```

---

## Step 6: Verify EC2 Instance Creation

```bash id="jpw31l"
aws ec2 describe-instances --region us-east-1
```

Verify:

* Instance Name → `xfusion-ec2`
* Instance Type → `t2.micro`
* Key Pair → `xfusion-kp`

---

## ⚠ Issues Faced & Solutions

### Issue 1: Missing Key Pair Resource

### Problem

Attempted to directly assign:

```hcl id="0v2u8q"
key_name = "xfusion-kp"
```

without creating the key pair.

### Solution

Created the RSA key pair:

```hcl id="slnh15"
resource "tls_private_key" "xfusion_key"
resource "aws_key_pair" "xfusion_kp"
```

### Learning

Terraform must provision the key pair before attaching it to EC2.

---

### Issue 2: Security Group Not Explicitly Referenced

### Problem

The task required attaching the default security group.

### Solution

Fetched it using:

```hcl id="9jlwm6"
data "aws_security_group" "default"
```

and attached it using:

```hcl id="yxg5px"
vpc_security_group_ids
```

### Learning

Existing AWS resources can be referenced using Terraform data sources.

---

### Issue 3: Incorrect Resource Naming

### Problem

Used:

```hcl id="kt2xpc"
name = "xfusion-ec2"
```

### Solution

Correct naming uses tags:

```hcl id="h3r9vx"
tags = {
  Name = "xfusion-ec2"
}
```

---

## 🔍 Key Concepts Learned

### EC2 Instance Resource

Creates a virtual machine in AWS.

---

### TLS Private Key

Generates an RSA private/public key pair.

---

### AWS Key Pair

Registers the generated public key in AWS.

---

### Data Source

Fetches existing AWS resources.

---

### Security Group Attachment

Controls inbound and outbound traffic rules.

---

## ✅ Verification Command

```bash id="vfobui"
aws ec2 describe-instances --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS EC2 instance
* Name: `xfusion-ec2`
* Key pair: `xfusion-kp`
* Attached default security group
* Deployed in `us-east-1`

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* EC2 provisioning using Terraform
* RSA key generation
* Security group attachment
* Infrastructure dependency management

This task introduced complete compute provisioning using Terraform by combining key management, networking, and EC2 deployment.
