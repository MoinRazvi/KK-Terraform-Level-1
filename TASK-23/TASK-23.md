# Terraform Level-1 Task-23: Create Amazon OpenSearch Domain

## 📌 Task Objective

Create an Amazon OpenSearch Service domain using Terraform.

### Requirements

* Domain Name: `datacenter-es`
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

resource "aws_opensearch_domain" "datacenter_es" {
  domain_name    = "datacenter-es"
  engine_version = "OpenSearch_1.3"

  cluster_config {
    instance_type = "t3.small.search"
  }

  ebs_options {
    ebs_enabled = true
    volume_size = 10
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

**Note:** OpenSearch domain creation may take several minutes to complete.

---

## Step 6: Verify OpenSearch Domain Creation

```bash
aws opensearch list-domain-names \
--region us-east-1
```

Verify:

```text
datacenter-es
```

---

## Step 7: View Domain Details

```bash
aws opensearch describe-domain \
--domain-name datacenter-es \
--region us-east-1
```

Verify:

* Domain Name → `datacenter-es`
* Processing → `false`
* Domain Status → Active

---

## 🔍 Key Concepts Learned

### Amazon OpenSearch Service

Amazon OpenSearch Service is a managed search and analytics service used for:

* Log analytics
* Application monitoring
* Full-text search
* Security analytics
* Operational dashboards

---

### OpenSearch Domain

An OpenSearch domain is the primary deployment unit for OpenSearch clusters.

```hcl
resource "aws_opensearch_domain"
```

Creates and manages OpenSearch domains using Terraform.

---

### Domain Name

```hcl
domain_name = "datacenter-es"
```

Defines the OpenSearch domain name.

---

### Cluster Configuration

```hcl
cluster_config {
  instance_type = "t3.small.search"
}
```

Specifies the node type used by the OpenSearch cluster.

---

### EBS Storage

```hcl
ebs_options {
  ebs_enabled = true
  volume_size = 10
}
```

Attaches persistent storage to the OpenSearch domain.

---

## ✅ Verification Commands

```bash
aws opensearch list-domain-names \
--region us-east-1
```

```bash
aws opensearch describe-domain \
--domain-name datacenter-es \
--region us-east-1
```

---

## 🎯 Outcome

Successfully created:

* Amazon OpenSearch Domain
* Domain Name: `datacenter-es`
* Managed Search Cluster
* Terraform-managed OpenSearch resource

---

## 🚀 Key Takeaway

This task strengthened understanding of:

* Amazon OpenSearch Service
* Search and analytics infrastructure
* OpenSearch domain provisioning
* Terraform-managed AWS services

This task introduces managed search infrastructure using Terraform, enabling scalable log analysis, monitoring, and search capabilities within AWS environments.
