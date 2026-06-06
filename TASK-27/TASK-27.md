# Terraform Level-1 Task-27: Attach IAM Policy to IAM User

## 📌 Task Objective

Attach an IAM policy to an IAM user using Terraform.

### Requirements

* IAM User: `iamuser_siva`
* IAM Policy: `iampolicy_siva`
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

The existing Terraform configuration already contained:

* IAM User (`iamuser_siva`)
* IAM Policy (`iampolicy_siva`)

To attach the policy to the user, add the following resource:

```hcl
resource "aws_iam_user_policy_attachment" "policy_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

Final configuration:

```hcl
# Create IAM user
resource "aws_iam_user" "user" {
  name = "iamuser_siva"

  tags = {
    Name = "iamuser_siva"
  }
}

# Create IAM Policy
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_siva"
  description = "IAM policy allowing EC2 read actions for siva"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Read*"]
        Resource = "*"
      }
    ]
  })
}

# Attach Policy to User
resource "aws_iam_user_policy_attachment" "policy_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
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
aws_iam_user_policy_attachment.policy_attach
```

---

## Step 6: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 7: Verify Policy Attachment

```bash
aws iam list-attached-user-policies \
--user-name iamuser_siva
```

Expected output should contain:

```text
iampolicy_siva
```

---

## 🔍 Key Concepts Learned

### IAM User

```hcl
resource "aws_iam_user"
```

Creates and manages IAM users.

---

### IAM Policy

```hcl
resource "aws_iam_policy"
```

Creates reusable IAM permissions.

---

### IAM Policy Attachment

```hcl
resource "aws_iam_user_policy_attachment"
```

Attaches an IAM policy to an IAM user.

---

### Resource References

Terraform resources can reference other resources directly:

```hcl
user       = aws_iam_user.user.name
policy_arn = aws_iam_policy.policy.arn
```

This automatically creates dependencies between resources.

---

## ✅ Verification Commands

```bash
aws iam list-attached-user-policies \
--user-name iamuser_siva
```

```bash
terraform state list
```

Expected resources:

```text
aws_iam_user.user
aws_iam_policy.policy
aws_iam_user_policy_attachment.policy_attach
```

---

## 🎯 Outcome

Successfully created and attached:

* IAM User: `iamuser_siva`
* IAM Policy: `iampolicy_siva`
* Policy Attachment between User and Policy
* Terraform-managed IAM resources

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* IAM User Management
* IAM Policy Creation
* Policy Attachments
* Terraform Resource References
* AWS Identity and Access Management (IAM)

This task demonstrates how Terraform can manage relationships between AWS resources by using resource references, allowing infrastructure components to be linked automatically and managed as code.

---

## ⚠ Issues Faced & Solution

### Issue

Initially attempted to place the policy attachment resource inside the IAM policy resource block:

```hcl
resource "aws_iam_policy" "policy" {

  resource "aws_iam_user_policy_attachment" "policy_attach" {
    ...
  }

}
```

### Error

Terraform throws an error similar to:

```text
Blocks of type "resource" are not expected here.
```

### Root Cause

Terraform does not allow nested resources. Every resource must be declared independently at the top level.

### Solution

Moved the attachment resource outside the IAM policy block:

```hcl
resource "aws_iam_user_policy_attachment" "policy_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

This allowed Terraform to:

1. Create the IAM User.
2. Create the IAM Policy.
3. Attach the Policy to the User automatically in the correct order.
