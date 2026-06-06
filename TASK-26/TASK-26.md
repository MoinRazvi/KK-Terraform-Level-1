# Terraform Level-1 Task-26: Attach Elastic IP to EC2 Instance

## 📌 Task Objective

Attach an existing Elastic IP to an EC2 instance using Terraform.

### Requirements

* EC2 Instance Name: `xfusion-ec2`
* Elastic IP Name: `xfusion-ec2-eip`
* Region: `us-east-1`
* Use Terraform only
* Update the existing `main.tf`

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

## Step 2: Update `main.tf`

The existing Terraform configuration already contained:

* EC2 Instance
* Elastic IP

To attach the Elastic IP to the instance, add the following line inside the `aws_eip` resource:

```hcl
instance = aws_instance.ec2.id
```

Final configuration:

```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-3f72ac5b0c0292b72"

  vpc_security_group_ids = [
    "sg-1367d4f0afb3b98e4"
  ]

  tags = {
    Name = "xfusion-ec2"
  }
}

# Provision Elastic IP and attach to EC2
resource "aws_eip" "ec2_eip" {
  instance = aws_instance.ec2.id

  tags = {
    Name = "xfusion-ec2-eip"
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

## Step 5: Review Changes

```bash
terraform plan
```

Verify Terraform plans to associate the Elastic IP with the EC2 instance.

---

## Step 6: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 7: Verify Elastic IP Association

```bash
aws ec2 describe-addresses \
--region us-east-1
```

Verify:

* Elastic IP → `xfusion-ec2-eip`
* Associated Instance → `xfusion-ec2`

---

## 🔍 Key Concepts Learned

### Elastic IP (EIP)

An Elastic IP is a static public IPv4 address that can be associated with an EC2 instance.

Benefits:

* Persistent public IP
* Easy failover
* Stable endpoint for applications

---

### Direct EIP Association

```hcl
instance = aws_instance.ec2.id
```

Associates the Elastic IP directly with the EC2 instance.

---

### Resource References

Terraform resources can reference other resources using:

```hcl
aws_instance.ec2.id
```

This creates an automatic dependency between resources.

---

### Terraform Dependency Management

Terraform automatically:

1. Creates the EC2 instance.
2. Allocates the Elastic IP.
3. Associates the Elastic IP with the instance.

No manual dependency configuration is required.

---

## ✅ Verification Commands

```bash
aws ec2 describe-addresses --region us-east-1
```

```bash
terraform state list
```

Expected resources:

```bash
aws_instance.ec2
aws_eip.ec2_eip
```

---

## 🎯 Outcome

Successfully attached:

* EC2 Instance: `xfusion-ec2`
* Elastic IP: `xfusion-ec2-eip`
* Terraform-managed network association

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Elastic IP management
* EC2 networking
* Terraform resource references
* Infrastructure dependency management

This task demonstrates how Terraform can directly associate networking resources with compute resources using resource references, simplifying infrastructure management and ensuring consistent deployments.

---

## ⚠ Issues Faced & Solution

### Issue

Initially attempted to use:

```hcl
data "aws_instance"
data "aws_eip"
resource "aws_eip_association"
```

to associate the Elastic IP.

### Root Cause

The EC2 instance and Elastic IP were already managed by Terraform resources in the existing configuration.

### Solution

Used direct resource reference:

```hcl
instance = aws_instance.ec2.id
```

inside the `aws_eip` resource.

This allowed Terraform to automatically associate the Elastic IP with the EC2 instance without requiring additional data sources or association resources.
