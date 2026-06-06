# Terraform Level-1 Task-29: Backup and Delete S3 Bucket Using AWS CLI Through Terraform

## 📌 Task Objective

Backup the contents of an existing S3 bucket and then delete the bucket using AWS CLI commands executed through Terraform.

### Requirements

* Existing S3 Bucket: `nautilus-bck-20476`
* Copy all bucket contents to:

```bash
/opt/s3-backup/
```

* Delete the S3 bucket after backup
* Use AWS CLI through Terraform
* Update the existing `main.tf`
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

## Step 2: Update `main.tf`

Since the task explicitly requires AWS CLI commands to be executed through Terraform, use a `null_resource` with a `local-exec` provisioner.

```hcl
resource "null_resource" "s3_backup_cleanup" {

  provisioner "local-exec" {
    command = <<EOT
mkdir -p /opt/s3-backup
aws s3 cp s3://nautilus-bck-20476 /opt/s3-backup --recursive
aws s3 rb s3://nautilus-bck-20476 --force
EOT
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

Verify Terraform plans to create:

```text
null_resource.s3_backup_cleanup
```

---

## Step 6: Apply Configuration

```bash
terraform apply -auto-approve
```

Terraform will execute:

```bash
mkdir -p /opt/s3-backup
```

```bash
aws s3 cp s3://nautilus-bck-20476 /opt/s3-backup --recursive
```

```bash
aws s3 rb s3://nautilus-bck-20476 --force
```

---

## Step 7: Verify Backup

```bash
ls -l /opt/s3-backup
```

Verify bucket contents are present.

---

## Step 8: Verify Bucket Deletion

```bash
aws s3 ls
```

Verify:

```text
nautilus-bck-20476
```

is no longer listed.

---

## 🔍 Key Concepts Learned

### null_resource

```hcl
resource "null_resource"
```

Used when Terraform needs to execute actions rather than manage infrastructure resources.

---

### local-exec Provisioner

```hcl
provisioner "local-exec"
```

Executes shell commands on the machine running Terraform.

---

### AWS CLI Integration

Terraform can invoke AWS CLI commands when a task specifically requires CLI-based operations.

---

### S3 Recursive Copy

```bash
aws s3 cp s3://bucket-name /local/path --recursive
```

Downloads all objects from an S3 bucket.

---

### Force Delete Bucket

```bash
aws s3 rb s3://bucket-name --force
```

Deletes all objects and removes the bucket.

---

## ✅ Verification Commands

```bash
ls -l /opt/s3-backup
```

```bash
aws s3 ls
```

---

## 🎯 Outcome

Successfully completed:

* S3 bucket backup
* Local backup storage
* S3 bucket deletion
* AWS CLI execution through Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Terraform provisioners
* AWS CLI integration
* S3 backup operations
* Infrastructure cleanup automation

This task demonstrates how Terraform can orchestrate operational tasks using AWS CLI commands when infrastructure resources require backup and cleanup actions outside normal Terraform resource management.

---

## ⚠ Issues Faced & Solution

### Requirement

The task specifically required:

> Use AWS CLI through Terraform

### Solution

Used:

```hcl
resource "null_resource"
```

with:

```hcl
provisioner "local-exec"
```

to execute AWS CLI commands directly from Terraform.

This allowed the bucket contents to be backed up locally before safely deleting the bucket.
