# Terraform Level-1 Task-17: Create DynamoDB Table

## 📌 Task Objective

Create an AWS DynamoDB table using Terraform.

### Requirements

* Table Name: `xfusion-users`
* Primary Key: `xfusion_id`
* Primary Key Type: `String (S)`
* Billing Mode: `PAY_PER_REQUEST`
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

resource "aws_dynamodb_table" "xfusion_users" {
  name         = "xfusion-users"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "xfusion_id"

  attribute {
    name = "xfusion_id"
    type = "S"
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

## Step 5: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 6: Verify DynamoDB Table Creation

```bash
aws dynamodb list-tables --region us-east-1
```

Verify:

```text
xfusion-users
```

---

## Step 7: View Table Details

```bash
aws dynamodb describe-table \
--table-name xfusion-users \
--region us-east-1
```

Verify:

* Table Name → `xfusion-users`
* Partition Key → `xfusion_id`
* Billing Mode → `PAY_PER_REQUEST`

---

## 🔍 Key Concepts Learned

### DynamoDB

Amazon DynamoDB is a fully managed NoSQL database service.

Common use cases:

* User data storage
* Session management
* Application metadata
* High-scale workloads

---

### DynamoDB Table

```hcl
resource "aws_dynamodb_table"
```

Creates and manages DynamoDB tables through Terraform.

---

### Hash Key (Partition Key)

```hcl
hash_key = "xfusion_id"
```

Defines the primary key used to uniquely identify items in the table.

---

### Attribute Definition

```hcl
attribute {
  name = "xfusion_id"
  type = "S"
}
```

`S` represents a String data type.

---

### PAY_PER_REQUEST Billing Mode

```hcl
billing_mode = "PAY_PER_REQUEST"
```

Provides on-demand pricing where AWS automatically scales capacity based on traffic.

---

## ✅ Verification Commands

```bash
aws dynamodb list-tables --region us-east-1
```

```bash
aws dynamodb describe-table \
--table-name xfusion-users \
--region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* DynamoDB Table
* Table Name: `xfusion-users`
* Primary Key: `xfusion_id`
* Billing Mode: `PAY_PER_REQUEST`
* Terraform-managed database resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon DynamoDB provisioning
* NoSQL database fundamentals
* Primary key configuration
* On-demand billing mode
* Terraform database automation

This task introduces database provisioning with Terraform, enabling scalable and serverless data storage management in AWS.
