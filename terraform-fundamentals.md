# Terraform Fundamentals - Complete Beginner Reference Guide

# 📌 What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool developed by HashiCorp that allows you to create, update, and destroy infrastructure using configuration files instead of manually creating resources from the AWS Console.

Example:

Instead of creating:

* EC2 Instance
* VPC
* IAM User
* S3 Bucket

manually through AWS Console, Terraform does it automatically.

---

# Terraform Workflow

```text
Write Code
    ↓
terraform init
    ↓
terraform validate
    ↓
terraform plan
    ↓
terraform apply
    ↓
Infrastructure Created
```

---

# Basic Terraform File Structure

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

# Terraform Block Types

## Provider Block

Tells Terraform which cloud provider to use.

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Examples:

```hcl
provider "aws"
provider "azure"
provider "google"
```

---

## Resource Block

Used to create infrastructure.

Syntax:

```hcl
resource "<resource_type>" "<resource_name>" {
}
```

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

# Resource Structure

```hcl
resource "aws_instance" "web" {
}
```

Breakdown:

```text
resource
│
├── aws_instance   → Resource Type
│
└── web            → Resource Name
```

---

# Resource Type

Defined by the provider.

Examples:

```hcl
aws_instance
aws_vpc
aws_subnet
aws_s3_bucket
aws_iam_user
aws_iam_group
aws_iam_role
aws_ebs_volume
```

Cannot be changed.

---

# Resource Name

User-defined.

Examples:

```hcl
resource "aws_instance" "web" {}
resource "aws_instance" "server" {}
resource "aws_instance" "myec2" {}
resource "aws_instance" "this" {}
```

All are valid.

---

# Why Resource Name is Required?

Terraform uses it to reference resources.

Example:

```hcl
resource "aws_vpc" "main" {
}
```

Reference:

```hcl
aws_vpc.main.id
```

---

# Resource Reference Structure

Syntax:

```text
resource_type.resource_name.attribute
```

Example:

```hcl
aws_vpc.main.id
```

Breakdown:

```text
aws_vpc      → Resource Type
main         → Resource Name
id           → Attribute
```

---

# What is an Attribute?

Attributes are values returned by a resource after creation.

Example EC2:

```hcl
resource "aws_instance" "web" {
}
```

Terraform generates:

```text
id
arn
public_ip
private_ip
availability_zone
subnet_id
instance_type
```

These can be referenced.

---

# Most Common Attributes

## .id

Most frequently used.

Example:

```hcl
aws_vpc.main.id
```

Value:

```text
vpc-12345678
```

Used when AWS expects:

```text
vpc_id
subnet_id
volume_id
instance_id
```

Example:

```hcl
vpc_id = aws_vpc.main.id
```

---

## .arn

ARN = Amazon Resource Name

Example:

```hcl
aws_iam_policy.policy.arn
```

Value:

```text
arn:aws:iam::123456789012:policy/test
```

Used when AWS expects:

```text
policy_arn
topic_arn
role_arn
```

Example:

```hcl
policy_arn = aws_iam_policy.policy.arn
```

---

## .name

Returns resource name.

Example:

```hcl
aws_iam_user.user.name
```

Value:

```text
john
```

Used when AWS expects:

```text
user
group
role
```

Example:

```hcl
user = aws_iam_user.user.name
```

---

## .public_ip

Example:

```hcl
aws_instance.web.public_ip
```

Value:

```text
54.32.12.10
```

---

## .private_ip

Example:

```hcl
aws_instance.web.private_ip
```

Value:

```text
10.0.1.15
```

---

## .availability_zone

Example:

```hcl
aws_instance.web.availability_zone
```

Value:

```text
us-east-1a
```

---

## .bucket

Example:

```hcl
aws_s3_bucket.my_bucket.bucket
```

Value:

```text
my-bucket
```

---

# What is ".this"?

Many beginners think `.this` is an attribute.

