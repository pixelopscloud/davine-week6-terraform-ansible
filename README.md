## 📌 Project Overview

In this project, I automated the following tasks:

### Terraform

- Created an AWS VPC
- Created a public subnet
- Created an Internet Gateway
- Created a Route Table
- Associated the Route Table with the subnet
- Created a Security Group (SSH restricted to my IP, HTTP open)
- Created 3 EC2 instances (Amazon Linux 2, t3.micro)
- Configured public IP assignment
- Used an AWS Key Pair for SSH access
- Used Terraform variables and outputs
- Used a Terraform data source to dynamically find the latest Amazon Linux AMI
- Ran `terraform fmt` and `terraform validate` before planning

### Ansible

- Created an application user (`devops`) on all EC2 instances
- Installed required packages (`httpd`, `wget`, `git`, `unzip`)
- Deployed a custom branded `index.html` page (Davine Technologies)
- Started and enabled the `httpd` service
- Verified connectivity using Ansible Ping
- Made a configuration change (updated page content) and re-ran the playbook
- Verified the change was applied automatically, without manual server access

---

## 🏗️ Architecture

```text
                         Internet
                            |
                    Internet Gateway
                            |
                        AWS VPC
                      10.0.0.0/16
                            |
                     Public Subnet
                     10.0.1.0/24
                            |
          ---------------------------------
          |               |               |
       EC2 #1          EC2 #2          EC2 #3
       t3.micro        t3.micro        t3.micro
          |               |               |
          ----------- Apache/httpd --------
```

## ☁️ AWS Resources

| Resource | Purpose |
|---|---|
| VPC | Provides isolated AWS network |
| Public Subnet | Hosts EC2 instances |
| Internet Gateway | Provides internet connectivity |
| Route Table | Routes internet traffic |
| Route Table Association | Associates subnet with route table |
| Security Group | Controls inbound/outbound traffic |
| EC2 Instances | Application/Web servers |
| Key Pair | SSH authentication |

## 📁 Project Structure

devops-week6/
│
├── ansible/
│ ├── inventory.ini
│ └── playbook.yml
│
├── terraform/
│ ├── ec2.tf
│ ├── outputs.tf
│ ├── provider.tf
│ ├── terraform.tfvars
│ ├── variables.tf
│ └── vpc.tf
│
├── .gitignore
└── README.md


## 🔧 Technologies Used

- AWS
- Terraform
- Ansible
- Amazon EC2
- Amazon VPC
- Amazon Linux 2
- Apache HTTP Server
- Git
- GitHub
- SSH

---

## 1. Terraform Setup

Terraform was used to provision the AWS infrastructure.

Formatting and validation were run before planning:

```bash
terraform fmt
terraform validate
```

Expected result:

Success! The configuration is valid.


## 2. Terraform Plan

Before creating the infrastructure:

```bash
terraform plan
```

Terraform planned to create:

- 3 EC2 instances
- 1 VPC
- 1 Subnet
- 1 Internet Gateway
- 1 Route Table
- 1 Route Table Association
- 1 Security Group

Total:

Plan: 9 to add, 0 to change, 0 to destroy.


## 3. Terraform Apply

The infrastructure was created using:

```bash
terraform apply
```

Terraform created the AWS networking infrastructure and three EC2 instances.

VPC
└── Public Subnet
├── EC2 Instance 1
├── EC2 Instance 2
└── EC2 Instance 3


## 4. Terraform Outputs

Terraform was configured to output:

- EC2 instance IDs
- Private IP addresses
- Public IP addresses
- Subnet ID
- VPC ID

Command:

```bash
terraform output
```

Public IP addresses are intentionally not stored in this README.

## 5. SSH Access

The EC2 instances were accessed using the AWS private key.

Key permissions:

```bash
chmod 400 ~/.ssh/week6-key.pem
```

SSH example:

```bash
ssh -i ~/.ssh/week6-key.pem ec2-user@<EC2_PUBLIC_IP>
```

The instances were running Amazon Linux 2.

## 6. Ansible Configuration

Ansible was used to configure all three EC2 instances.

The inventory contained the EC2 public IP addresses and SSH configuration.

Example:

```ini
[web]
<EC2_PUBLIC_IP_1>
<EC2_PUBLIC_IP_2>
<EC2_PUBLIC_IP_3>

[web:vars]
ansible_user=ec2-user
ansible_private_key_file=~/.ssh/week6-key.pem
```

Actual infrastructure IP addresses are not included in this README.

## 7. Ansible Connectivity Test

Ansible connectivity was tested using:

```bash
ansible -i inventory.ini web -m ping
```

All three EC2 instances successfully responded:

SUCCESS
ping: pong


## 8. Ansible Playbook

The Ansible playbook performed the following tasks:

**Task 1 – Create Application User**
Created a user named `devops` on all three EC2 instances.

