## Terraform AWS EC2 Project

This project provisions an **Amazon EC2 instance** on AWS using Terraform. It covers creating the infrastructure, generating and using an SSH key pair, and connecting to the instance.

---

### **Project Steps**

#### **1. Install Terraform**

Download and install Terraform from [terraform.io](https://developer.hashicorp.com/terraform/downloads).
Verify installation:

```bash
terraform -version
```

#### **2. Create AWS Key Pair**

1. In AWS Console → EC2 → Key Pairs → Create key pair.
2. Save the `.pem` file locally (e.g., `terraform-key.pem`).
3. Move it to a safe directory and set permissions:

   ```bash
   chmod 400 terraform-key.pem
   ```

#### **3. Write Terraform Config Files**

**`provider.tf`**

```hcl
provider "aws" {
  region = "us-east-1"
}
```

**`main.tf`**

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316" # Amazon Linux 2 AMI
  instance_type = "t2.micro"
  key_name      = "terraform-key"

  tags = {
    Name = "Terraform-EC2"
  }
}
```

**`outputs.tf`**

```hcl
output "instance_id" {
  value = aws_instance.web.id
}

output "instance_public_ip" {
  value = aws_instance.web.public_ip
}
```

#### **4. Initialize Terraform**

```bash
terraform init
```

#### **5. Deploy Infrastructure**

```bash
terraform apply -auto-approve
```

Terraform will output:

```
instance_id = "i-xxxxxxxx"
instance_public_ip = "xx.xx.xx.xx"
```

#### **6. Connect to EC2 Instance**

From your terminal:

```bash
ssh -i /path/to/terraform-key.pem ec2-user@<public-ip>
```

---

### **.gitignore Setup**

To avoid committing secrets or local state:

**`.gitignore`**

```
.terraform/
*.tfstate
*.tfstate.backup
*.pem
.vagrant/
```

---

### **Project Cleanup**

When done, remove all resources:

```bash
terraform destroy -auto-approve
```

---

### **Prerequisites**

* AWS account with IAM permissions for EC2.
* Terraform installed.
* SSH client installed (OpenSSH).

---

### **Author**

Created by Phillip (**GeigerJR**) — *Infrastructure as Code on AWS using Terraform*.

---
https://roadmap.sh/projects/iac-digitalocean
