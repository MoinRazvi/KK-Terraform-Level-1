# 🚀 Understanding Terraform Files in Depth

Before moving further into Level-2, it's worth understanding **why Terraform projects are structured into multiple files** and **how Terraform processes them internally**. Once you understand this, tasks involving IAM, SNS, DynamoDB, S3, CloudWatch, etc., become much easier.

---

# 🏗️ How Terraform Works Internally

When you run:

```bash
terraform init
terraform plan
terraform apply
```

Terraform does **NOT** execute files one by one.

Many beginners think:

```text
variables.tf
↓
locals.tf
↓
main.tf
↓
outputs.tf
```

This is incorrect.

Terraform loads **all `.tf` files in the directory together** and builds a dependency graph.

Example:

```text
terraform/
├── main.tf
├── variables.tf
├── locals.tf
├── outputs.tf
```

Terraform internally combines them into:

```text
Single Configuration
├── Variables
├── Data Sources
├── Locals
├── Resources
└── Outputs
```

The file names are only for human readability.

Terraform doesn't care if the resource is in:

```bash
main.tf
```

or

```bash
ec2.tf
```

or

```bash
network.tf
```

Terraform merges everything.

---

# 📄 variables.tf

## Purpose

Defines inputs for your Terraform configuration.

Think of variables like function parameters in programming.

Without variables:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Hardcoded value.

With variables:

```hcl
variable "instance_type" {
  type = string
}
```

```hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

Now reusable.

---

## Example

```hcl
variable "KKE_INSTANCE_TYPE" {
  type = string
}
```

Terraform creates:

```text
var.KKE_INSTANCE_TYPE
```

which can be referenced anywhere.

---

## Why Use Variables?

### ❌ Hardcoded

```hcl
instance_type = "t2.micro"
```

Need to edit code each time.

---

### ✅ Variable Based

```hcl
instance_type = var.KKE_INSTANCE_TYPE
```

Only update:

```hcl
terraform.tfvars
```

---

# 📄 terraform.tfvars

## Purpose

Provides values to variables.

Example:

Variable definition:

```hcl
variable "KKE_INSTANCE_TYPE" {}
```

Value assignment:

```hcl
KKE_INSTANCE_TYPE = "t2.micro"
```

---

## Internal Flow

Terraform sees:

```hcl
var.KKE_INSTANCE_TYPE
```

and substitutes:

```hcl
t2.micro
```

during planning.

---

## Priority Order

Terraform loads variables in this order:

```text
1. CLI flags
2. *.auto.tfvars
3. terraform.tfvars
4. Environment variables
5. Default values
```

Example:

```bash
terraform apply -var="KKE_INSTANCE_TYPE=t3.micro"
```

This overrides terraform.tfvars.

---

# 📄 main.tf

## Purpose

Contains actual infrastructure resources.

Example:

```hcl
resource "aws_instance" "web" {
}
```

or

```hcl
resource "aws_vpc" "main" {
}
```

or

```hcl
resource "aws_s3_bucket" "bucket" {
}
```

---

## Structure

```hcl
resource "<provider>_<resource>" "<name>" {
}
```

Example:

```hcl
resource "aws_instance" "web" {
}
```

Where:

```text
aws_instance → Resource Type
web          → Local Name
```

Referenced as:

```hcl
aws_instance.web.id
```

---

# 📄 locals.tf

## Purpose

Stores calculated values.

Think of it as variables inside Terraform.

Example:

```hcl
locals {
  company = "Nautilus"
}
```

Usage:

```hcl
local.company
```

---

## Why Use Locals?

Without locals:

```hcl
Name = "datacenter-instance-prod"
```

Repeated everywhere.

With locals:

```hcl
locals {
  environment = "prod"
}
```

```hcl
Name = "datacenter-instance-${local.environment}"
```

More maintainable.

---

# 📄 outputs.tf

## Purpose

Display values after deployment.

Example:

```hcl
output "instance_id" {
  value = aws_instance.web.id
}
```

After apply:

```bash
instance_id = i-123456789
```

---

## Why Important?

Useful when:

* Passing values to other modules
* Sharing resource IDs
* Automation pipelines
* GitHub Actions
* Jenkins

Example:

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```

