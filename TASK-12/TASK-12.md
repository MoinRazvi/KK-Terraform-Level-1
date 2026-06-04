# Terraform Level-1 Task-12: Create Public S3 Bucket

## 📌 Task Objective

Create a public Amazon S3 bucket using Terraform.

### Requirements

* Bucket Name: `nautilus-s3-23263` *(replace with the bucket name provided in your lab)*
* Access: Public
* Configure the appropriate ACL to allow public access
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
  bucket = "nautilus-s3-23263"
}

resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.nautilus_bucket.id

  block_public_acls       = false
  ignore_public_acls      = false
  block_public_policy     = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "nautilus_bucket_acl" {
  depends_on = [
    aws_s3_bucket_public_access_block.public_access
  ]

  bucket = aws_s3_bucket.nautilus_bucket.id
  acl    = "public-read"
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

Verify the bucket exists:

```bash
aws s3 ls
```

Expected output:

```bash
nautilus-s3-23263
```

---

## Step 7: Verify Public Access Block Configuration

```bash
aws s3api get-public-access-block \
--bucket nautilus-s3-23263
```

Expected:

```json
{
  "PublicAccessBlockConfiguration": {
    "BlockPublicAcls": false,
    "IgnorePublicAcls": false,
    "BlockPublicPolicy": false,
    "RestrictPublicBuckets": false
  }
}
```

---

## Step 8: Verify Public ACL

```bash
aws s3api get-bucket-acl \
--bucket nautilus-s3-23263
```

Verify the output contains:

```json
{
  "URI": "http://acs.amazonaws.com/groups/global/AllUsers"
},
"Permission": "READ"
```

This confirms the bucket is publicly accessible.

---

## ⚠ Issues Faced & Solution

### Issue

The bucket was successfully created using Terraform, but the lab validation failed with:

```text
'public s3 bucket doesn't exist'
```

### Investigation

Verified bucket creation:

```bash
aws s3 ls
```

Verified public access block settings:

```bash
aws s3api get-public-access-block --bucket <bucket-name>
```

Verified bucket ACL:

```bash
aws s3api get-bucket-acl --bucket <bucket-name>
```

---

### Root Cause

The bucket existed, but public access configuration needed to be explicitly configured using:

```hcl
resource "aws_s3_bucket_public_access_block"
```

along with:

```hcl
acl = "public-read"
```

---

### Solution

Added:

```hcl
resource "aws_s3_bucket_public_access_block" "public_access" {
  block_public_acls       = false
  ignore_public_acls      = false
  block_public_policy     = false
  restrict_public_buckets = false
}
```

and configured:

```hcl
acl = "public-read"
```

using `aws_s3_bucket_acl`.

After applying the configuration, the lab validation passed successfully.

---

## 🔍 Key Concepts Learned

### Amazon S3

Amazon S3 is AWS object storage used for:

* Static website hosting
* Backups
* Application assets
* Data archiving

---

### Public Access Block

Controls whether public access is allowed for an S3 bucket.

---

### Bucket ACL

ACLs define access permissions for bucket objects.

```hcl
acl = "public-read"
```

allows public read access.

---

### Resource Dependencies

Terraform ensures the public access block configuration is applied before the ACL through:

```hcl
depends_on
```

---

## ✅ Verification Commands

```bash
aws s3 ls
```

```bash
aws s3api get-public-access-block --bucket <bucket-name>
```

```bash
aws s3api get-bucket-acl --bucket <bucket-name>
```

---

## 🎯 Outcome

Successfully created:

* Public Amazon S3 Bucket
* Public Access Block configuration
* Public Read ACL
* Terraform-managed storage resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon S3 provisioning
* Public bucket configuration
* Bucket ACL management
* AWS Public Access Block settings
* Terraform resource dependencies

This task highlighted the importance of configuring both ACLs and Public Access Block settings when creating publicly accessible S3 buckets using Terraform.
