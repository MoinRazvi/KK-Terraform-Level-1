# Terraform Level-1 Task-10: Create Snapshot of Existing EBS Volume

## 📌 Task Objective

Create a snapshot of an existing EBS volume using Terraform.

### Requirements

* Source Volume: `xfusion-vol`
* Snapshot Name: `xfusion-vol-ss`
* Description: `Xfusion Snapshot`
* Region: `us-east-1`
* Snapshot state must be **completed**

Terraform working directory:

```bash id="a1r8mk"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="p7w4xc"
cd /home/bob/terraform
```

---

## Step 2: Update `main.tf`

Add the snapshot resource while keeping the existing EBS volume resource.

```hcl id="v8k2yn"
provider "aws" {
  region = "us-east-1"
}

resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name = "xfusion-vol"
  }
}

resource "aws_ebs_snapshot" "xfusion_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Xfusion Snapshot"

  tags = {
    Name = "xfusion-vol-ss"
  }
}
```

---

## Step 3: Initialize Terraform

```bash id="m4j9zp"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="q6t3ha"
terraform validate
```

Expected output:

```bash id="r9v8nb"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="f2x7pk"
terraform apply -auto-approve
```

---

## Step 6: Verify Snapshot Creation

```bash id="n5k1wc"
aws ec2 describe-snapshots --owner-ids self --region us-east-1
```

Verify:

* Snapshot Name → `xfusion-vol-ss`
* Description → `Xfusion Snapshot`
* State → `completed`

---

## 🔍 Key Concepts Learned

### EBS Snapshot

An EBS snapshot is a point-in-time backup of an EBS volume.

It is used for:

* Backup and recovery
* Disaster recovery
* Volume restoration
* Data protection

---

### Resource Dependency

Terraform automatically creates the snapshot only after the EBS volume is available.

```hcl id="u3p7mf"
volume_id = aws_ebs_volume.k8s_volume.id
```

---

### Snapshot Description

Adds metadata to identify the snapshot purpose.

---

### Tags

Used for naming and organizing snapshot resources.

---

### Terraform State Management

Resources already managed by Terraform should remain in configuration when dependent resources are added.

---

## ✅ Verification Commands

Check Terraform-managed resources:

```bash id="k8m4tx"
terraform state list
```

Expected:

```bash id="j3r6wa"
aws_ebs_volume.k8s_volume
aws_ebs_snapshot.xfusion_snapshot
```

Check snapshot status:

```bash id="c9v2pl"
aws ec2 describe-snapshots --owner-ids self --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* Snapshot of existing EBS volume
* Snapshot Name: `xfusion-vol-ss`
* Description: `Xfusion Snapshot`
* Region: `us-east-1`
* Snapshot state: Completed

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS snapshot automation
* Terraform resource dependencies
* State management
* Infrastructure backup provisioning

This task reinforced an important Terraform concept: dependent resources should reference Terraform-managed resources to maintain proper state tracking and avoid unintended destruction.
