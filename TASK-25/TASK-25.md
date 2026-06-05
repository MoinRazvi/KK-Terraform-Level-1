# Terraform Level-1 Task-25: Change EC2 Instance Type

## 📌 Task Objective

Modify an existing EC2 instance using Terraform.

### Requirements

* EC2 Instance Name: `devops-ec2`
* Current Instance Type: `t2.micro`
* New Instance Type: `t2.nano`
* Instance must remain in **Running** state after modification.
* Wait for EC2 status checks to complete before applying changes.
* Region: `us-east-1`

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

## Step 2: Identify Existing EC2 Instance

Verify the instance exists:

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--region us-east-1
```

---

## Step 3: Wait for Status Checks

Check instance status:

```bash
aws ec2 describe-instance-status \
--instance-ids <INSTANCE_ID> \
--region us-east-1
```

Ensure both checks show:

```text
passed
```

Do not modify the instance while it is in:

```text
initializing
```

state.

---

## Step 4: Update `main.tf`

Use a data source to locate the existing instance and manage its configuration.

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_instance" "devops_ec2" {
  filter {
    name   = "tag:Name"
    values = ["devops-ec2"]
  }
}

resource "aws_instance" "devops_ec2" {
  ami           = data.aws_instance.devops_ec2.ami
  instance_type = "t2.nano"

  tags = {
    Name = "devops-ec2"
  }
}
```

---

## Step 5: Initialize Terraform

```bash
terraform init
```

---

## Step 6: Validate Configuration

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```

---

## Step 7: Review Changes

```bash
terraform plan
```

Verify Terraform plans to change:

```text
instance_type: "t2.micro" -> "t2.nano"
```

---

## Step 8: Apply Changes

```bash
terraform apply -auto-approve
```

Terraform will stop and start the instance automatically if required.

---

## Step 9: Verify Instance Type

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--region us-east-1 \
--query 'Reservations[*].Instances[*].[InstanceType,State.Name]'
```

Expected output:

```text
t2.nano
running
```

---

## 🔍 Key Concepts Learned

### EC2 Instance Type Modification

AWS allows instance types to be changed after creation.

Common reasons:

* Cost optimization
* Performance tuning
* Resource right-sizing

---

### Terraform Data Source

```hcl
data "aws_instance"
```

Used to locate existing AWS resources.

---

### Instance Type

```hcl
instance_type = "t2.nano"
```

Updates the compute capacity of the EC2 instance.

---

### Terraform Plan

```bash
terraform plan
```

Shows infrastructure changes before they are applied.

---

## ✅ Verification Commands

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--region us-east-1
```

```bash
aws ec2 describe-instance-status \
--instance-ids <INSTANCE_ID> \
--region us-east-1
```

---

## 🎯 Outcome

Successfully modified:

* EC2 Instance: `devops-ec2`
* Instance Type: `t2.micro` → `t2.nano`
* Instance State: `running`
* Terraform-managed infrastructure update

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* EC2 instance lifecycle management
* Infrastructure modification using Terraform
* Cost optimization through right-sizing
* Terraform state and resource updates

This task demonstrates how Terraform can be used not only to provision infrastructure but also to safely modify existing AWS resources while maintaining the desired state.
