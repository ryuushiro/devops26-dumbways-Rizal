# Infrastructure as Code (IaC)
## 1. IaC with Terraform in WSL to AWS
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
------
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
------
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
------
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
------
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






