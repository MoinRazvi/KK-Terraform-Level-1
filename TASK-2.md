## Terraform Level-1 Task-02: Create AWS Security Group in Default VPC

### 📌 Task Objective

The Nautilus DevOps team is gradually migrating infrastructure to AWS using Terraform.

The objective of this task was to create an AWS Security Group under the **default VPC** in the **us-east-1** region with the following requirements:

* Create a security group named **devops-sg**
* Add description:
  **Security group for Nautilus App Servers**
* Allow inbound **HTTP** traffic on port **80**
* Allow inbound **SSH** traffic on port **22**
* Source CIDR for both rules:
  **0.0.0.0/0**
* Use **Terraform**
* Create everything inside `main.tf`

---

## 🛠 Environment Setup

### Navigate to Terraform Working Directory

```bash
cd /home/bob/terraform
```

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Apply Configuration

```bash
terraform apply -auto-approve
```

---

## 🏗 Terraform Configuration (`main.tf`)

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "devops_sg" {
  name        = "devops-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

## ⚠ Issues Faced & Solutions

### Issue 1: Incorrect Terraform Attribute Syntax

### Problem

Used:

```hcl
from port = 80
```

### Error

Terraform validation failed due to invalid argument syntax.

### Solution

Corrected to:

```hcl
from_port = 80
```

### Learning

Terraform uses **underscore-based attribute naming**, not spaces.

---

### Issue 2: Missing Default VPC Association

### Problem

Security group was defined without explicitly specifying the VPC.

### Why This Matters

The lab required the security group to be created **under the default VPC**.

### Solution

Added:

```hcl
data "aws_vpc" "default" {
  default = true
}

vpc_id = data.aws_vpc.default.id
```

### Learning

Terraform must explicitly know which VPC resource to target.

---

### Issue 3: Unnecessary Egress Rule

### Problem

Initially added an outbound (egress) rule.

### Why It Was Incorrect

The lab only required **inbound rules**.

### Solution

Removed the egress block.

### Learning

AWS Security Groups allow outbound traffic by default unless explicitly modified.

---

## 🔍 Key Terraform Concepts Learned

### Provider Block

Defines the cloud provider and region.

```hcl
provider "aws"
```

---

### Data Source

Fetches existing AWS resources.

```hcl
data "aws_vpc" "default"
```

---

### Resource Block

Creates infrastructure components.

```hcl
resource "aws_security_group"
```

---

### Ingress Rules

Defines inbound traffic permissions.

* HTTP → Port 80
* SSH → Port 22

---

## ✅ Verification

Verify security group creation:

```bash
aws ec2 describe-security-groups --region us-east-1 --group-names devops-sg
```

---

## 🎯 Outcome

Successfully created:

* AWS Security Group
* Attached to Default VPC
* HTTP Access Enabled
* SSH Access Enabled
* Deployed in us-east-1 using Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Terraform syntax precision
* AWS Security Group configuration
* Default VPC referencing
* Infrastructure as Code best practices

This task reinforced how small syntax mistakes like `from port` vs `from_port` can break Terraform deployments.
