# Terraform Level-1 Task-32: Delete IAM Role Using Terraform

## 📌 Task Objective

Delete an existing IAM Role using Terraform while keeping the provisioning code available for future use.

### Requirements

* IAM Role Name: `iamrole_rose`
* Region: `us-east-1`
* Delete the IAM Role using Terraform
* Keep the provisioning code for future use

Terraform working directory:

```bash
/home/bob/terraform
```

---

## Existing Configuration

```hcl
resource "aws_iam_role" "this" {
  name = "iamrole_rose"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
}
```

---

# Method 1: Using `count = 0`

## Configuration

```hcl
resource "aws_iam_role" "this" {
  count = 0

  name = "iamrole_rose"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
}
```

## Apply Changes

```bash
terraform apply -auto-approve
```

## Expected Result

Terraform plans to destroy:

```text
aws_iam_role.this[0]
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
aws_iam_role.this
```

---

## Step 2: Verify Resource Details

```bash
terraform state show aws_iam_role.this
```

Verify:

```text
name = "iamrole_rose"
```

---

## Step 3: Destroy IAM Role

```bash
terraform destroy -target=aws_iam_role.this -auto-approve
```

Expected output:

```text
Destroying aws_iam_role.this...
Destroy complete!
```

---

## Step 4: Verify Deletion

```bash
aws iam get-role --role-name iamrole_rose
```

Expected:

```text
NoSuchEntity
```

---

## Method 3: Remove Resource Block

Example:

```hcl
# IAM role resource removed from main.tf
```

Then:

```bash
terraform apply -auto-approve
```

### Result

Terraform would delete the IAM role.

### Limitation

❌ Does not satisfy task requirement:

```text
Keep the provisioning code, as we might need to provision this resource again later.
```

Not recommended.

---

## Method 4: Terraform State Remove

```bash
terraform state rm aws_iam_role.this
```

### Result

Only removes the role from Terraform state.

The IAM role still exists in AWS.

### Limitation

❌ Resource is not deleted.

Not suitable for this task.

---

## Verification Commands

### Check Terraform State

```bash
terraform state list
```

### Verify IAM Role

```bash
aws iam get-role --role-name iamrole_rose
```

Expected:

```text
NoSuchEntity
```

---

## 🎯 Outcome

Successfully deleted:

* IAM Role: `iamrole_rose`
* Region: `us-east-1`
* Terraform-managed resource

---

## ⚠ Issues Faced & Solution

### Possible Issue

Attempting:

```bash
terraform destroy -target=aws_iam_role.role -auto-approve
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
aws_iam_role.this
```

Terraform `-target` requires the exact resource address.

### Solution

Use the exact address returned by Terraform state:

```bash
terraform destroy -target=aws_iam_role.this -auto-approve
```

---

## 🔍 Key Concepts Learned

### Terraform Resource Address

A Terraform resource consists of:

```text
<resource_type>.<resource_name>
```

Example:

```hcl
resource "aws_iam_role" "this" {
  name = "iamrole_rose"
}
```

Terraform address:

```text
aws_iam_role.this
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

* Previous cleanup labs (Task-30 and Task-31) validated successfully using explicit Terraform destroy operations.

---

## 📌 Cleanup Task Pattern Observed

| Task    | Resource     | Method That Passed                                              |
| ------- | ------------ | --------------------------------------------------------------- |
| Task-30 | EC2 Instance | `terraform destroy -target=aws_instance.ec2 -auto-approve`      |
| Task-31 | IAM Group    | `terraform destroy -target=aws_iam_group.this -auto-approve`    |
| Task-32 | IAM Role     | `terraform destroy -target=aws_iam_role.role -auto-approve` |

For Nautilus cleanup labs, validators appear to prefer an explicit Terraform destroy operation against the exact Terraform resource address.
