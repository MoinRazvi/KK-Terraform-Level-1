# Terraform Level-1 Task-11: Create CloudWatch Alarm

## 📌 Task Objective

Create an AWS CloudWatch alarm using Terraform with the following requirements:

* Alarm Name: `nautilus-alarm`
* Monitor: EC2 CPU Utilization
* Threshold: Greater than 80%
* Evaluation Period: 5 minutes
* Evaluation Period Count: 1
* Region: `us-east-1`

Terraform working directory:

```bash id="4w2nka"
/home/bob/terraform
```

---

## Step 1: Navigate to Working Directory

```bash id="m8q5tp"
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

```hcl id="p6r8wx"
provider "aws" {
  region = "us-east-1"
}

resource "aws_cloudwatch_metric_alarm" "nautilus_alarm" {
  alarm_name          = "nautilus-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
}
```

---

## Step 3: Initialize Terraform

```bash id="x7m4vq"
terraform init
```

---

## Step 4: Validate Configuration

```bash id="t3k9wb"
terraform validate
```

Expected output:

```bash id="j5p8cx"
Success! The configuration is valid.
```

---

## Step 5: Apply Configuration

```bash id="n1r7za"
terraform apply -auto-approve
```

---

## Step 6: Verify Alarm Creation

```bash id="q4v2pm"
aws cloudwatch describe-alarms --region us-east-1
```

Verify:

* Alarm Name → `nautilus-alarm`
* Metric → `CPUUtilization`
* Threshold → `80`
* Evaluation Period → `300 seconds`
* Evaluation Period Count → `1`

---

## ⚠ Issues Faced & Solution

### Issue

Initially attempted to fetch a running EC2 instance using a Terraform data source:

```hcl id="u8p3zd"
data "aws_instance" "instance" {
  filter {
    name   = "instance-state-name"
    values = ["running"]
  }
}
```

Terraform failed with:

```text id="k2w7xr"
Error: no matching EC2 Instance found
```

### Root Cause

The AWS account used for the lab did not contain any EC2 instances.

Verification:

```bash id="c5m9qn"
aws ec2 describe-instances --region us-east-1
```

Output:

```bash id="r7x1pk"
Reservations: []
```

### Solution

Removed the EC2 data source and created the CloudWatch alarm using only the required task parameters.

---

## 🔍 Key Concepts Learned

### CloudWatch Alarm

CloudWatch alarms monitor AWS metrics and trigger actions when thresholds are breached.

---

### CPU Utilization Metric

Measures the percentage of compute resources currently being used.

---

### Evaluation Period

```hcl id="s9v6ma"
evaluation_periods = 1
period             = 300
```

Monitors one evaluation period of five minutes.

---

### Threshold

```hcl id="b3n8wy"
threshold = 80
```

The alarm transitions to ALARM state when CPU utilization exceeds 80%.

---

### Terraform Monitoring Resources

Terraform can provision monitoring and alerting resources alongside infrastructure resources.

---

## ✅ Verification Command

```bash id="f8m2rp"
aws cloudwatch describe-alarms --region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* AWS CloudWatch Alarm
* Alarm Name: `nautilus-alarm`
* Metric: `CPUUtilization`
* Threshold: `80%`
* Evaluation Period: `5 minutes`
* Managed using Terraform

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* AWS CloudWatch monitoring
* Metric-based alerting
* Terraform monitoring resources
* Troubleshooting Terraform data sources

This task highlighted the importance of validating existing AWS resources before referencing them through Terraform data sources.