---

# 📄 provider.tf

Sometimes you'll see:

```bash
provider.tf
```

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Purpose:

Tell Terraform:

```text
Which cloud?
Which region?
Which credentials?
```

---

# 📄 data.tf

Used for querying existing resources.

Example:

```hcl
data "aws_ami" "amazon_linux" {
}
```

---

## Difference

### Resource

Creates something

```hcl
resource "aws_instance" "web" {
}
```

---

### Data Source

Reads existing information

```hcl
data "aws_ami" "amazon_linux" {
}
```

No resource created.

---

# 📄 terraform.tfstate

Most important file.

Created after:

```bash
terraform apply
```

Contains:

```text
What Terraform created
Current resource IDs
Attributes
Dependencies
```

Example:

```json
{
 "id":"i-123456789"
}
```

---

## Why Needed?

Terraform compares:

```text
Desired State
vs
Current State
```

Example:

Code says:

```hcl
instance_type = "t2.micro"
```

State says:

```text
t2.micro
```

No changes.

---

If changed:

```hcl
instance_type = "t3.micro"
```

Terraform detects drift.

---

# 📄 terraform.lock.hcl

Created during:

```bash
terraform init
```

Contains provider versions.

Example:

```hcl
provider "registry.terraform.io/hashicorp/aws"
```

Version lock:

```text
5.98.0
```

Ensures:

```text
Dev
QA
Prod
```

all use same provider version.

---

# 📄 .terraform Directory

Created during:

```bash
terraform init
```

Contains:

```text
Downloaded Providers
Plugins
Modules
```

Example:

```bash
.terraform/
```

Never edit manually.

---

# 🔄 Complete Execution Flow

Suppose you run:

```bash
terraform apply
```

Terraform internally performs:

### Step 1

Read all `.tf` files

```text
variables.tf
locals.tf
main.tf
outputs.tf
```

---

### Step 2

Load variable values

```text
terraform.tfvars
```

---

### Step 3

Resolve locals

```hcl
local.AMI_ID
```

---

### Step 4

Read data sources

```hcl
data.aws_ami.amazon_linux
```

---

### Step 5

Build dependency graph

```text
VPC
 ↓
Subnet
 ↓
Security Group
 ↓
EC2
```

---

### Step 6

Generate execution plan

```bash
terraform plan
```

---

### Step 7

Create resources in dependency order

```text
VPC
 ↓
Subnet
 ↓
SG
 ↓
EC2
```

---

### Step 8

Update state file

```bash
terraform.tfstate
```

---

### Step 9

Display outputs

```bash
terraform output
```

---

# 🎯 Real-World Project Structure

In production, you rarely use only `main.tf`.

Example:

```bash
terraform/
├── provider.tf
├── variables.tf
├── locals.tf
├── data.tf
├── networking.tf
├── security.tf
├── compute.tf
├── storage.tf
├── outputs.tf
├── terraform.tfvars
├── terraform.tfstate
└── terraform.lock.hcl
```

This makes large infrastructures easier to manage.

---

# 💡 Interview Question

**Q: Does Terraform execute files in the order of their names?**

**Answer:**

No. Terraform loads all `.tf` files in the working directory, merges them into a single configuration, builds a dependency graph, and executes resources based on dependencies rather than file names.

This is one of the most important Terraform concepts and is frequently asked in DevOps interviews.

---

## 🏆 Key Takeaway

Think of Terraform files as **logical containers**, not execution units.

```text
variables.tf  → Inputs
terraform.tfvars → Variable Values
locals.tf → Internal Calculations
data.tf → Read Existing Resources
main.tf → Create Resources
outputs.tf → Display Results
terraform.tfstate → Track Infrastructure
terraform.lock.hcl → Lock Provider Versions
```

Once you understand this flow, concepts like `count`, `for_each`, modules, remote state, workspaces, lifecycle rules, and complex AWS deployments become much easier to grasp. 🚀
