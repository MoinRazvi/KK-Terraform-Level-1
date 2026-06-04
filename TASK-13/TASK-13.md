# Terraform Level-1 Task-13: Create Private S3 Bucket

## 📌 Task Objective

Create a private Amazon S3 bucket using Terraform.

### Requirements

* Bucket Name: `nautilus-s3-7187`
* Access: Private
* Block all public access
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

resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-7187"
}

resource "aws_s3_bucket_public_access_block" "private_bucket" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  block_public_acls       = true
  ignore_public_acls      = true
  block_public_policy     = true
  restrict_public_buckets = true
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

## Step 6: Verify Bucket Creation

```bash
aws s3 ls
```

Verify:

```text
nautilus-s3-7187
```

---

## Step 7: Verify Public Access Block Configuration

```bash
aws s3api get-public-access-block \
--bucket nautilus-s3-7187
```

Expected output:

```json
{
  "PublicAccessBlockConfiguration": {
    "BlockPublicAcls": true,
    "IgnorePublicAcls": true,
    "BlockPublicPolicy": true,
    "RestrictPublicBuckets": true
  }
}
```

---

## 🔍 Key Concepts Learned

### Amazon S3

Amazon S3 is AWS object storage used for:

* Backups
* Application data
* Static content
* Long-term storage

---

### Private S3 Bucket

A private bucket prevents anonymous users from accessing bucket contents.

---

### Public Access Block

```hcl
resource "aws_s3_bucket_public_access_block"
```

Controls public accessibility of an S3 bucket.

---

### Block All Public Access

```hcl
block_public_acls       = true
ignore_public_acls      = true
block_public_policy     = true
restrict_public_buckets = true
```

These settings ensure the bucket remains private.

---

## ✅ Verification Commands

```bash
aws s3 ls
```

```bash
aws s3api get-public-access-block \
--bucket nautilus-s3-7187
```

---

## 🎯 Outcome

Successfully created:

* Private Amazon S3 Bucket
* Bucket Name: `nautilus-s3-7187`
* Public Access Completely Blocked
* Terraform-managed S3 Resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon S3 bucket provisioning
* Private bucket configuration
* Public Access Block settings
* Terraform S3 resource management

This task reinforced AWS security best practices by ensuring storage resources remain private and inaccessible to the public unless explicitly required.
