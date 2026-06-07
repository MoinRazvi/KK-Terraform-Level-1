# Terraform Level-1 Task-31: Delete IAM Group Using Terraform

## 📌 Task Objective

Delete an existing IAM Group using Terraform while keeping the provisioning code available for future use.

### Requirements

* IAM Group Name: `iamgroup_mariyam`
* Region: `us-east-1`
* Delete the IAM Group using Terraform
* Keep the provisioning code for future use

Terraform working directory:

```bash
/home/bob/terraform
```

---

## Existing Configuration

```hcl
resource "aws_iam_group" "this" {
  name = "iamgroup_mariyam"
}
```

---

## Method 1: Using `count = 0`

### Configuration

```hcl
resource "aws_iam_group" "this" {
  count = 0

  name = "iamgroup_mariyam"
}
```

### Apply Changes

```bash
terraform apply -auto-approve
```

### Expected Result

Terraform plans to destroy:

```text
aws_iam_group.this[0]
```

while keeping the provisioning code.

### Lab Result

⚠ Not tested in this lab because the validator behavior was already identified from previous cleanup tasks.

---

## Method 2: Targeted Destroy (Successful Method)

### Verify Terraform State

```bash
terraform state list
```

Output:

```text
aws_iam_group.this
```

### Destroy IAM Group

```bash
terraform destroy -target=aws_iam_group.this -auto-approve
```

### Result

Terraform explicitly destroys only the IAM group resource.

Example output:

```text
Destroying aws_iam_group.this...
Destroy complete!
```

### Verification

Check Terraform state:

```bash
terraform state list
```

Verify the resource is removed.

Check AWS:

```bash
aws iam get-group --group-name iamgroup_mariyam
```

Expected:

```text
NoSuchEntity
```

### Lab Result

✅ Lab validation passed successfully.

---

## Method 3: Remove Resource Block

Example:

```hcl
# IAM group resource removed from main.tf
```

Then:

```bash
terraform apply -auto-approve
```

### Result

Terraform would delete the IAM group.

### Limitation

❌ Does not satisfy task requirement:

```text
Keep the provisioning code, as we might need to provision this resource again later.
```

Not recommended.

---

## Method 4: Terraform State Remove

```bash
terraform state rm aws_iam_group.this
```

### Result

Only removes the resource from Terraform state.

The IAM group continues to exist in AWS.

### Limitation

❌ Resource is not deleted.

Not suitable for this task.

---

## Verification Commands

### Check Terraform State

```bash
terraform state list
```

### Verify IAM Group

```bash
aws iam get-group --group-name iamgroup_mariyam
```

Expected:

```text
NoSuchEntity
```

---

## 🎯 Outcome

Successfully deleted:

* IAM Group: `iamgroup_mariyam`
* Region: `us-east-1`
* Terraform-managed resource

---

## ⚠ Issues Faced & Solution

### Issue

Initially attempted:

```bash
terraform destroy -target=aws_iam_group.group -auto-approve
```

Terraform returned:

```text
No changes. No objects need to be destroyed.
```

### Root Cause

The actual Terraform resource name was:

```text
aws_iam_group.this
```

not:

```text
aws_iam_group.group
```

Terraform `-target` requires the exact resource address stored in Terraform state.

### Verification

```bash
terraform state list
```

Output:

```text
aws_iam_group.this
```

### Solution That Worked

Executed:

```bash
terraform destroy -target=aws_iam_group.this -auto-approve
```

Terraform successfully deleted the IAM group and the lab validation passed.

---

## 🔍 Key Concepts Learned

### Terraform Resource Address

A Terraform resource consists of:

```text
<resource_type>.<resource_name>
```

Example:

```hcl
resource "aws_iam_group" "this" {
  name = "iamgroup_mariyam"
}
```

Terraform address:

```text
aws_iam_group.this
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

Successful approach:

```bash
terraform destroy -target=aws_iam_group.this -auto-approve
```

Reason:

* Validator explicitly recognized the Terraform destroy operation.
* Lab passed successfully.

---

## 📌 Cleanup Task Pattern Observed

| Task    | Resource     | Method That Passed                                           |
| ------- | ------------ | ------------------------------------------------------------ |
| Task-30 | EC2 Instance | `terraform destroy -target=aws_instance.ec2 -auto-approve`   |
| Task-31 | IAM Group    | `terraform destroy -target=aws_iam_group.this -auto-approve` |

For Nautilus cleanup labs, validators appear to prefer an explicit Terraform destroy operation against the exact Terraform resource address.
