> Update 17/04/2026: Making progress on Ansible, but not 100% yet!

------

# Infrastructure as Code (IaC)
## I. IaC with Terraform in WSL to AWS
### 1. Install WSL in your local windows
```powershell
wsl --install
```
### 2. After WSL is installed, open it! (You can do it with searching "wsl" on the *Start* bar or just type `wsl` in Powershell) <br>
<img width="1235" height="658" alt="image" src="https://github.com/user-attachments/assets/6a838475-5432-4f75-a9ee-17db70bb2b48" /><br>

### 3. Create directories as specified on the task.
 ```plaintext
Automation
└── Terraform
    ├── aws
    │   ├── main.tf
    │   ├── outputs.tf
    │   ├── providers.tf
    │   └── variables.tf
    ├── azure
    │   └── ...
    └── gcp
        └── ...
```
<img width="560" height="128" alt="image" src="https://github.com/user-attachments/assets/1d21676b-c6dd-4d13-a2d7-07ba57da84c9" />

### 4. Install AWS CLI Version 2
Since Ubuntu 24.04 does not provide awscli in the standard repository, use the manual installation method: 

```bash
sudo apt-get update && sudo apt-get install curl unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
# Verify installation
aws --version
```
<img width="968" height="133" alt="image" src="https://github.com/user-attachments/assets/d20a2e03-ca58-4767-89d2-86159b50529a" />

