# DevOps Week 6 – Terraform & Ansible


This project demonstrates Infrastructure as Code (IaC) using Terraform and server configuration/automation using Ansible on AWS EC2.


The infrastructure was created using Terraform and the EC2 instances were then configured using Ansible.


---


## 📌 Project Overview


In this project, I automated the following tasks:


### Terraform


- Created an AWS VPC
- Created a public subnet
- Created an Internet Gateway
- Created a Route Table
- Associated the Route Table with the subnet
- Created a Security Group
- Created 3 EC2 instances
- Configured public IP assignment
- Used an AWS Key Pair for SSH access
- Used Terraform variables and outputs
- Used Terraform data source to dynamically find the Amazon Linux AMI


### Ansible


- Created a `devops` user on all EC2 instances
- Installed Apache HTTP Server (`httpd`)
- Started and enabled the `httpd` service
- Verified connectivity using Ansible Ping
- Verified the configured servers using Ansible commands


---


# 🏗️ Architecture


```text
                         Internet
                            |
                            |
                    Internet Gateway
                            |
                            |
                     AWS VPC
                   10.0.0.0/16
                            |
                     Public Subnet
                    10.0.1.0/24
                            |
          ---------------------------------
          |               |               |
          |               |               |
       EC2 #1          EC2 #2          EC2 #3
       t3.micro        t3.micro        t3.micro
          |               |               |
          ----------- Apache/httpd --------
☁️ AWS Resources
Resource	Purpose
VPC	Provides isolated AWS network
Public Subnet	Hosts EC2 instances
Internet Gateway	Provides internet connectivity
Route Table	Routes internet traffic
Route Table Association	Associates subnet with route table
Security Group	Controls inbound/outbound traffic
EC2 Instances	Application/Web servers
Key Pair	SSH authentication
📁 Project Structure
devops-week6/
│
├── ansible/
│   ├── inventory.ini
│   └── playbook.yml
│
├── terraform/
│   ├── ec2.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── terraform.tfvars
│   ├── variables.tf
│   └── vpc.tf
│
├── .gitignore
└── README.md
🔧 Technologies Used
AWS
Terraform
Ansible
Amazon EC2
Amazon VPC
Amazon Linux 2
Apache HTTP Server
Git
GitHub
SSH
1. Terraform Setup

Terraform was used to provision the AWS infrastructure.

Terraform configuration was validated using:

terraform validate

Expected result:

Success! The configuration is valid.
2. Terraform Plan

Before creating the infrastructure:

terraform plan

Terraform planned to create:

3 EC2 instances
1 VPC
1 Subnet
1 Internet Gateway
1 Route Table
1 Route Table Association
1 Security Group

Total:

Plan: 9 to add, 0 to change, 0 to destroy.
3. Terraform Apply

The infrastructure was created using:

terraform apply

Terraform created the AWS networking infrastructure and three EC2 instances.

The infrastructure consisted of:

VPC
 └── Public Subnet
      ├── EC2 Instance 1
      ├── EC2 Instance 2
      └── EC2 Instance 3
4. Terraform Outputs

Terraform was configured to output:

EC2 instance IDs
Private IP addresses
Public IP addresses
Subnet ID
VPC ID

Command:

terraform output

Public IP addresses are intentionally not stored in this README.

5. SSH Access

The EC2 instances were accessed using the AWS private key.

Key permissions:

chmod 400 ~/.ssh/week6-key.pem

SSH example:

ssh -i ~/.ssh/week6-key.pem ec2-user@<EC2_PUBLIC_IP>

The instances were running:

Amazon Linux 2
6. Ansible Configuration

Ansible was used to configure all three EC2 instances.

The inventory contained the EC2 public IP addresses and SSH configuration.

Example:

[web]
<EC2_PUBLIC_IP_1>
<EC2_PUBLIC_IP_2>
<EC2_PUBLIC_IP_3>


[web:vars]
ansible_user=ec2-user
ansible_private_key_file=~/.ssh/week6-key.pem

Actual infrastructure IP addresses are not included in this README.

7. Ansible Connectivity Test

Ansible connectivity was tested using:

ansible -i inventory.ini web -m ping

All three EC2 instances successfully responded:

SUCCESS
ping: pong
8. Ansible Playbook

The Ansible playbook performed three main tasks.

Task 1 – Create User

Created a user named:

devops

on all three EC2 instances.

Task 2 – Install Apache

Amazon Linux 2 uses the yum package manager and Apache is installed using:

httpd
Task 3 – Start and Enable Apache

The Apache service was started and enabled:

httpd

This ensures Apache starts automatically after reboot.

9. Ansible Playbook Execution

The playbook was executed using:

ansible-playbook -i inventory.ini playbook.yml

The playbook completed successfully on all three servers.

Example:

PLAY RECAP


EC2-1 : ok=4 changed=2 failed=0
EC2-2 : ok=4 changed=2 failed=0
EC2-3 : ok=4 changed=2 failed=0

This confirmed:

Facts were gathered
devops user was created
httpd was installed
httpd service was started and enabled
10. Verification

Ansible connectivity:

ansible -i inventory.ini web -m ping

Expected:

SUCCESS
ping: pong

Server configuration was also verified using Ansible remote commands.

🔐 Security Considerations

Sensitive files and credentials were excluded from Git.

The .gitignore file excludes Terraform state and local/private files.

Example:

terraform/terraform.tfstate
terraform/terraform.tfstate.backup
.terraform/
*.pem

Terraform state files should not normally be committed to a public GitHub repository because they may contain infrastructure information and potentially sensitive values.

Private SSH keys such as:

week6-key.pem

must NEVER be committed to GitHub.

🧹 Infrastructure Cleanup

After completing the practical work and collecting the required screenshots, the AWS infrastructure was destroyed to avoid unnecessary AWS charges.

Cleanup command:

terraform destroy

The Terraform-managed AWS infrastructure was removed after completing the exercise.

📸 Project Evidence

Screenshots were captured during the practical exercise, including:

Terraform installation
Terraform version
Ansible installation
Ansible version
Terraform validation
Terraform plan
Terraform apply
AWS EC2 instances
Terraform outputs
Ansible ping
Ansible playbook execution
User creation
Apache installation
Apache service configuration
Project tree structure

Screenshots can be stored under:

screenshots/
├── terraform-version.png
├── ansible-version.png
├── terraform-validate.png
├── terraform-plan.png
├── terraform-apply.png
├── aws-ec2.png
├── terraform-output.png
├── ansible-ping.png
├── ansible-playbook.png
└── project-tree.png
🎯 Learning Outcomes
Terraform
Infrastructure as Code
AWS provider configuration
Terraform variables
Terraform outputs
Terraform data sources
VPC creation
Subnet creation
Internet Gateway
Route Tables
Security Groups
EC2 provisioning
Terraform state
Terraform plan
Terraform apply
Terraform destroy
Ansible
Inventory management
SSH-based remote management
Ansible modules
Ansible playbooks
User management
Package installation
Service management
Idempotent configuration
Remote server verification
🔄 Automation Flow
Terraform
   |
   v
Create AWS Infrastructure
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
Ansible Ping
   |
   v
Run Ansible Playbook
   |
   +----> Create devops user
   |
   +----> Install httpd
   |
   +----> Start & Enable httpd
   |
   v
Verify Servers
   |
   v
Terraform Destroy
✅ Project Status
Terraform Infrastructure       ✅ Completed
AWS VPC                        ✅ Completed
Public Subnet                  ✅ Completed
Internet Gateway               ✅ Completed
Route Table                    ✅ Completed
Security Group                 ✅ Completed
3 EC2 Instances                ✅ Completed
SSH Access                     ✅ Completed
Ansible Inventory              ✅ Completed
Ansible Connectivity           ✅ Completed
User Creation                  ✅ Completed
Apache Installation            ✅ Completed
Apache Service                 ✅ Completed
Infrastructure Cleanup         ✅ Completed
GitHub Repository              ✅ Completed
👨‍💻 Author

Davine DevOps Team

DevOps Practice Project – Week 6

Technologies:

AWS + Terraform + Ansible + EC2 + GitHub
