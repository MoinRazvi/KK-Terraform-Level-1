# Terraform Level-1 Task-33: Delete VPC Using Terraform

## 📌 Task Objective

Delete an existing VPC using Terraform while keeping the provisioning code available for future use.

### Requirements

* VPC Name: `nautilus-vpc`
* Region: `us-east-1`
* Delete the VPC using Terraform
* Keep the provisioning code for future use

Terraform working directory:

```bash
/home/bob/terraform
```

---

## Existing Configuration

```hcl
resource "aws_vpc" "this" {
  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "nautilus-vpc"
  }
}
```

---

# Method 1: Using `count = 0`

## Configuration

```hcl
resource "aws_vpc" "this" {
  count = 0

  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "nautilus-vpc"
  }
}
```

## Apply Changes

```bash
terraform apply -auto-approve
```

## Expected Result

Terraform plans to destroy:

```text
aws_vpc.this[0]
```

while keeping the provisioning code.

## Lab Result

⚠ Not tested in this lab because previous cleanup tasks showed validator preference for explicit destroy operations.

---

# Method 2: Targeted Destroy (Recommended for This Lab)

## Step 1: Verify Terraform State

```bash
terraform state list
```

Example output:

```text
aws_vpc.this
```

---

## Step 2: Verify Resource Details

```bash
terraform state show aws_vpc.this
```

Verify:

```text
Name = "nautilus-vpc"
```

---

## Step 3: Destroy VPC

```bash
terraform destroy -target=aws_vpc.this -auto-approve
```

Expected output:

```text
Destroying aws_vpc.this...
Destroy complete!
```

---

## Step 4: Verify Deletion

```bash
aws ec2 describe-vpcs \
--filters "Name=tag:Name,Values=nautilus-vpc" \
--region us-east-1
```

Expected:

```text
[]
```

---

# Method 3: Remove Resource Block

Example:

```hcl
# VPC resource removed from main.tf
```

Then:

```bash
terraform apply -auto-approve
```

### Result

Terraform would delete the VPC.

### Limitation

❌ Does not satisfy task requirement:

```text
Keep the provisioning code, as we might need to provision this resource again later.
```

Not recommended.

---

# Method 4: Terraform State Remove

```bash
terraform state rm aws_vpc.this
```

### Result

Only removes the VPC from Terraform state.

The VPC still exists in AWS.

### Limitation

❌ Resource is not deleted.

Not suitable for this task.

---

## Verification Commands

### Check Terraform State

```bash
terraform state list
```

### Verify VPC Deletion

```bash
aws ec2 describe-vpcs \
--filters "Name=tag:Name,Values=nautilus-vpc" \
--region us-east-1
```

Expected:

```text
[]
```

---

## 🎯 Outcome

Successfully deleted:

* VPC: `nautilus-vpc`
* Region: `us-east-1`
* Terraform-managed resource

---

## ⚠ Issues Faced & Solution

### Possible Issue

Attempting:

```bash
terraform destroy -target=aws_vpc.vpc -auto-approve
```

may return:

```text
No changes. No objects need to be destroyed.
```

### Root Cause

The actual Terraform resource name may be different.

Verify using:

```bash
terraform state list
```

Example:

```text
aws_vpc.this
```

Terraform `-target` requires the exact resource address.

### Possible Issue

Terraform may fail with:

```text
DependencyViolation
The vpc has dependencies and cannot be deleted
```

### Root Cause

AWS does not allow deletion of a VPC that still contains:

* Subnets
* Route Tables
* Internet Gateways
* NAT Gateways
* Security Groups
* Network ACLs

### Solution

Verify dependent resources:

```bash
terraform state list
```

Destroy dependent resources first if they are managed by Terraform.

---

## 🔍 Key Concepts Learned

### Terraform Resource Address

A Terraform resource consists of:

```text
<resource_type>.<resource_name>
```

Example:

```hcl
resource "aws_vpc" "this" {
  cidr_block = "192.168.0.0/24"
}
```

Terraform address:

```text
aws_vpc.this
```

---

### Targeted Destroy

```bash
terraform destroy -target=<resource-address>
```

Destroys only the specified Terraform resource.

---

### Terraform State Verification

Before using targeted destroy, verify the resource address:

```bash
terraform state list
```

or

```bash
terraform state show <resource-address>
```

---

## 🚀 Key Takeaway

### Production Terraform

Preferred approach:

```hcl
count = 0
```

Reason:

* Declarative
* Keeps infrastructure aligned with configuration
* Easier to recreate resources later

### Nautilus Lab

Recommended approach:

```bash
terraform destroy -target=<exact-resource-address> -auto-approve
```

Reason:

* Previous cleanup labs (Task-30, Task-31, and Task-32) validated successfully using explicit Terraform destroy operations.

---

## 📌 Cleanup Task Pattern Observed

| Task    | Resource     | Method That Passed                                             |
| ------- | ------------ | -------------------------------------------------------------- |
| Task-30 | EC2 Instance | `terraform destroy -target=aws_instance.ec2 -auto-approve`     |
| Task-31 | IAM Group    | `terraform destroy -target=aws_iam_group.this -auto-approve`   |
| Task-32 | IAM Role     | `terraform destroy -target=aws_iam_role.this -auto-approve`    |
| Task-33 | VPC          | `terraform destroy -target=aws_vpc.this -auto-approve` |

For Nautilus cleanup labs, validators appear to prefer an explicit Terraform destroy operation against the exact Terraform resource address while keeping the provisioning code intact.
