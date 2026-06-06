# Terraform Level-1 Task-28: Enable Versioning on Existing S3 Bucket

## 📌 Task Objective

Enable versioning on an existing Amazon S3 bucket using Terraform.

### Requirements

* Existing S3 Bucket: `nautilus-s3-22005`
* Enable Versioning
* Region: `us-east-1`
* Update the existing `main.tf`
* Use Terraform only

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

Since the S3 bucket already exists, only manage the versioning configuration.

```hcl
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-22005"
  aws_s3_bucket_acl = "private"

  tags = {
    Name        = "nautilus-s3-22005"
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = "nautilus-s3-22005"

  versioning_configuration {
    status = "Enabled"
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

Verify Terraform plans to enable versioning on:

```text
nautilus-s3-22005
```

---

## Step 6: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 7: Verify Versioning

```bash
aws s3api get-bucket-versioning \
--bucket nautilus-s3-22005
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

---

## 🔍 Key Concepts Learned

### S3 Versioning

Amazon S3 Versioning allows multiple versions of an object to be stored within the same bucket.

Benefits:

* Protects against accidental deletion
* Protects against overwrites
* Enables object recovery
* Improves backup and retention capabilities

---

### S3 Bucket Versioning Resource

```hcl
resource "aws_s3_bucket_versioning"
```

Used to manage versioning on an S3 bucket.

---

### Versioning Configuration

```hcl
versioning_configuration {
  status = "Enabled"
}
```

Enables object version tracking.

---

## ✅ Verification Commands

```bash
aws s3api get-bucket-versioning \
--bucket nautilus-s3-22005
```

```bash
terraform state list
```

Expected resource:

```text
aws_s3_bucket_versioning.versioning
```

---

## 🎯 Outcome

Successfully configured:

* Existing S3 Bucket: `nautilus-s3-22005`
* Versioning: Enabled
* Terraform-managed versioning configuration

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon S3 Versioning
* Data protection and recovery
* Managing existing AWS resources with Terraform
* Object version history and rollback capabilities

This task demonstrates how Terraform can modify existing AWS resources without recreating them, helping improve data durability and recovery.

---

## ⚠ Issues Faced & Solution

### Issue

Initially attempted to enable versioning directly inside the S3 bucket resource:

```hcl
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-22005"

  versioning_configuration {
    status = "Enabled"
  }
}
```

### Warning Observed

```text
acl is deprecated. Use the aws_s3_bucket_acl resource instead.
```

### Root Cause

The bucket already existed and Terraform now manages S3 versioning through a dedicated resource:

```hcl
aws_s3_bucket_versioning
```

Additionally, the `acl` argument inside `aws_s3_bucket` is deprecated in newer AWS provider versions.

### Solution

Used a dedicated versioning resource:

```hcl
resource "aws_s3_bucket_versioning" "versioning" {
  bucket = "nautilus-s3-22005"

  versioning_configuration {
    status = "Enabled"
  }
}
```

This successfully enabled versioning without recreating the bucket and allowed the lab to pass.