### 5. Install Terraform
Go to [Terraform's Install Page](https://developer.hashicorp.com/terraform/install), scroll down to Linux section, and copy then run the bash code.
<img width="1020" height="527" alt="image" src="https://github.com/user-attachments/assets/b8b995a3-321a-4c1a-8c6d-98a34a4505c0" />
```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```
After that, run `terraform --version` to check if it's installed!
<img width="517" height="87" alt="image" src="https://github.com/user-attachments/assets/b23a7d5d-283d-4862-aee2-08146e24782d" />

### 6. AWS Credentials Configuration
Get your **Access Key ID** and **Secret Access Key** from the AWS Console (Security Credentials Menu).<br>
You can access it by clicking your account name on the top right screen, then choose "Security Credentials".<br>
<img width="428" height="601" alt="image" src="https://github.com/user-attachments/assets/f1fce64c-7231-4b0f-b1fe-a2495898f970" /><br>
Scroll down until you can see the "Access keys" section, then click "Create access key". Check the `I understand creating a root access key is not a best practice, but I still want to create one.` Then click "Create access key" again.
After that, you'd see your keys on your screen. **PLEASE SAVE IT BECAUSE THE SECRET KEY WILL BE ONLY SHOWN ONCE!**<br>
<img width="1642" height="657" alt="image" src="https://github.com/user-attachments/assets/d9b7739d-fd3f-4a67-9576-160bccd613a0" /><br>
After all that, run this:
```bash
aws configure
```
Then fill it with:
```plaintext
AWS Access Key ID: (Enter your Key ID)
AWS Secret Access Key: (Enter your Secret Key)
Default region name: ap-southeast-3 (Jakarta) or ap-southeast-1 (Singapore)
Default output format: (just press enter)
```
To confirm, you can run `aws sts get-caller-identity` to see the ID and Key that you've saved.
<img width="557" height="147" alt="image" src="https://github.com/user-attachments/assets/4542c231-783a-43b3-a072-53687d40daf8" />

### 7. Terraform File Structure
First, let us see what we'd create.
- providers.tf
- variables.tf
- main.tf
- outputs.tf
-----
`providers.tf` is a file that gonna tell Terraform on what cloud computing that we'll use.
```terraform
# ==========================================
# TERRAFORM BLOCK (Core Settings)
# ==========================================
# This block tells Terraform which plugin versions must be downloaded 
# when we first run the 'terraform init' command.
terraform {
  required_providers {
    
    # We assign the local name "aws" for this provider.
    aws = {
      
      # 'source' is the address where Terraform downloads the plugin.
      # "hashicorp/aws" means the official plugin made by HashiCorp (the creator of Terraform) specifically for AWS.
      source  = "hashicorp/aws"
      
      # 'version' specifies which plugin versions are allowed to be used.
      # The '~>' symbol (pessimistic constraint) means we are asking for any "5.x" version (e.g., 5.1, 5.8, 5.40),
      # as long as it DOES NOT upgrade to the major version 6.0, because major versions usually contain breaking changes.
      version = "~> 5.0"
    }
  }
}

# ==========================================
# PROVIDER BLOCK (Your Specific Account Settings)
# ==========================================
# Once downloaded, this block is used to configure how the AWS plugin operates.
provider "aws" {
  
  # 'region' tells AWS in which location (Singapore, Jakarta, etc.) this infrastructure will be built.
  # Instead of hardcoding the value here, we pull the value from the variables.tf file
  # using the command 'var.aws_region'.
  region = var.aws_region
}
```
-----
Next, is `variables.tf`. The file acts as a "centralized control panel" or a dictionary for your infrastructure. Instead of hardcoding values directly into your main code (main.tf or providers.tf), you store them here. This makes your code much cleaner, reusable, and easier to modify without accidentally breaking the main logic.
```terraform
# ==========================================
# REGION VARIABLE
# ==========================================
# This block declares a new variable named "aws_region".
# You can reference this anywhere in your Terraform code using 'var.aws_region'.
variable "aws_region" {
  
  # 'description' is purely for human readability. It helps you or your teammates 
  # quickly understand what this variable is used for.
  description = "AWS region for deployment"
  
  # 'type' enforces the kind of data allowed here. 
  # 'string' means it must be text (wrapped in quotes).
  type        = string
  
  # 'default' is the fallback value used if you don't explicitly provide a different one 
  # (for example, by passing it through the terminal command). 
  # Here it's set to Jakarta ("ap-southeast-3") 
  default     = "ap-southeast-3" 
}

# ==========================================
# SSH KEY NAME VARIABLE
# ==========================================
# This block declares a variable named "key_name".
variable "key_name" {
  
  description = "Name of the SSH key pair"
  
  type        = string
  
  # This is the default name that will be used for both the AWS Key Pair 
  # and the local .pem file that gets generated.
  # Because it's stored here as a variable, if you ever want to change the key name 
  # (e.g., to "my_key" or "my_project_key"), you only have to change it in this one file, 
  # rather than hunting down every single reference inside the massive main.tf file.
  default     = "my-aws-key"
}
```
-----
`main.tf` is the core of Terraform's set-up. Here, we wrote the setting that Terraform will follow for our plans.
```terraform
# ==========================================
# 1. NETWORKING (Core Network Infrastructure)
# ==========================================

# A VPC (Virtual Private Cloud) acts as an isolated network for our servers.
resource "aws_vpc" "main_vpc" {
  cidr_block           = "10.0.0.0/16" # The internal IP range available for this network
  enable_dns_hostnames = true          # Allows AWS to automatically assign DNS names
  enable_dns_support   = true          # Enables internal DNS resolution
  tags = { Name = "TaskVPC" }          
}

# A Subnet is a specific subdivision inside the VPC where the servers will reside.
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.main_vpc.id  # Connects this subnet to the VPC above
  cidr_block              = "10.0.1.0/24"        # Allocates a smaller block of IPs for this subnet
  map_public_ip_on_launch = true                 # Auto-assigns public IPs to new instances
  availability_zone       = "${var.aws_region}a" # Specifies the physical data center zone (e.g., Zone A)
  tags = { Name = "TaskPublicSubnet" }
}

# An Internet Gateway acts as a door allowing network traffic to reach the outside internet.
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id 
  tags = { Name = "TaskIGW" }
}

# A Route Table dictates where network traffic should be directed.
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id
  route {
    cidr_block = "0.0.0.0/0"                 # Target: all internet traffic
    gateway_id = aws_internet_gateway.igw.id # Destination: route it through the Internet Gateway
  }
  tags = { Name = "TaskPublicRouteTable" }
}

# This association explicitly links our Subnet to the Route Table created above.
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}


# ==========================================
# 2. FIREWALL (Security Group)
# ==========================================

# A Security Group acts as a virtual firewall for our instances.
resource "aws_security_group" "allow_all" {
  name        = "allow_all_traffic"
  description = "Allow all inbound and outbound traffic"
  vpc_id      = aws_vpc.main_vpc.id 

  # Ingress rules govern INBOUND traffic (data coming into the server)
  ingress {
    from_port   = 0             # 0 means all ports are open
    to_port     = 0             
    protocol    = "-1"          # -1 means all protocols (TCP, UDP, etc.) are allowed
    cidr_blocks = ["0.0.0.0/0"] # Allows access from any IP address globally
  }

  # Egress rules govern OUTBOUND traffic (data leaving the server)
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"] 
  }
  tags = { Name = "AllowAllSG" }
}


# ==========================================
# 3. SSH KEY PAIR (Authentication)
# ==========================================

# Instructs Terraform to generate a new RSA cryptographic key pair locally.
resource "tls_private_key" "ssh_key" {
  algorithm = "RSA"  
  rsa_bits  = 4096   
}

# Registers the newly generated Public Key with your AWS account.
resource "aws_key_pair" "generated_key" {
  key_name   = var.key_name                               
  public_key = tls_private_key.ssh_key.public_key_openssh 
}

# Saves the Private Key to your local machine as a .pem file for SSH access.
resource "local_file" "private_key_pem" {
  content         = tls_private_key.ssh_key.private_key_pem 
  filename        = "${path.module}/${var.key_name}.pem"    
  file_permission = "0400"  # Sets read-only permissions to satisfy SSH security requirements                                
}


# ==========================================
# 4. DATA SOURCES (Dynamic ID Lookup)
# ==========================================

# Automatically searches for the latest official Ubuntu 24.04 image ID.
data "aws_ami" "ubuntu" {
  most_recent = true               
  owners      = ["099720109477"]   # The official AWS account ID for Canonical (Ubuntu)
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*"] 
  }
}


# ==========================================
# 5. COMPUTE (EC2 Instances)
# ==========================================

# Provisioning the First Ubuntu Server
resource "aws_instance" "ubuntu_server_1" {
  ami           = data.aws_ami.ubuntu.id              # Uses the Ubuntu ID found above
  instance_type = "t3.small"                          # The selected Free Tier eligible spec
  subnet_id     = aws_subnet.public_subnet.id         
  vpc_security_group_ids = [aws_security_group.allow_all.id] # Attaches the firewall rules
  key_name      = aws_key_pair.generated_key.key_name # Attaches the generated SSH key

  tags = { Name = "Ubuntu-24-Server-1" }
}

# Provisioning the Second Ubuntu Server
resource "aws_instance" "ubuntu_server_2" {
  ami           = data.aws_ami.ubuntu.id              
  instance_type = "t3.small"                          
  subnet_id     = aws_subnet.public_subnet.id         
  vpc_security_group_ids = [aws_security_group.allow_all.id] 
  key_name      = aws_key_pair.generated_key.key_name 

  tags = { Name = "Ubuntu-24-Server-2" }
}


# ==========================================
# 6. STATIC IPs (Elastic IPs)
# ==========================================

# Allocates and attaches a permanent Static IP to Server 1.
resource "aws_eip" "eip_1" {
  instance = aws_instance.ubuntu_server_1.id 
  domain   = "vpc"                         
  tags = { Name = "Ubuntu-EIP-1" }
}

# Allocates and attaches a permanent Static IP to Server 2.
resource "aws_eip" "eip_2" {
  instance = aws_instance.ubuntu_server_2.id
  domain   = "vpc"
  tags = { Name = "Ubuntu-EIP-2" }
}


# ==========================================
# 7. BLOCK STORAGE (EBS Volumes)
# ==========================================

# Creates a 20GB extra virtual hard drive for Server 1.
resource "aws_ebs_volume" "vol_1" {
  availability_zone = "${var.aws_region}a" # Must match the exact zone of the EC2 instance
  size              = 20                    # Kept at 7GB to ensure total account storage stays under the 30GB Free Tier limit
  type              = "gp3"                # General Purpose SSD type
  tags = { Name = "Ubuntu-Extra-Vol-1" }
}

# Creates a 20GB extra virtual hard drive for Server 2.
resource "aws_ebs_volume" "vol_2" {
  availability_zone = "${var.aws_region}a"
  size              = 20                    
  type              = "gp3"
  tags = { Name = "Ubuntu-Extra-Vol-2" }
}

# Physically attaches the extra volume to Server 1.
resource "aws_volume_attachment" "vol_att_1" {
  device_name = "/dev/sdh"                           # The device path recognized by the Linux OS
  volume_id   = aws_ebs_volume.vol_1.id              # The specific hard drive to attach
  instance_id = aws_instance.ubuntu_server_1.id      # The specific server to attach it to
}

# Physically attaches the extra volume to Server 2.
resource "aws_volume_attachment" "vol_att_2" {
  device_name = "/dev/sdh"
  volume_id   = aws_ebs_volume.vol_2.id
  instance_id = aws_instance.ubuntu_server_2.id
}
```
-----
Lastly, `outputs.tf` acts like a "receipt" or a "summary dashboard" after your infrastructure is successfully built.

When Terraform finishes creating everything, it generates a massive, complex file in the background (called the state file) that contains every single detail about your servers. Instead of forcing you to dig through that huge file or log into the AWS website just to find your new IP addresses, outputs.tf extracts exactly the information you need and prints it neatly right in your terminal.
```terraform
# ==========================================
# SERVER 1 OUTPUTS
# ==========================================

# Prints the allocated Static IP of Server 1 to the terminal.
output "server_1_static_ip" {
  value = aws_eip.eip_1.public_ip
}

# Generates a ready-to-copy SSH command for Server 1.
output "ssh_to_server_1" {
  # Combines the key name and the static IP dynamically.
  # The username is 'ubuntu' because we are using an Ubuntu OS image.
  value = "ssh -i ${var.key_name}.pem ubuntu@${aws_eip.eip_1.public_ip}"
}


# ==========================================
# SERVER 2 OUTPUTS
# ==========================================

# Prints the allocated Static IP of Server 2 to the terminal.
output "server_2_static_ip" {
  value = aws_eip.eip_2.public_ip
}

# Generates a ready-to-copy SSH command for Server 2.
output "ssh_to_server_2" {
  value = "ssh -i ${var.key_name}.pem ubuntu@${aws_eip.eip_2.public_ip}"
}
```
### 8. Execution Steps
Run these commands sequentially:
```bash
# 1. Initialize the plugins
terraform init

# 2. Review the execution plan
terraform plan

# 3. Deploy the infrastructure
terraform apply --auto-approve
```
<img width="815" height="383" alt="image" src="https://github.com/user-attachments/assets/bf1536b7-f176-430a-a586-7a0677e43976" /><br> *Terraform initialize to run in AWS.*<br><br>
<img width="1405" height="611" alt="image" src="https://github.com/user-attachments/assets/0f887136-ca66-4d38-818b-0b87e002bb0b" /><br> *Terraform review the plans.*<br><br>
<img width="1029" height="953" alt="image" src="https://github.com/user-attachments/assets/ef34ad26-0fae-421e-96ba-657d0fa0fd80" /><br> *Example of outputs after applying the plans (`terraform apply --auto-approve`).*<br><br>
<img width="734" height="196" alt="image" src="https://github.com/user-attachments/assets/48cdf63a-36b6-4475-9cd6-b037f089969b" /><br> *Terraform successfuly created two servers. This output made by `outputs.tf`*<br><br>

### 9. SSH Access to Servers
Once `apply` is complete, Terraform will generate a `my-aws-key.pem` file in your directory. Ensure the file permissions are properly set before using it:
```bash
chmod 400 my-aws-key.pem

# SSH into Server 1 (Ubuntu)
ssh -i my-aws-key.pem ubuntu@<SERVER_1_STATIC_IP>

# SSH into Server 2 (Ubuntu)
ssh -i my-aws-key.pem ubuntu@<SERVER_2_STATIC_IP>
```
<img width="984" height="886" alt="image" src="https://github.com/user-attachments/assets/724c8740-e3c5-4c60-8370-1e3ca79a48ca" /><br> *Applying `chmod` and accessing Server 1*<br><br>
<img width="1002" height="760" alt="image" src="https://github.com/user-attachments/assets/682adfe8-a2d4-49f6-931f-88e49b1ac4d2" /><br> *Accessing Server 2*

### 10. Destroying the Infrastructure
After you're done with the servers, you might want to "destroy" them. To avoid unwanted charges after you are done experimenting, tear down the environment:
```bash
terraform destroy
# Type 'yes' when prompted for confirmation
```
<img width="848" height="274" alt="image" src="https://github.com/user-attachments/assets/4ac5343c-8a6f-45ae-b363-fc1f248725e8" /><br> *Terraforms review the command on how much will be destroyed.* <br><br>
<img width="624" height="150" alt="image" src="https://github.com/user-attachments/assets/092acf43-5596-4b04-bf8e-16f96591dcd5" /><br> *Both servers successfuly destroyed!*

------

## II. Configures Servers with Ansible
### 1. Install required apps
First, we need to install pipx first. 
```bash
sudo apt update
sudo apt install pipx
pipx ensurepath
```
<img width="336" height="94" alt="image" src="https://github.com/user-attachments/assets/d747e1d9-1d10-44d4-ac0f-8283103076c1" /><br> *Checking pipx version.* <br><br>
Then, we can install Ansible with pipx by following [this guide from Ansible's official website](https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html)
```bash
pipx install --include-deps ansible 
```
<img width="1033" height="512" alt="image" src="https://github.com/user-attachments/assets/dcfe7a79-8fa1-4cb3-8af7-74ad3c2ef75a" /><br>
We also going to install these so Ansible can run smoothly.

| Tool             | How                                                | Purpose                                       |
|------------------|----------------------------------------------------|-----------------------------------------------|
| passlib          | `pipx inject ansible passlib`                        | Password hashing for Ansible user creation    |
| community.docker | `ansible-galaxy collection install community.docker` | Ansible module for managing Docker containers |
| ansible.posix    | `ansible-galaxy collection install ansible.posix`    | Ansible module for ssh key management         |

### 2. Preparing the Directories and Files
Now, time to create the directories and files, structured like this:
```plaintext
Automation/
└── Ansible/
    ├── ansible.cfg	                  # Config file — tells Ansible where your inventory is, which SSH key to use, which remote user, etc.
    ├── site.yaml                     # Master playbook — just imports/calls all other playbooks in order
    ├── appserver.yaml                # Server 1: Docker, FE, BE
    ├── gateway.yaml                  # Server 2: Nginx, Database, etc.
    ├── monitoring.yaml               # For monitoring configs
    ├── group_vars/
    │   └── all                       # Global variables shared across all playbooks — DB passwords, image names, ports, etc
    ├── templates
    │   └── nginx_fe.conf.j2          # Tells Nginx to forward traffic from your FE domain to S1 port 3000
    │   └── nginx_be.conf.j2          # Tells Nginx to forward traffic from your BE domain to S1 port 5000
    │   └── nginx_prometheus.conf.j2  # Tells Nginx to forward traffic from your Prometheus domain to port 9090
    │   └── nginx_grafana.conf.j2     # Tells Nginx to forward traffic from your Grafana domain to port 3000
    │   └── prometheus.yml.j2         # Tells Prometheus which servers to scrape metrics from
    └── inventory                     # List of your servers with their IPs, grouped by role        
```
<img width="772" height="56" alt="image" src="https://github.com/user-attachments/assets/c867d084-44c4-47d7-8aaa-19695ecbc117" /><br>

-----
`inventory` is basically a list of your servers that Ansible needs to know about. Before Ansible can do anything to your servers, it needs to know:
- Where are the servers? (IP addresses)
- How do I group them? (appserver, gateway)
- How do I connect to them? (which user, which SSH key)

```ini
# [appserver] is a group name — servers listed under here are your S1 servers
# You can have multiple IPs under one group if you have multiple app servers
[appserver]
108.136.90.214   # S1 IP address — Frontend + Backend server

# [gateway] is another group — servers listed here are your S2 servers
[gateway]
16.78.119.244    # S2 IP address — Nginx + Database server

# [all:vars] applies these variables to ALL servers in this inventory
[all:vars]

# The Linux user Ansible will use to SSH into your servers
# Since we're using AWS Ubuntu, the default user is "ubuntu"
ansible_user=ubuntu

# Path to your AWS .pem key file — needed to authenticate SSH without a password
ansible_ssh_private_key_file=~/Automation/Terraform/aws/my-aws-key.pem

# Skips the "Are you sure you want to connect?" prompt when SSH-ing into a new server
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
-----
For `ansible.cfg`, think of it as Ansible's settings file. Without it, you'd have to pass all these options manually every time you run a command. With it, Ansible automatically knows:
- Where your inventory is
- Which SSH key to use
- Which user to connect as
```ini
[defaults]
# Path to your inventory file — tells Ansible where to find your servers
inventory = ./Inventory

# The default user Ansible uses to SSH into your servers
remote_user = ubuntu

# Path to your AWS .pem key file for SSH authentication
private_key_file = ~/Automation/Terraform/aws/my-aws-key.pem

# Speeds up Ansible by disabling host key checking
host_key_checking = False

# Makes Ansible output more readable by using YAML format
stdout_callback = yaml

# How many servers Ansible can configure simultaneously
# Since we only have 2 servers, 10 is more than enough
forks = 10
```
-----
`group_vars/all` is a central config file for all your variables. Instead of hardcoding values like domain names or ports inside each playbook, you define them here once and reference them everywhere like {{ fe_domain }}.
This way if something changes — like a domain name or port — you only need to update it in one place instead of hunting through every playbook.
```ini
# ============================================================
# SSH & User Configuration
# ============================================================

# The new Linux user that will be created on both servers
new_user: rizal

# Password for the new user (will be hashed automatically by Ansible)
user_password: "your_password_here"

# ============================================================
# Domain Configuration
# ============================================================

# Base domain from Cloudflare
base_domain: studentdumbways.my.id

# Frontend domain
fe_domain: "wayshub.rizal.{{ base_domain }}"

# Backend/API domain
be_domain: "api.rizal.{{ base_domain }}"

# Grafana domain
grafana_domain: "grafana.rizal.{{ base_domain }}"

# Prometheus domain
prometheus_domain: "prometheus.rizal.{{ base_domain }}"

# ============================================================
# Application Configuration
# ============================================================

# Frontend Docker image name
fe_image: wayshub-frontend

# Backend Docker image name
be_image: wayshub-backend

# Frontend container port
fe_port: 3000

# Backend container port
be_port: 5000

# ============================================================
# Monitoring Configuration
# ============================================================

# Prometheus port
prometheus_port: 9090

# Grafana port
grafana_port: 3000

# Node exporter port
node_exporter_port: 9100
```
-----
Now, we'll create playbooks that is `appserver.yaml`, `gateway.yaml`, and `monitoring.yaml`. A playbook is basically a to-do list for Ansible. It's a YAML file that tells Ansible: 
- Which servers to connect to
- What tasks to run on those servers
- In what order to run them

The key parts:

| Part    | What it means? |
| -------- | ------- |
| `hosts`  | Which group from your Inventory to run on    |
| `become` | Whether to use sudo (true = yes)     |
| `task`    | The actual list of things to do    |
| `name`   | Human readable description of each task |
| `module` | The Ansible built-in tool that does the actual work |

What are modules?
Modules are Ansible's built-in tools. Instead of writing bash scripts, Ansible has ready-made modules for almost everything. So when you run ansible-playbook `appserver.yaml`, Ansible reads the file top to bottom and executes each task one by one on your server — without you having to SSH in manually.

For, `appserver.yaml`, we'll use this script:
```yaml
# This playbook runs on S1 (appserver) only
- name: Configure Appserver
  hosts: appserver
  become: true  # Run as sudo/root

  tasks:
    # ============================================================
    # User Setup
    # ============================================================

    # Create new user "rizal" on the server
    - name: Create new user
      ansible.builtin.user:
        name: "{{ new_user }}"
        password: "{{ user_password | password_hash('sha512') }}"
        shell: /bin/bash
        create_home: true
        state: present

    # Add the new user to the sudo group so they can run sudo commands
    - name: Add user to sudo group
      ansible.builtin.user:
        name: "{{ new_user }}"
        groups: sudo
        append: true

    # Copy your local SSH public key to the new user's authorized_keys
    # This allows SSH login using your key instead of password
    - name: Set up SSH key for new user
      ansible.posix.authorized_key:
        user: "{{ new_user }}"
        state: present
        key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

    # Allow password authentication via SSH
    - name: Enable password authentication
      ansible.builtin.lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^PasswordAuthentication'
        line: 'PasswordAuthentication yes'
        state: present

    # Restart SSH service to apply changes
    - name: Restart SSH service
      ansible.builtin.service:
        name: ssh
        state: restarted

    # ============================================================
    # Docker Installation
    # ============================================================

    # Install required packages for Docker
    - name: Install Docker dependencies
      ansible.builtin.apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
        state: present
        update_cache: true

    # Add Docker's official GPG key to verify downloads
    - name: Add Docker GPG key
      ansible.builtin.apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    # Add Docker's official repository
    - name: Add Docker repository
      ansible.builtin.apt_repository:
        repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu noble stable"
        state: present

    # Install Docker engine
    - name: Install Docker
      ansible.builtin.apt:
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
        state: present
        update_cache: true

    # Add the new user to docker group so they can run Docker without sudo
    - name: Add user to docker group
      ansible.builtin.user:
        name: "{{ new_user }}"
        groups: docker
        append: true

    # Make sure Docker service is running
    - name: Start and enable Docker
      ansible.builtin.service:
        name: docker
        state: started
        enabled: true

    # ============================================================
    # Deploy Frontend & Backend
    # ============================================================

    # Clone the frontend repo from GitHub
    - name: Clone frontend repository
      ansible.builtin.git:
        repo: https://github.com/kelompok1-dumbways/wayshub-frontend.git
        dest: /home/{{ new_user }}/wayshub-frontend
        force: true

    # Clone the backend repo from GitHub
    - name: Clone backend repository
      ansible.builtin.git:
        repo: https://github.com/kelompok1-dumbways/wayshub-backend.git
        dest: /home/{{ new_user }}/wayshub-backend
        force: true

    # Build and run frontend Docker container
    - name: Run frontend container
      community.docker.docker_container:
        name: wayshub-frontend
        build:
          path: /home/{{ new_user }}/wayshub-frontend
        image: "{{ fe_image }}"
        state: started
        restart_policy: always
        ports:
          - "{{ fe_port }}:3000"

    # Build and run backend Docker container
    - name: Run backend container
      community.docker.docker_container:
        name: wayshub-backend
        build:
          path: /home/{{ new_user }}/wayshub-backend
        image: "{{ be_image }}"
        state: started
        restart_policy: always
        ports:
          - "{{ be_port }}:5000"

    # ============================================================
    # Node Exporter (Monitoring)
    # ============================================================

    # Run Node Exporter as a Docker container for monitoring
    - name: Run Node Exporter container
      community.docker.docker_container:
        name: node-exporter
        image: prom/node-exporter:latest
        state: started
        restart_policy: always
        ports:
          - "{{ node_exporter_port }}:9100"
```

What this playbook does in order:

- Creates user rizal with password + SSH key
- Installs Docker
- Clones FE & BE repos from GitHub
- Runs FE & BE as Docker containers
- Runs Node Exporter for monitoring

For, `gateway.yaml`, we'll use this script:
```yaml
---
# This playbook runs on S2 (gateway) only
- name: Configure Gateway
  hosts: gateway
  become: true  # Run as sudo/root

  tasks:
    # ============================================================
    # User Setup
    # ============================================================

    # Create new user "rizal" on the server
    - name: Create new user
      ansible.builtin.user:
        name: "{{ new_user }}"
        password: "{{ user_password | password_hash('sha512') }}"
        shell: /bin/bash
        create_home: true
        state: present

    # Add the new user to the sudo group
    - name: Add user to sudo group
      ansible.builtin.user:
        name: "{{ new_user }}"
        groups: sudo
        append: true

    # Copy your local SSH public key to the new user's authorized_keys
    - name: Set up SSH key for new user
      ansible.posix.authorized_key:
        user: "{{ new_user }}"
        state: present
        key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

    # Allow password authentication via SSH
    - name: Enable password authentication
      ansible.builtin.lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^PasswordAuthentication'
        line: 'PasswordAuthentication yes'
        state: present

    # Restart SSH service to apply changes
    - name: Restart SSH service
      ansible.builtin.service:
        name: ssh
        state: restarted

    # ============================================================
    # Docker Installation
    # ============================================================

    # Install required packages for Docker
    - name: Install Docker dependencies
      ansible.builtin.apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
        state: present
        update_cache: true

    # Add Docker's official GPG key
    - name: Add Docker GPG key
      ansible.builtin.apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    # Add Docker's official repository
    - name: Add Docker repository
      ansible.builtin.apt_repository:
        repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu noble stable"
        state: present

    # Install Docker engine
    - name: Install Docker
      ansible.builtin.apt:
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
        state: present
        update_cache: true

    # Add new user to docker group
    - name: Add user to docker group
      ansible.builtin.user:
        name: "{{ new_user }}"
        groups: docker
        append: true

    # Make sure Docker service is running
    - name: Start and enable Docker
      ansible.builtin.service:
        name: docker
        state: started
        enabled: true

    # ============================================================
    # Nginx Reverse Proxy
    # ============================================================

    # Install Nginx
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    # Make sure Nginx is running
    - name: Start and enable Nginx
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

    # Create Nginx config for frontend
    - name: Configure Nginx for frontend
      ansible.builtin.template:
        src: templates/nginx_fe.conf.j2
        dest: /etc/nginx/sites-available/wayshub-frontend
        mode: '0644'

    # Create Nginx config for backend
    - name: Configure Nginx for backend
      ansible.builtin.template:
        src: templates/nginx_be.conf.j2
        dest: /etc/nginx/sites-available/wayshub-backend
        mode: '0644'

    # Enable frontend site
    - name: Enable frontend site
      ansible.builtin.file:
        src: /etc/nginx/sites-available/wayshub-frontend
        dest: /etc/nginx/sites-enabled/wayshub-frontend
        state: link

    # Enable backend site
    - name: Enable backend site
      ansible.builtin.file:
        src: /etc/nginx/sites-available/wayshub-backend
        dest: /etc/nginx/sites-enabled/wayshub-backend
        state: link

    # Reload Nginx to apply new configs
    - name: Reload Nginx
      ansible.builtin.service:
        name: nginx
        state: reloaded

    # ============================================================
    # SSL Certificate (Let's Encrypt)
    # ============================================================

    # Install Certbot for SSL certificate generation
    - name: Install Certbot
      ansible.builtin.apt:
        name:
          - certbot
          - python3-certbot-nginx
        state: present
        update_cache: true

    # Generate SSL certificate for frontend domain
    - name: Generate SSL for frontend
      ansible.builtin.command:
        cmd: >
          certbot --nginx -d {{ fe_domain }}
          --non-interactive --agree-tos
          --email admin@{{ base_domain }}
      args:
        creates: /etc/letsencrypt/live/{{ fe_domain }}

    # Generate SSL certificate for backend domain
    - name: Generate SSL for backend
      ansible.builtin.command:
        cmd: >
          certbot --nginx -d {{ be_domain }}
          --non-interactive --agree-tos
          --email admin@{{ base_domain }}
      args:
        creates: /etc/letsencrypt/live/{{ be_domain }}

    # ============================================================
    # Node Exporter (Monitoring)
    # ============================================================

    # Run Node Exporter as Docker container
    - name: Run Node Exporter container
      community.docker.docker_container:
        name: node-exporter
        image: prom/node-exporter:latest
        state: started
        restart_policy: always
        ports:
          - "{{ node_exporter_port }}:9100"
```

What this playbook does in order:

- Creates user rizal with password + SSH key
- Installs Docker
- Installs Nginx
- Configures Nginx reverse proxy for FE & BE
- Generates SSL certificates via Let's Encrypt
- Runs Node Exporter for monitoring

For, `monitoring.yaml`, we'll use this script:
```yaml
---
# This playbook runs on S2 (gateway) only
# Prometheus and Grafana only need to be on one server
- name: Configure Monitoring
  hosts: gateway
  become: true  # Run as sudo/root

  tasks:
    # ============================================================
    # Prometheus
    # ============================================================

    # Create a directory to store Prometheus config
    - name: Create Prometheus config directory
      ansible.builtin.file:
        path: /home/{{ new_user }}/prometheus
        state: directory
        mode: '0755'

    # Copy Prometheus config file to the server
    # This tells Prometheus which servers to scrape metrics from
    - name: Copy Prometheus config
      ansible.builtin.template:
        src: templates/prometheus.yml.j2
        dest: /home/{{ new_user }}/prometheus/prometheus.yml
        mode: '0644'

    # Run Prometheus as a Docker container
    - name: Run Prometheus container
      community.docker.docker_container:
        name: prometheus
        image: prom/prometheus:latest
        state: started
        restart_policy: always
        ports:
          - "{{ prometheus_port }}:9090"
        # Mount the config file into the container
        volumes:
          - /home/{{ new_user }}/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml

    # ============================================================
    # Grafana
    # ============================================================

    # Create a directory to store Grafana data
    - name: Create Grafana data directory
      ansible.builtin.file:
        path: /home/{{ new_user }}/grafana
        state: directory
        mode: '0755'

    # Run Grafana as a Docker container
    - name: Run Grafana container
      community.docker.docker_container:
        name: grafana
        image: grafana/grafana:latest
        state: started
        restart_policy: always
        ports:
          - "{{ grafana_port }}:3000"
        # Mount the data directory so Grafana data persists
        volumes:
          - /home/{{ new_user }}/grafana:/var/lib/grafana
        # Set default admin credentials
        env:
          GF_SECURITY_ADMIN_USER: "admin"
          GF_SECURITY_ADMIN_PASSWORD: "admin123"

    # ============================================================
    # Nginx config for Prometheus and Grafana
    # ============================================================

    # Create Nginx config for Prometheus
    - name: Configure Nginx for Prometheus
      ansible.builtin.template:
        src: templates/nginx_prometheus.conf.j2
        dest: /etc/nginx/sites-available/prometheus
        mode: '0644'

    # Create Nginx config for Grafana
    - name: Configure Nginx for Grafana
      ansible.builtin.template:
        src: templates/nginx_grafana.conf.j2
        dest: /etc/nginx/sites-available/grafana
        mode: '0644'

    # Enable Prometheus site
    - name: Enable Prometheus site
      ansible.builtin.file:
        src: /etc/nginx/sites-available/prometheus
        dest: /etc/nginx/sites-enabled/prometheus
        state: link

    # Enable Grafana site
    - name: Enable Grafana site
      ansible.builtin.file:
        src: /etc/nginx/sites-available/grafana
        dest: /etc/nginx/sites-enabled/grafana
        state: link

    # Reload Nginx to apply new configs
    - name: Reload Nginx
      ansible.builtin.service:
        name: nginx
        state: reloaded

    # ============================================================
    # SSL for Monitoring domains
    # ============================================================

    # Generate SSL certificate for Prometheus
    - name: Generate SSL for Prometheus
      ansible.builtin.command:
        cmd: >
          certbot --nginx -d {{ prometheus_domain }}
          --non-interactive --agree-tos
          --email admin@{{ base_domain }}
      args:
        creates: /etc/letsencrypt/live/{{ prometheus_domain }}

    # Generate SSL certificate for Grafana
    - name: Generate SSL for Grafana
      ansible.builtin.command:
        cmd: >
          certbot --nginx -d {{ grafana_domain }}
          --non-interactive --agree-tos
          --email admin@{{ base_domain }}
      args:
        creates: /etc/letsencrypt/live/{{ grafana_domain }}
```

What this playbook does in order:

- Creates directories for Prometheus & Grafana data
- Copies Prometheus config (tells it where to scrape metrics from)
- Runs Prometheus as Docker container
- Runs Grafana as Docker container
- Configures Nginx reverse proxy for both
- Generates SSL certificates for both domains

Lastly, `site.yaml` is the master playbook, the one that gonna runs all playbooks in order.

```yaml
# Master playbook — runs all playbooks in order
# Just run: ansible-playbook site.yaml
# Step 1: Configure appserver (S1) first
# Sets up user, Docker, deploys FE & BE, Node Exporter
- import_playbook: appserver.yaml
# Step 2: Configure gateway (S2)
# Sets up user, Docker, Nginx, SSL, Node Exporter
- import_playbook: gateway.yaml
# Step 3: Set up monitoring
# Deploys Prometheus, Grafana on S2
# Configures Nginx for monitoring domains
- import_playbook: monitoring.yaml
```

-----

Next, we're going to create some configuration files for Nginx and Prometheus inside `/templates/` directory. They use the .j2 extension which stands for Jinja2, a Python templating engine that Ansible uses.<br>
Why are we doing it this way? Without templates, if your IP changes you'd have to manually edit every config file. With templates, you just update `group_vars/all` and rerun Ansible — all configs update automatically.

**nginx_be.conf.j2**
```python
# Nginx reverse proxy config for Backend API
server {
    # Listen on port 80 (HTTP)
    listen 80;

    # The domain name this config applies to
    server_name {{ be_domain }};

    location / {
        # Forward all traffic to the backend container on S1
        proxy_pass http://{{ hostvars[groups['appserver'][0]]['ansible_host'] | default(groups['appserver'][0]) }}:{{ be_port }};

        # Pass the real IP of the visitor to the backend
        proxy_set_header X-Real-IP $remote_addr;

        # Pass the original host header
        proxy_set_header Host $host;

        # Pass the forwarded IP chain
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

**nginx_fe.conf.j2**
```python
# Nginx reverse proxy config for Frontend
server {
    # Listen on port 80 (HTTP)
    listen 80;

    # The domain name this config applies to
    server_name {{ fe_domain }};

    location / {
        # Forward all traffic to the frontend container on S1
        proxy_pass http://{{ hostvars[groups['appserver'][0]]['ansible_host'] | default(groups['appserver'][0]) }}:{{ fe_port }};

        # Pass the real IP of the visitor to the backend
        proxy_set_header X-Real-IP $remote_addr;

        # Pass the original host header
        proxy_set_header Host $host;

        # Pass the forwarded IP chain
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

**nginx_grafana.conf.j2**
```python
# Nginx reverse proxy config for Grafana
server {
    # Listen on port 80 (HTTP)
    listen 80;

    # The domain name this config applies to
    server_name {{ grafana_domain }};

    location / {
        # Forward all traffic to Grafana container
        proxy_pass http://localhost:{{ grafana_port }};

        # Pass the real IP of the visitor
        proxy_set_header X-Real-IP $remote_addr;

        # Pass the original host header
        proxy_set_header Host $host;

        # Pass the forwarded IP chain
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        # Required for Grafana WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

**nginx_prometheus.conf.j2**
```python
# Nginx reverse proxy config for Prometheus
server {
    # Listen on port 80 (HTTP)
    listen 80;

    # The domain name this config applies to
    server_name {{ prometheus_domain }};

    location / {
        # Forward all traffic to Prometheus container
        proxy_pass http://localhost:{{ prometheus_port }};

        # Pass the real IP of the visitor
        proxy_set_header X-Real-IP $remote_addr;

        # Pass the original host header
        proxy_set_header Host $host;

        # Pass the forwarded IP chain
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

**prometheus.yml.j2**
```yaml
# Prometheus configuration file
global:
  # How often Prometheus scrapes metrics from targets
  scrape_interval: 15s

  # How often Prometheus evaluates alerting rules
  evaluation_interval: 15s

scrape_configs:
  # Job for scraping Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Job for scraping Node Exporter on S1 (appserver)
  - job_name: 'appserver'
    static_configs:
      - targets: ['{{ groups["appserver"][0] }}:{{ node_exporter_port }}']
        labels:
          instance: 'appserver' # Label to identify this server in Grafana

  # Job for scraping Node Exporter on S2 (gateway)
  - job_name: 'gateway'
    static_configs:
      - targets: ['localhost:{{ node_exporter_port }}']
        labels:
          instance: 'gateway' # Label to identify this server in Grafana
```

### 3. Running the Ansible
Run `ansible-playbook site.yaml`
<img width="976" height="202" alt="image" src="https://github.com/user-attachments/assets/1f018beb-4e16-40be-9cc5-91416654e01d" /><br> *The script running.* <br><br>
After the script already done running, we can check the IP for frontend.<br>
<img width="1148" height="852" alt="image" src="https://github.com/user-attachments/assets/037bffa6-18ab-4a9a-8383-b3191a63b81c" /><br> *Wayshub running.*<br><br>

### 4. Setting Up Prometheus & Grafana
First we need to connect Prometheus to Grafana. Open the Grafana UI `http://SERVER_2_IP:3000`. Then input your Admin ID and Password.
Then go to `Connections → Data Sources → Add data source`. After that, choose Prometheus.
Set URL to `http://SERVER_2_IP:9090`. Then, click `Save & Test`.
<img width="1073" height="605" alt="image" src="https://github.com/user-attachments/assets/94fd4e5e-18e0-4808-bbbc-ef5cfeecc091" /><br><br>
Second, we can costumize our panel.
- Click + > Dashboard > Add Panel > Configure Visualization.
- Select Prometheus as the source.
- Set Query: Switch the toggle from "Builder" to Code, then paste the the query and click run query.




(to be continue)





