# Terraform Level-1 Task-19: Create SNS Topic

## 📌 Task Objective

Create an AWS SNS Topic using Terraform.

### Requirements

* Topic Name: `xfusion-notifications`
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

resource "aws_sns_topic" "xfusion_notifications" {
  name = "xfusion-notifications"
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

## Step 6: Verify SNS Topic Creation

```bash
aws sns list-topics --region us-east-1
```

Verify the output contains:

```text
xfusion-notifications
```

---

## Step 7: View Topic Details

```bash
aws sns list-topics --region us-east-1
```

Verify:

* Topic Name → `xfusion-notifications`
* Topic ARN generated successfully

---

## 🔍 Key Concepts Learned

### Amazon SNS

Amazon Simple Notification Service (SNS) is a fully managed messaging service used for:

* Notifications
* Alerts
* Application messaging
* Event-driven architectures

---

### SNS Topic

An SNS Topic acts as a communication channel for sending messages to subscribers.

Subscribers can include:

* Email
* SMS
* Lambda Functions
* SQS Queues
* HTTP/HTTPS Endpoints

---

### SNS Topic Resource

```hcl
resource "aws_sns_topic"
```

Creates and manages SNS topics through Terraform.

---

### Topic Name

```hcl
name = "xfusion-notifications"
```

Defines the SNS topic name.

---

## ✅ Verification Commands

```bash
aws sns list-topics --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS SNS Topic
* Topic Name: `xfusion-notifications`
* Terraform-managed messaging resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon SNS
* Event-driven messaging
* Notification services
* Terraform-managed AWS resources

This task introduces cloud-native notification services using Terraform, enabling scalable communication between AWS services and applications.
