# Terraform Level-1 Task-22: Create CloudFormation Stack

## 📌 Task Objective

Create an AWS CloudFormation Stack using Terraform.

### Requirements

* Stack Name: `datacenter-stack`
* Resource: S3 Bucket
* Bucket Name: `datacenter-bucket-15160`
* Versioning: Enabled
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

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_cloudformation_stack" "datacenter_stack" {
  name = "datacenter-stack"

  template_body = <<STACK
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "DatacenterBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "datacenter-bucket-15160",
        "VersioningConfiguration": {
          "Status": "Enabled"
        }
      }
    }
  }
}
STACK
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

## Step 6: Verify CloudFormation Stack Creation

```bash
aws cloudformation describe-stacks \
--stack-name datacenter-stack \
--region us-east-1
```

Verify:

```text
datacenter-stack
```

---

## Step 7: Verify S3 Bucket Creation

```bash
aws s3 ls
```

Verify:

```text
datacenter-bucket-15160
```

---

## Step 8: Verify Bucket Versioning

```bash
aws s3api get-bucket-versioning \
--bucket datacenter-bucket-15160
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

---

## 🔍 Key Concepts Learned

### AWS CloudFormation

CloudFormation is AWS's Infrastructure as Code (IaC) service that provisions AWS resources using templates.

---

### CloudFormation Stack

A stack is a collection of AWS resources managed as a single unit.

```hcl
resource "aws_cloudformation_stack"
```

Creates and manages CloudFormation stacks through Terraform.

---

### Template Body

```hcl
template_body = <<STACK
...
STACK
```

Embeds a CloudFormation template directly inside Terraform.

---

### S3 Bucket Versioning

```json
"VersioningConfiguration": {
  "Status": "Enabled"
}
```

Allows multiple versions of objects to be stored in the bucket.

Benefits:

* Accidental deletion protection
* Object recovery
* Audit and rollback capability

---

## ✅ Verification Commands

```bash
aws cloudformation describe-stacks \
--stack-name datacenter-stack \
--region us-east-1
```

```bash
aws s3api get-bucket-versioning \
--bucket datacenter-bucket-15160
```

---

## 🎯 Outcome

Successfully created:

* CloudFormation Stack
* Stack Name: `datacenter-stack`
* S3 Bucket: `datacenter-bucket-15160`
* Bucket Versioning Enabled
* Terraform-managed CloudFormation deployment

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS CloudFormation
* Infrastructure as Code (IaC)
* Terraform and CloudFormation integration
* S3 bucket versioning
* Stack-based resource provisioning

This task demonstrates how Terraform can orchestrate CloudFormation stacks, enabling teams to combine multiple Infrastructure as Code technologies within AWS environments.
