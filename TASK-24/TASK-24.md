# Terraform Level-1 Task-24: Create AWS Secrets Manager Secret

## 📌 Task Objective

Create an AWS Secrets Manager secret using Terraform.

### Requirements

* Secret Name: `xfusion-secret`
* Secret Value:

  * username: `admin`
  * password: `Namin123`
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

resource "aws_secretsmanager_secret" "xfusion_secret" {
  name = "xfusion-secret"
}

resource "aws_secretsmanager_secret_version" "xfusion_secret_value" {
  secret_id = aws_secretsmanager_secret.xfusion_secret.id

  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
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

## Step 6: Verify Secret Creation

```bash
aws secretsmanager list-secrets \
--region us-east-1
```

Verify:

```text
xfusion-secret
```

---

## Step 7: Verify Secret Value

```bash
aws secretsmanager get-secret-value \
--secret-id xfusion-secret \
--region us-east-1
```

Expected output:

```json
{
  "username": "admin",
  "password": "Namin123"
}
```

---

## 🔍 Key Concepts Learned

### AWS Secrets Manager

AWS Secrets Manager is used to securely store and manage:

* Database credentials
* API keys
* Application secrets
* Authentication tokens

---

### Secret Resource

```hcl
resource "aws_secretsmanager_secret"
```

Creates the secret container in AWS Secrets Manager.

---

### Secret Version

```hcl
resource "aws_secretsmanager_secret_version"
```

Stores the actual secret value.

---

### jsonencode()

```hcl
secret_string = jsonencode({...})
```

Converts Terraform data into valid JSON format for storage.

---

### Resource Dependency

```hcl
secret_id = aws_secretsmanager_secret.xfusion_secret.id
```

Ensures Terraform creates the secret before storing the secret value.

---

## ✅ Verification Commands

```bash
aws secretsmanager list-secrets \
--region us-east-1
```

```bash
aws secretsmanager get-secret-value \
--secret-id xfusion-secret \
--region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS Secrets Manager Secret
* Secret Name: `xfusion-secret`
* Username: `admin`
* Password: `Namin123`
* Terraform-managed secret resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS Secrets Manager
* Secure credential storage
* Secret version management
* Terraform resource dependencies
* Infrastructure as Code security practices

This task introduces secure secret management using Terraform, enabling applications and services to retrieve sensitive information without hardcoding credentials in source code.