**Task 2 – Install Required Packages**
Installed `httpd`, `wget`, `git`, and `unzip` using the `yum` package manager (Amazon Linux 2).

**Task 3 – Deploy Web Application Page**
Deployed a custom branded `index.html` page to `/var/www/html/` showing project and author details.

**Task 4 – Start and Enable Apache**
The `httpd` service was started and enabled to ensure it starts automatically after reboot.

**Task 5 – Firewalld Rule (conditional)**
An optional firewalld rule was attempted for HTTP; this step is safely ignored on Amazon Linux 2 since firewalld is not installed by default (AWS Security Group already permits port 80).

## 9. Ansible Playbook Execution

The playbook was executed using:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

The playbook completed successfully on all three servers.

Example:

PLAY RECAP
100.54.228.91 : ok=6 changed=4 failed=0 ignored=1
100.54.44.9 : ok=6 changed=4 failed=0 ignored=1
34.205.37.244 : ok=6 changed=4 failed=0 ignored=1


This confirmed:

- Facts were gathered
- `devops` user was created
- Required packages were installed
- Application page was deployed
- `httpd` service was started and enabled

## 10. Configuration Change & Re-run

To demonstrate idempotent, repeatable configuration management, the `index.html` content was updated in the playbook (an additional status line was added) and the playbook was re-run:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

The `Deploy branded index.html` task reported `changed`, confirming the update was applied automatically across all servers — without connecting to or manually editing any server.

## 11. Verification

Ansible connectivity:

```bash
ansible -i inventory.ini web -m ping
```

Application verification via curl:

```bash
curl -s http://<EC2_PUBLIC_IP>
```

The branded Davine Technologies page was successfully served from all three instances, confirming Apache was correctly installed, configured, and running.

---

## 🔐 Security Considerations

Sensitive files and credentials were excluded from Git via `.gitignore`:

.terraform/
*.tfstate
*.tfstate.backup
.terraform.lock.hcl
*.pem


Terraform state files are not committed to the public GitHub repository since they may contain infrastructure details and sensitive values.

Private SSH keys (e.g. `week6-key.pem`) must NEVER be committed to GitHub.

---

## 🧹 Infrastructure Cleanup

After completing the practical work and collecting the required screenshots, the AWS infrastructure will be destroyed to avoid unnecessary AWS charges:

```bash
terraform destroy
```

---

## 📸 Project Evidence

Screenshots captured during the practical exercise include:

- Terraform fmt / validate
- Terraform plan
- Terraform apply and outputs
- AWS EC2 instances (console)
- Ansible ping (connectivity test)
- Ansible playbook execution (first run)
- Ansible playbook execution (configuration change re-run)
- Configured server / branded web page (browser)
- Project directory structure

---

## 🎯 Learning Outcomes

**Terraform**
- Infrastructure as Code
- AWS provider configuration
- Variables, outputs, and data sources
- VPC, Subnet, Internet Gateway, Route Tables, Security Groups
- EC2 provisioning
- Terraform state, plan, apply, destroy

**Ansible**
- Inventory management
- SSH-based remote management
- Ansible modules and playbooks
- User management
- Package installation
- Web server / service configuration
- Idempotent, repeatable configuration changes
- Remote server verification

---

## 🔄 Automation Flow

Terraform
|
v
Create AWS Infrastructure (VPC, Subnet, IGW, Route Table, SG)
|
v
Create 3 EC2 Instances
|
v
Get EC2 Public IPs
|
v
Configure Ansible Inventory
|
v
Ansible Ping (connectivity check)
|
v
Run Ansible Playbook
|
+----> Create devops user
|
+----> Install required packages
|
+----> Deploy branded application page
|
+----> Start & enable httpd
|
v
Verify Servers (curl / browser)
|
v
Configuration Change --> Re-run Playbook --> Re-verify
|
v
Terraform Destroy (cleanup)


## ✅ Project Status

| Task | Status |
|---|---|
| Terraform Infrastructure | ✅ Completed |
| AWS VPC | ✅ Completed |
| Public Subnet | ✅ Completed |
| Internet Gateway | ✅ Completed |
| Route Table | ✅ Completed |
| Security Group | ✅ Completed |
| 3 EC2 Instances | ✅ Completed |
| SSH Access | ✅ Completed |
| Ansible Inventory | ✅ Completed |
| Ansible Connectivity | ✅ Completed |
| Application User Creation | ✅ Completed |
| Required Packages Installed | ✅ Completed |
| Branded Web Page Deployed | ✅ Completed |
| Apache Service Configured | ✅ Completed |
| Configuration Change Verified | ✅ Completed |
| GitHub Repository | ✅ Completed |

---

## 👨‍💻 Author

**Muhammad Ali**
Junior DevOps Engineer

Davine Technologies — DevOps Internship, Week 6

Technologies: AWS + Terraform + Ansible + EC2 + GitHub
