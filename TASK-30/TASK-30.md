# Terraform Level-1 Task-30: Delete EC2 Instance Using Terraform

## 📌 Task Objective

Delete the existing EC2 instance using Terraform while keeping the provisioning code available for future use.

### Requirements

* EC2 Instance Name: `devops-ec2`
* Region: `us-east-1`
* Delete the EC2 instance using Terraform
* Keep the provisioning code
* Ensure the instance reaches the `terminated` state

Terraform working directory:

```bash
/home/bob/terraform
```

---

## Existing Configuration

```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  vpc_security_group_ids = [
    "sg-ce464f88104d6ed53"
  ]

  tags = {
    Name = "devops-ec2"
  }
}
```

---

# Method 1: Using `count = 0`

## Configuration

```hcl
resource "aws_instance" "ec2" {
  count = 0

  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  vpc_security_group_ids = [
    "sg-ce464f88104d6ed53"
  ]

  tags = {
    Name = "devops-ec2"
  }
}
```

## Apply Changes

```bash
terraform apply -auto-approve
```

## Result

Terraform successfully destroyed the EC2 instance.

Output:

```text
aws_instance.ec2[0]: Destroying...
aws_instance.ec2[0]: Destruction complete
```

## Verification

```bash
terraform state list
```

Output:

```text
<empty>
```

Instance state:

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--query 'Reservations[*].Instances[*].[InstanceId,State.Name]'
```

Output:

```text
terminated
```

## Lab Result

❌ Lab validation failed

Error:

```text
EC2 instance have not been deleted using 'terraform'
```

Although the instance was destroyed successfully by Terraform.

---

# Method 2: Using Targeted Destroy

## Command

```bash
terraform destroy -target=aws_instance.ec2 -auto-approve
```

## Result

Terraform explicitly destroyed only the EC2 resource.

Output:

```text
Destroying aws_instance.ec2...
Destroy complete!
```

## Verification

```bash
terraform state list
```

Output:

```text
<empty>
```

Instance state:

```bash
terminated
```

## Lab Result

✅ Lab validation passed

This was the successful method for this specific Nautilus lab.

---

# Method 3: Remove Resource Block

Example:

```hcl
# EC2 resource removed from main.tf
```

Then:

```bash
terraform apply -auto-approve
```

## Result

Terraform would destroy the instance.

## Limitation

❌ Does not satisfy task requirement:

```text
Keep the provisioning code, as we might need to provision this instance again later.
```

Not recommended.

---

# Method 4: Terraform State Remove

```bash
terraform state rm aws_instance.ec2
```

## Result

Only removes resource from Terraform state.

Instance continues running in AWS.

## Limitation

❌ Instance is not deleted.

Not suitable for this task.

---

## Additional Observation

The task description included:

```text
Please make sure the Status check is completed
(if it's still in Initializing state)
before making any changes to the instance.
```

Waiting for the EC2 instance to complete initialization before performing the Terraform destroy operation appears to help avoid validation issues.

Check instance status:

```bash
aws ec2 describe-instance-status \
--region us-east-1
```

Wait until:

```text
SystemStatus = ok
InstanceStatus = ok
```

before executing Terraform changes.

---

## Verification Commands

Check instance state:

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--query 'Reservations[*].Instances[*].[InstanceId,State.Name]'
```

Check Terraform state:

```bash
terraform state list
```

---

## 🎯 Outcome

Successfully deleted:

* EC2 Instance: `devops-ec2`
* Region: `us-east-1`
* Terraform-managed resource

---

## ⚠ Issues Faced & Solution

### Issue

Used:

```hcl
count = 0
```

and executed:

```bash
terraform apply -auto-approve
```

Terraform successfully destroyed the instance and removed it from state, but the lab validator failed with:

```text
EC2 instance have not been deleted using 'terraform'
```

### Root Cause

The Nautilus validator appears to expect a direct Terraform destroy operation against the resource rather than a destroy caused by reducing the resource count.

### Solution That Worked

Waited for EC2 status checks to complete and executed:

```bash
terraform destroy -target=aws_instance.ec2 -auto-approve
```

The instance was terminated and the lab validation passed successfully.

---

## 🚀 Key Takeaway

### Production Terraform

Preferred approach:

```hcl
count = 0
```

Reason:

* Declarative
* Keeps infrastructure state aligned with configuration
* Easier to recreate later

### Nautilus Lab

Successful approach:

```bash
terraform destroy -target=aws_instance.ec2 -auto-approve
```

Reason:

* Validator explicitly recognized the Terraform destroy operation
* Lab passed successfully
