# Terraform Level-1 Task-20: Create SSM Parameter

## 📌 Task Objective

Create an AWS Systems Manager (SSM) Parameter using Terraform.

### Requirements

* Parameter Name: `nautilus-ssm-parameter`
* Parameter Type: `String`
* Parameter Value: `nautilus-value`
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

resource "aws_ssm_parameter" "nautilus_parameter" {
  name  = "nautilus-ssm-parameter"
  type  = "String"
  value = "nautilus-value"
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

## Step 6: Verify SSM Parameter Creation

```bash
aws ssm get-parameter \
--name "nautilus-ssm-parameter" \
--region us-east-1
```

Verify:

```json
{
    "Parameter": {
        "Name": "nautilus-ssm-parameter",
        "Type": "String",
        "Value": "nautilus-value"
    }
}
```

---

## 🔍 Key Concepts Learned

### AWS Systems Manager Parameter Store

AWS Systems Manager Parameter Store is a service used to securely store and manage:

* Configuration values
* Environment variables
* Application settings
* Secrets (using SecureString)

---

### SSM Parameter Resource

```hcl
resource "aws_ssm_parameter"
```

Creates and manages SSM parameters using Terraform.

---

### Parameter Name

```hcl
name = "nautilus-ssm-parameter"
```

Defines the parameter identifier used for retrieval.

---

### Parameter Type

```hcl
type = "String"
```

Stores plain text values.

Common parameter types:

* String
* StringList
* SecureString

---

### Parameter Value

```hcl
value = "nautilus-value"
```

Stores the actual configuration value.

---

## ✅ Verification Commands

```bash
aws ssm get-parameter \
--name "nautilus-ssm-parameter" \
--region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS SSM Parameter
* Parameter Name: `nautilus-ssm-parameter`
* Parameter Type: `String`
* Parameter Value: `nautilus-value`
* Terraform-managed configuration resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS Systems Manager Parameter Store
* Configuration management
* Parameter provisioning using Terraform
* Infrastructure as Code best practices

This task introduces centralized configuration management using Terraform, enabling applications and services to securely retrieve configuration values from AWS Systems Manager Parameter Store.
