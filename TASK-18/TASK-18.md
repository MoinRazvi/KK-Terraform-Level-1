# Terraform Level-1 Task-18: Create AWS Kinesis Data Stream

## 📌 Task Objective

Create an AWS Kinesis Data Stream using Terraform.

### Requirements

* Stream Name: `devops-stream`
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

resource "aws_kinesis_stream" "devops_stream" {
  name             = "devops-stream"
  shard_count      = 1
  retention_period = 24
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

## Step 6: Verify Kinesis Stream Creation

```bash
aws kinesis list-streams --region us-east-1
```

Verify:

```text
devops-stream
```

---

## Step 7: View Stream Details

```bash
aws kinesis describe-stream \
--stream-name devops-stream \
--region us-east-1
```

Verify:

* Stream Name → `devops-stream`
* Stream Status → `ACTIVE`
* Shard Count → `1`

---

## 🔍 Key Concepts Learned

### Amazon Kinesis Data Streams

Amazon Kinesis Data Streams is a fully managed service used for:

* Real-time data ingestion
* Streaming analytics
* Log processing
* Event-driven architectures
* IoT data processing

---

### Kinesis Stream Resource

```hcl
resource "aws_kinesis_stream"
```

Creates and manages Kinesis Data Streams using Terraform.

---

### Stream Name

```hcl
name = "devops-stream"
```

Defines the Kinesis stream name.

---

### Shard Count

```hcl
shard_count = 1
```

A shard provides a unit of capacity for data ingestion and processing.

Since the task does not specify the number of shards, a single shard is sufficient.

---

### Retention Period

```hcl
retention_period = 24
```

Stores stream data for 24 hours before automatic deletion.

---

## ✅ Verification Commands

```bash
aws kinesis list-streams --region us-east-1
```

```bash
aws kinesis describe-stream \
--stream-name devops-stream \
--region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS Kinesis Data Stream
* Stream Name: `devops-stream`
* Stream Status: Active
* Terraform-managed streaming resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon Kinesis Data Streams
* Real-time data processing services
* Streaming infrastructure provisioning
* Terraform-managed AWS services

This task introduces event streaming and real-time data ingestion using Terraform, which forms the foundation for analytics, monitoring, and event-driven cloud architectures.
