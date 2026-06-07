# Terraform Level-1 Task-34: Upload File to Existing S3 Bucket

## 📌 Task Objective

Copy the file `/tmp/xfusion.txt` to the existing S3 bucket `xfusion-cp-1848` using Terraform.

### Requirements

* S3 Bucket: `xfusion-cp-1848`
* Source File: `/tmp/xfusion.txt`
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

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "xfusion-cp-1848"

  acl = "private"

  tags = {
    Name = "xfusion-cp-1848"
  }
}

resource "aws_s3_object" "upload_file" {
  bucket = aws_s3_bucket.my_bucket.id
  key    = "xfusion.txt"
  source = "/tmp/xfusion.txt"
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

```text
Success! The configuration is valid.
```

---

## Step 5: Review Execution Plan

```bash
terraform plan
```

Expected resource:

```text
aws_s3_object.upload_file
```

---

## Step 6: Apply Configuration

```bash
terraform apply -auto-approve
```

Expected output:

```text
aws_s3_object.upload_file: Creation complete
```

---

## Step 7: Verify File Upload

```bash
aws s3 ls s3://xfusion-cp-1848
```

Expected output:

```text
xfusion.txt
```

---

## 🔍 Key Concepts Learned

### Upload Files to S3 Using Terraform

```hcl
resource "aws_s3_object"
```

Uploads files directly from the local system to an S3 bucket.

---

### Object Key

```hcl
key = "xfusion.txt"
```

Defines the object name inside the bucket.

---

### Source File

```hcl
source = "/tmp/xfusion.txt"
```

Specifies the local file path to upload.

---

### Bucket Reference

```hcl
bucket = aws_s3_bucket.my_bucket.id
```

References the existing S3 bucket managed by Terraform.

---

## ✅ Verification Commands

```bash
aws s3 ls s3://xfusion-cp-1848
```

```bash
terraform state list
```

Expected:

```text
aws_s3_bucket.my_bucket
aws_s3_object.upload_file
```

---

## 🎯 Outcome

Successfully uploaded:

* File: `/tmp/xfusion.txt`
* Bucket: `xfusion-cp-1848`
* Object: `xfusion.txt`

using Terraform.

---

## ⚠ Issues Faced & Solution

### Issue

Terraform may fail if the source file is not present.

### Verification

```bash
ls -l /tmp/xfusion.txt
```

### Solution

Ensure the file exists before executing:

```bash
terraform apply -auto-approve
```

---

## 🚀 Key Takeaway

This task demonstrates how Terraform can manage S3 objects in addition to AWS infrastructure resources. By using `aws_s3_object`, files can be uploaded and managed through Infrastructure as Code, eliminating the need for manual uploads through the AWS CLI or Console.