It is NOT.

Example:

```hcl
resource "aws_iam_role" "this" {
}
```

Breakdown:

```text
aws_iam_role  → Resource Type
this          → Resource Name
```

Terraform Reference:

```hcl
aws_iam_role.this.arn
```

Here:

```text
this = Resource Name
arn  = Attribute
```

---

# Why Use "this"?

It is a Terraform convention.

Common in reusable modules.

Example:

```hcl
resource "aws_vpc" "this" {}
resource "aws_subnet" "this" {}
resource "aws_iam_role" "this" {}
```

Equivalent to:

```hcl
resource "aws_vpc" "main" {}
resource "aws_vpc" "nautilus_vpc" {}
resource "aws_vpc" "prod" {}
```

Only the name changes.

---

# Terraform State

Terraform tracks all resources inside a state file.

Check:

```bash
terraform state list
```

Example:

```text
aws_instance.web
aws_vpc.main
aws_iam_role.this
```

---

# Inspect Resource State

```bash
terraform state show aws_instance.web
```

Output:

```text
id = i-12345
public_ip = 54.12.34.56
private_ip = 10.0.1.15
```

Useful to discover available attributes.

---

# Data Sources

Used to fetch existing resources.

Example:

```hcl
data "aws_instance" "server" {
  filter {
    name   = "tag:Name"
    values = ["devops-ec2"]
  }
}
```

Reference:

```hcl
data.aws_instance.server.id
```

---

# Resource vs Data Source

## Resource

Creates infrastructure.

```hcl
resource "aws_instance" "web" {
}
```

---

## Data Source

Reads existing infrastructure.

```hcl
data "aws_instance" "web" {
}
```

---

# Implicit Dependencies

Terraform automatically understands dependencies.

Example:

```hcl
resource "aws_vpc" "main" {
}

resource "aws_subnet" "public" {
  vpc_id = aws_vpc.main.id
}
```

Terraform creates:

```text
VPC
 ↓
Subnet
```

Automatically.

---

# Variables

Example:

```hcl
variable "instance_type" {
  default = "t2.micro"
}
```

Usage:

```hcl
instance_type = var.instance_type
```

---

# Outputs

Used to display values.

Example:

```hcl
output "public_ip" {
  value = aws_instance.web.public_ip
}
```

After apply:

```text
public_ip = 54.12.34.56
```

---

# Common Terraform Commands

Initialize:

```bash
terraform init
```

Validate:

```bash
terraform validate
```

Plan:

```bash
terraform plan
```

Apply:

```bash
terraform apply
```

Auto Approve:

```bash
terraform apply -auto-approve
```

Destroy:

```bash
terraform destroy
```

Targeted Destroy:

```bash
terraform destroy -target=aws_instance.web
```

Show State:

```bash
terraform state list
```

Inspect Resource:

```bash
terraform state show aws_instance.web
```

---

# Quick Rule for Labs

If argument ends with:

```text
*_id
```

Use:

```hcl
resource.id
```

Example:

```hcl
vpc_id = aws_vpc.main.id
```

---

If argument ends with:

```text
*_arn
```

Use:

```hcl
resource.arn
```

Example:

```hcl
policy_arn = aws_iam_policy.policy.arn
```

---

If argument asks for:

```text
name
```

Use:

```hcl
resource.name
```

Example:

```hcl
user = aws_iam_user.user.name
```

---

# Final Summary

Terraform references follow this structure:

```text
resource_type.resource_name.attribute
```

Example:

```hcl
aws_instance.web.id
aws_instance.web.public_ip
aws_vpc.main.id
aws_iam_policy.policy.arn
aws_iam_user.user.name
aws_iam_role.this.arn
```

Remember:

```text
Resource Type  → What to create
Resource Name  → What Terraform calls it
Attribute      → Value returned by AWS
```

This single concept explains most Terraform references used in AWS infrastructure provisioning.
