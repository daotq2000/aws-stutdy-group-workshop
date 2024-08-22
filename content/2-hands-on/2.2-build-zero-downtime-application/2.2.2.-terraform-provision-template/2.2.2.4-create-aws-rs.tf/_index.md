---
title : "Create aws-resources directory"
description: "Discover how to create a Terraform provision template for building zero downtime applications. Learn step-by-step instructions and best practices for automating your AWS infrastructure"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.2.4 </b> "
image: "images/2.2/teraform-aws.jpeg"

---
## Create aws-resources directory
This directory contains many resources as VPC, Load Balancer, Ec2...
The structure similar as below:
    
    ├── 00_main.tf
    ├── 01_vpc.tf
    ├── 02_subnet.tf
    ├── 03_ig.tf
    ├── 04_routeTable.tf
    ├── 05_sg.tf
    ├── 06_bastionHost.tf
    ├── 07_nlb.tf
    ├── 08_nat.tf
    ├── 09_iam-role.tf
    ├── 10_eks-cluster.tf
    ├── 11_eks-nodes.tf
    ├── 12_registry.tf
    ├── 13_database.tf
    ├── 14_caching.tf
    ├── 15_sqs..tf
    ├── 16_s3.tf
    ├── 17_alb.tf
    ├── 18_cloudfront.tf
    ├── 19_waf.tf
    ├── 20_code-build.tf
    ├── deploy-out.tf
    └── variables.tf


### Create 00_main.tf

    provider "aws" {
    region = "us-east-1" # replace with your AWS region, e.g., "us-east-1"
    }
The main purpose of this file is declare resources aws used in project

### Create 01_vpc.tf
    
    resource "aws_vpc" "vpc-main" {
    vpc-main = var.vpc_cidr_block
    enable_dns_hostnames = true
    enable_dns_support = true
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }

The main purpose of this file is declare VPC, terraform will create VPC with name `vpc-main` and cidr_block we had declared at `variables.tf`.

At `variables.tf` we had define cidr_block

    variable "vpc_cidr_block" {
    type = string
    default = "10.0.0.0/16"
    }
After run terraform plan, apply , it's created vpc have name is `vpc-main` and `vpc_cidr_block` is `10.0.0.0/16`

After terraform created successfully , we have achieve result below:
![VPC created from terraform](/images/2.2/vpc1.png?featherlight=false&width=100pc)
![VPC detail created from terraform](/images/2.2/vpc2.png?featherlight=false&width=100pc)


### Create 02_subnet.tf

    `resource "aws_subnet" "public-subnet-1a" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.1.0/24" # example CIDR block
    availability_zone = "us-east-1a"  # use the availability zone in your region
    
    map_public_ip_on_launch = true # for the bastion host to be accessible from the internet
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1a" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.2.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1a" # use the availability zone in your region
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1b" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.3.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1b" # use a different availability zone
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1c" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.4.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1c" # use another different availability zone
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    #Create subnets group for rds cluster
    resource "aws_db_subnet_group" "subnet_group" {
    name       = "rds-subnet-group"
    subnet_ids = [aws_subnet.private-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id]
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    # Create subnets group for elastic cache
    resource "aws_elasticache_subnet_group" "redis_subnet_group" {
    name       = "redis-subnet-group"
    subnet_ids = [aws_subnet.private-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id]
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    `
This Terraform configuration defines several AWS subnets within a specified VPC. Let's break down each part:

**1. Public Subnet (us-east-1a):** This block defines a public subnet named "public-subnet-1a" with CIDR block "10.0.1.0/24" in availability zone "us-east-1a".
   map_public_ip_on_launch is set to true, indicating that instances launched in this subnet should be assigned public IP addresses.
   It's tagged with descriptive metadata.

**2. Private Subnets (us-east-1a, us-east-1b, us-east-1c):** Similar to the public subnet, but map_public_ip_on_launch is set to false, indicating instances launched in these subnets won't have public IP addresses.

**3. DB Subnet Group:** This block creates a DB subnet group named "rds-subnet-group" for an RDS cluster.
It includes the private subnets created earlier in its subnet_ids.

**4. Elasticache Subnet Group:** Similar to the DB subnet group, this block creates an Elasticache subnet group named "redis-subnet-group".
It also includes the private subnets created earlier in its subnet_ids.

These resources are organized within a VPC and allow for the segmentation of resources based on their need for public accessibility or their association with specific services like RDS or Elasticache. Additionally, they are tagged for better management and identification within the AWS environment.

After terraform created successfully , we have achieve result below:
![Subnet created from terraform](/images/2.2/Subnet1.png?featherlight=false&width=100pc)
![Subnet detail created from terraform](/images/2.2/Subnet2.png?featherlight=false&width=100pc)

### Create 03_ig.tf

    `
    resource "aws_internet_gateway" "gw" {
    vpc_id = aws_vpc.vpc-main.id
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }`

**Resource Type:** This block defines a resource of type aws_internet_gateway. In AWS, an internet gateway allows communication between instances in your VPC and the internet.

**Resource Name:** "gw" is the name given to this specific internet gateway resource block. It's used to refer to this resource elsewhere in the Terraform configuration.

**VPC Association:** vpc_id = aws_vpc.vpc-main.id associates this internet gateway with a specific VPC. aws_vpc.vpc-main.id references the ID of the VPC named "vpc-main", which must have been previously defined in the configuration.

**Tags:** This block defines tags for the internet gateway, providing metadata for identification and organization purposes. In this case, it has two tags: "name" and "description", both indicating that this resource is part of the "terraform project" and is "managed by terraform provisioning", respectively.

Overall, this configuration sets up an internet gateway within a specified VPC and applies descriptive tags for easier management and identification within the AWS environment.
 

After terraform created successfully , we have achieve result below:
![Internet Gateway created from terraform](/images/2.2/igw1.png?featherlight=false&width=100pc)

### Create 04_routeTable.tf
    
    `resource "aws_route_table" "rtb-public" {
    vpc_id = aws_vpc.vpc-main.id
    route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.gw.id
    }
    
    }
    resource "aws_route_table" "rtb-internal" {
    vpc_id = aws_vpc.vpc-main.id
    route {
    cidr_block = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.nat-gw.id
    }
    
    }
    
    # Associate the route table with the public subnet
    resource "aws_route_table_association" "associate_public_1a" {
    subnet_id      = aws_subnet.public-subnet-1a.id
    route_table_id = aws_route_table.rtb-public.id
    
    }
    resource "aws_route_table_association" "associate_private_1a" {
    subnet_id = aws_subnet.private-subnet-1a.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    resource "aws_route_table_association" "associate_private_1b" {
    subnet_id = aws_subnet.private-subnet-1b.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    resource "aws_route_table_association" "associate_private_1c" {
    subnet_id = aws_subnet.private-subnet-1c.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    `
This Terraform configuration sets up route tables for managing network traffic within a VPC, associating different routes with specific subnets. Let's break down each part:

1. Public Route Table: This block defines a route table named "rtb-public" associated with the VPC named "vpc-main".
   It includes a route with CIDR block "0.0.0.0/0", which effectively represents all IP addresses. This route directs traffic to the internet gateway specified by `aws_internet_gateway.gw.id`
2. Internal Route Table: Similar to the public route table, this block defines a route table named "rtb-internal" associated with the VPC named "vpc-main".
   It includes a route with CIDR block "0.0.0.0/0", directing traffic to a NAT gateway specified by aws_nat_gateway.nat-gw.id. This setup is common for private subnets to allow outbound internet access while keeping instances private.
3. Route Table Associations: These blocks associate the previously defined route tables with specific subnets.

For example, "associate_public_1a" associates the "rtb-public" route table with the public subnet "public-subnet-1a".

Similarly, "associate_private_1a", "associate_private_1b", and "associate_private_1c" associate the "rtb-internal" route table with the private subnets "private-subnet-1a", "private-subnet-1b", and "private-subnet-1c", respectively.

After terraform created successfully , we have achieve result below:
![Subnet created from terraform](/images/2.2/rtb1.png?featherlight=false&width=100pc)
![Subnet detail created from terraform](/images/2.2/rtb2.png?featherlight=false&width=100pc)

### Create 05_sg.tf
    
    `resource "aws_security_group" "bastion_sg" {
    name        = "bastion_security_group"
    description = "Allow SSH inbound traffic bastion_sg"
    vpc_id      = aws_vpc.vpc-main.id
    
    ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [
    "0.0.0.0/0", aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block
    ]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    
    
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_security_group" "sg_private_eks_node" {
    name = "Security group private eks node"
    vpc_id = aws_vpc.vpc-main.id
    ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [aws_vpc.vpc-main.cidr_block,aws_subnet.private-subnet-1a.cidr_block,aws_subnet.private-subnet-1b.cidr_block,aws_subnet.private-subnet-1c.cidr_block,]
    }
    egress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    
    }
    resource "aws_security_group" "postgres_aurora_sg" {
    name   = "security_rds_cluster"
    vpc_id = aws_vpc.vpc-main.id
    
    ingress {
    from_port   = 6739
    to_port     = 6739
    protocol    = "tcp"
    cidr_blocks = [
    aws_vpc.vpc-main.cidr_block, aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block
    ]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_security_group" "redis_sg" {
    name   = "redis_sg"
    vpc_id = aws_vpc.vpc-main.id
    ingress {
    from_port = 6379
    to_port = 6379
    protocol = "tcp"
    security_groups = [aws_security_group.bastion_sg.id]
    cidr_blocks = [aws_vpc.vpc-main.cidr_block,aws_subnet.public-subnet-1a.cidr_block,
    aws_subnet.private-subnet-1a.cidr_block,aws_subnet.private-subnet-1b.cidr_block,aws_subnet.private-subnet-1c.cidr_block,]
    }
    egress {
    from_port = 0
    to_port = 0
    protocol = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    #Create SG for Application Load Banlancer
    resource "aws_security_group" "alb_sg" {
    name   = "application_loadbalancer_sg"
    vpc_id = aws_vpc.vpc-main.id
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    }
    `
This Terraform configuration creates several AWS security groups, each with specific inbound and outbound rules to control traffic within a VPC. Let's break down each part:

**Bastion Security Group:**

Named "bastion_sg", it allows SSH (port 22) inbound traffic from anywhere (0.0.0.0/0) as well as from the CIDR blocks of the private and public subnets.

It allows all outbound traffic (0.0.0.0/0) and also allows outbound traffic on ports 6379 (Redis) and 5432 (PostgreSQL) to the CIDR blocks of the private and public subnets.

**Private EKS Node Security Group:**

Named "sg_private_eks_node", it allows SSH (port 22) inbound traffic from the CIDR blocks of the VPC and private subnets.
It allows outbound traffic on ports 6379 (Redis) and 5432 (PostgreSQL) to the CIDR blocks of the private and public subnets and also to any destination.
RDS Cluster Security Group:

Named "postgres_aurora_sg", it allows inbound traffic on port 6739 (for RDS or Aurora) from the CIDR blocks of the VPC and subnets.
It allows outbound traffic to the CIDR blocks of the private and public subnets.
Redis Security Group:

Named "redis_sg", it allows inbound traffic on port 6379 (Redis) only from the security group "bastion_sg" and from the CIDR blocks of the VPC and subnets.
It allows all outbound traffic (0.0.0.0/0).

**Application Load Balancer (ALB) Security Group:**

Named "alb_sg", it allows all inbound and outbound traffic from and to anywhere (0.0.0.0/0) on all ports (-1 for all protocols).
It allows outbound traffic on all ports to the CIDR blocks of the private and public subnets.


##### All security groups are tagged for identification and management purposes. They control traffic flow between different types of resources within the VPC, ensuring security and compliance with network policies.

After terraform created successfully , we have achieve result below:
![Security Group created from terraform](/images/2.2/sg.png?featherlight=false&width=100pc)
![Security Group created from terraform](/images/2.2/sg-detail.png?featherlight=false&width=100pc)

### Create 06_bastionHost.tf

      `resource "aws_instance" "bastion_host" {
      ami           = "ami-0c101f26f147fa7fd" # example AMI, update to the latest
      instance_type = "t2.micro" # example instance type, choose as per need
      subnet_id     = aws_subnet.public-subnet-1a.id
      key_name      = var.ssh_access_key # specify your key pair name
      security_groups = [aws_security_group.bastion_sg.id]
      user_data = <<EOF
      sudo yum update –y
      sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
      sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
      sudo yum upgrade
      sudo dnf install java-17-amazon-corretto -y
      sudo yum install jenkins -y
      sudo systemctl enable jenkins
      sudo systemctl start jenkins
      sudo systemctl status jenkins
      EOF
      
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }`

**Resource Type and Name:** It defines an AWS instance resource named "bastion_host". This resource type represents a virtual server in the Amazon Web Services (AWS) cloud.

**AMI and Instance Type:** The ami parameter specifies the Amazon Machine Image (AMI) ID to use for launching the instance. In this case, it's using a placeholder value (ami-0c101f26f147fa7fd). The instance_type parameter specifies the type of instance to launch, which in this case is a "t2.micro" instance.

**Subnet and Security:** The subnet_id parameter specifies the subnet in which the instance will be launched. It's using a variable (aws_subnet.public-subnet-1a.id), which should contain the actual subnet ID. The security_groups parameter defines the security group(s) associated with the instance, ensuring network security.

**Key Pair:** The key_name parameter specifies the name of the SSH key pair that will be used to connect to the instance. It's using a variable (var.ssh_access_key), which should contain the actual key pair name.

**User Data:** The user_data parameter contains the script that will be executed when the instance is launched. This script updates the system packages, installs Jenkins (a popular automation server), and starts the Jenkins service. It also installs Java 17 using the Amazon Corretto distribution.

**Tags:** Tags are key-value pairs used for organizing and managing AWS resources. In this block, tags are applied to the instance for identification and management purposes.

Overall, this block of code sets up an AWS EC2 instance (bastion host) with necessary configurations and software installations using Terraform. It's a part of an infrastructure provisioning process managed by Terraform.

After terraform created successfully , we have achieve result below:

![Ec2 Bastion Host](/images/2.2/ec2_basion_host.png?featherlight=false&width=100pc)
![Ec2 Bastion Host Details](/images/2.2/ec2_basion_host2.png?featherlight=false&width=100pc)

### Create 07_nat.tf
Next, we need create NAT Gateway to ec2 instance can connect over internet.

      resource "aws_eip" "nat-eip" {
      vpc = true
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      resource "aws_nat_gateway" "nat-gw" {
      allocation_id = aws_eip.nat-eip.id
      subnet_id     = aws_subnet.public-subnet-1a.id
      depends_on = [aws_internet_gateway.gw]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

Elastic IP Resource:

      resource "aws_eip" "nat-eip" {
      vpc = true
      tags = {
      name        = "terraform project"
      description = "managed by terraform provisioning"
      }
      }

This block defines an AWS Elastic IP (EIP) resource named "nat-eip". The vpc = true line specifies that this EIP is to be associated with a VPC (Virtual Private Cloud). Tags are also assigned to the EIP for identification and management purposes.

NAT Gateway Resource:
      
      resource "aws_nat_gateway" "nat-gw" {
      allocation_id = aws_eip.nat-eip.id
      subnet_id     = aws_subnet.public-subnet-1a.id
      depends_on    = [aws_internet_gateway.gw]
      tags = {
      name        = "terraform project"
      description = "managed by terraform provisioning"
      }
      }

This block defines an AWS NAT Gateway resource named "nat-gw". The allocation_id parameter specifies the Elastic IP allocation ID associated with the NAT Gateway, which is obtained from the previously defined Elastic IP resource (aws_eip.nat-eip.id). The subnet_id parameter specifies the subnet in which the NAT Gateway will be deployed. The depends_on parameter ensures that the NAT Gateway is created only after the associated internet gateway (aws_internet_gateway.gw) is created. Tags are also applied to the NAT Gateway for identification and management purposes.

Overall, this block of code provisions an Elastic IP and a NAT Gateway in an AWS environment using Terraform. The NAT Gateway facilitates outbound internet access for resources in private subnets within the VPC.

After terraform created successfully , we have achieve result below:

![Elastic IP Address ](/images/2.2/eip1.png?featherlight=false&width=100pc)
![Nat Gateway  ](/images/2.2/eip2.png?featherlight=false&width=100pc)
## Create 08_iam-role.tf
      
      ### This block defines an IAM role named "eks_role" for the EKS cluster. It specifies a policy allowing the EKS service principal (eks.amazonaws.com) to assume this role.
      resource "aws_iam_role" "role-eks" {
      name = "eks_role"
      
      assume_role_policy = jsonencode({
      "Version": "2012-10-17",
      "Statement": [
      {
      "Effect": "Allow",
      "Principal": {
      "Service": [
      "eks.amazonaws.com"
      ]
      },
      "Action": "sts:AssumeRole"
      }
      ]
      })
      }
      # This block attaches the "AmazonEKSClusterPolicy" IAM policy to the IAM role created for the EKS cluster. This policy grants permissions necessary for managing EKS cluster resources.
      resource "aws_iam_role_policy_attachment" "eks_attach" {
      role       = aws_iam_role.role-eks.name
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
      
      }
      # This block defines an IAM role named "eks-node-group-nodes" for the EKS worker nodes. It specifies a policy allowing EC2 instances (ec2.amazonaws.com) to assume this role.
      resource "aws_iam_role" "nodes" {
      name = "eks-node-group-nodes"
      
      assume_role_policy = jsonencode({
      Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
      Service = "ec2.amazonaws.com"
      }
      }]
      Version = "2012-10-17"
      })
      
      }
      
      #These blocks attach policies (AmazonEKSWorkerNodePolicy, AmazonEKS_CNI_Policy, AmazonEC2ContainerRegistryReadOnly) to the IAM role created for the EKS worker nodes. These policies grant permissions necessary for EKS node operation, networking, and accessing the Amazon ECR (Elastic Container Registry) for Docker image storage.
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEKSWorkerNodePolicy" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
      role       = aws_iam_role.nodes.name
      }
      
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEKS_CNI_Policy" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
      role       = aws_iam_role.nodes.name
      
      }
      
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEC2ContainerRegistryReadOnly" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
      role       = aws_iam_role.nodes.name
      }

After terraform created successfully , we have achieve result below:

![IAM Dashboard](/images/2.2/dashboard-iam.png?featherlight=false&width=100pc)
![Roles List Dashboard](/images/2.2/role-lists.png?featherlight=false&width=100pc)
![Policy  Dashboard](/images/2.2/role-lists2.png?featherlight=false&width=100pc)
![Policy Dashboard](/images/2.2/policy.png?featherlight=false&width=100pc)

## Create 09_eks-cluster.tf
      #This block defines an AWS EKS cluster resource with the name "eks_cluster".
      resource "aws_eks_cluster" "eks_cluster" {
      #Specifies the name of the EKS cluster, which in this case is "Eks-cluster".
      name     = "Eks-cluster"
      #Specifies the ARN (Amazon Resource Name) of the IAM role used by the EKS cluster. It's referencing the IAM role defined earlier (aws_iam_role.role-eks) and accessing its ARN.
      role_arn = aws_iam_role.role-eks.arn
      #Specifies the VPC configuration for the EKS cluster. It defines the subnets in which the EKS cluster's resources will be launched. The subnet_ids parameter contains a list of private subnet IDs where the EKS cluster's worker nodes will reside.
      vpc_config {
      subnet_ids = [
      aws_subnet.private-subnet-1a.id,
      aws_subnet.private-subnet-1b.id,
      aws_subnet.private-subnet-1c.id
      ]
      }
      #Specifies that the creation of the EKS cluster resource depends on the successful attachment of the IAM policy defined earlier (aws_iam_role_policy_attachment.eks_attach). This ensures that the necessary permissions are in place before attempting to create the EKS cluster.
      depends_on = [aws_iam_role_policy_attachment.eks_attach]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

After terraform created successfully , we have achieve result below:
![EKS Cluster](/images/2.2/eks.png?featherlight=false&width=100pc)
![EKS Cluster Detail](/images/2.2/Eks-detail.png?featherlight=false&width=100pc)

## Create 10_eks-nodes.tf
      ## This block defines an AWS EKS node group resource with the name "private-nodes".
      resource "aws_eks_node_group" "private-nodes" {
      ## Specifies the name of the EKS cluster to which this node group belongs. It references the name of the EKS cluster created earlier (aws_eks_cluster.eks_cluster.name).
      cluster_name    = aws_eks_cluster.eks_cluster.name
      ## Specifies the name of the EKS node group.
      node_group_name = "private-nodes"
      ## Specifies the ARN (Amazon Resource Name) of the IAM role used by the EKS node group. It references the IAM role defined earlier (aws_iam_role.nodes.arn).
      node_role_arn   = aws_iam_role.nodes.arn
      ## Specifies the Amazon Machine Image (AMI) type for the nodes. In this case, it's set to "AL2_x86_64" which indicates Amazon Linux 2.
      ami_type = "AL2_x86_64"
      ## Specifies the IDs of the subnets in which the EC2 instances (nodes) of this node group will be 
      subnet_ids = [
      aws_subnet.private-subnet-1a.id,
      aws_subnet.private-subnet-1b.id,
      aws_subnet.private-subnet-1c.id,
      ]
      
      capacity_type  = "SPOT"
      ## Defines the capacity type for the instances (in this case, "SPOT" instances), as well as the instance types to be used (here, "t3.medium").
      instance_types = ["t3.medium"]
      ##Specifies the scaling configuration for the node group, including the desired, maximum, and minimum number of nodes.
      scaling_config {
      desired_size = 1
      max_size     = 5
      min_size     = 0
      }
      ## Configures remote access to the nodes using SSH key (ec2_ssh_key) and specifies the security group to apply to the nodes.
      remote_access {
      ec2_ssh_key = var.ssh_access_key
      source_security_group_ids = [aws_security_group.sg_private_eks_node.id]
      }
      update_config {
      max_unavailable = 1
      }
      
      labels = {
      role = "Eks-cluster"
      }
      ## Specifies that the creation of the EKS node group resource depends on the successful attachment of the IAM policies defined earlier.
      depends_on = [
      aws_iam_role_policy_attachment.nodes-AmazonEKSWorkerNodePolicy,
      aws_iam_role_policy_attachment.nodes-AmazonEKS_CNI_Policy,
      aws_iam_role_policy_attachment.nodes-AmazonEC2ContainerRegistryReadOnly,
      ]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

After terraform created successfully , we have achieve result below:
![EKS Private Node ](/images/2.2/eks-private-node.png?featherlight=false&width=100pc)
## Create 11_registry.tf
      ## this block of code provisions an Amazon ECR repository named "eks-project" with specified configurations such as name and tags using Terraform. This repository can be used to store Docker /images and manage containerized applications within your AWS infrastructure.
      resource "aws_ecr_repository" "ecr_repository" {
      name = "eks-project"
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/ecr.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/ecr2.png?featherlight=false&width=100pc)
## Create 12_database.tf
      resource "aws_rds_cluster" "aurora-postgresql-cluster" {
      cluster_identifier = "aurora-postgresql-cluster"
      engine             = "aurora-postgresql"
      engine_mode        = "provisioned"
      engine_version     = "13.7"
      database_name      = "test"
      master_username    = "test"
      master_password    = "sadalkdakl232"
      vpc_security_group_ids = [aws_security_group.rds_cluster_sg.id] # Replace with your security group IDs
      db_subnet_group_name   = aws_db_subnet_group.subnet_group.name
      storage_encrypted  = true
      skip_final_snapshot = true
      serverlessv2_scaling_configuration {
      max_capacity = 1.0
      min_capacity = 0.5
      }
      }
      
      resource "aws_rds_cluster_instance" "aurora-postgresql-cluster" {
      for_each = var.az
      identifier = "instance-${each.value}"
      cluster_identifier = aws_rds_cluster.aurora-postgresql-cluster.id
      instance_class     = "db.serverless"
      engine             = aws_rds_cluster.aurora-postgresql-cluster.engine
      engine_version     = aws_rds_cluster.aurora-postgresql-cluster.engine_version
      availability_zone = each.value
      }

This block of code provisions an Amazon Aurora PostgreSQL database cluster with serverless v2 scaling configuration, including cluster configuration and instance configuration, using Terraform.

After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/sqs1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/ecr2.png?featherlight=false&width=100pc)
## Create 13_caching.tf
      resource "aws_elasticache_replication_group" "redis" {
      replication_group_id          = "redis-cluster"
      description = "redis replication"
      node_type                     = "cache.t3.micro"
      num_cache_clusters         = 1
      engine                        = "redis"
      engine_version                = "6.0"
      automatic_failover_enabled = false
      parameter_group_name          = "default.redis6.x"
      subnet_group_name             = aws_elasticache_subnet_group.redis_subnet_group.name
      security_group_ids            = [aws_security_group.redis_sg.id]
      auto_minor_version_upgrade    = true
      #   automatic_failover_enabled    = true
      at_rest_encryption_enabled    = false
      transit_encryption_enabled    = false
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      depends_on = [
      aws_eks_cluster.eks_cluster,
      aws_eks_node_group.private-nodes
      ]
      }
This block of code provisions an Amazon ElastiCache Redis replication group with specified configurations using Terraform, ensuring dependencies are met with the EKS cluster and node group.

After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/redis1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/redis2.png?featherlight=false&width=100pc)
## Create 14_sqs..tf
      resource "aws_sqs_queue" "sqs_iaac" {
      name = "sqs_queue"
      delay_seconds = 9
      max_message_size = 2048
      message_retention_seconds = 86400
      receive_wait_time_seconds = 10
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

This block of code provisions an Amazon SQS queue with specified configurations such as name, delay seconds, max message size, message retention seconds, receive wait time seconds, and tags using Terraform. This queue can be used to decouple the components of a distributed application by enabling asynchronous communication between them.
After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/sqs1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/sqs2.png?featherlight=false&width=100pc)
##  Create 15_s3.tf

      resource "aws_s3_bucket" "eks-project-front-end-source" {
      bucket = "eks-project-front-end-source"
      }
      resource "aws_s3_bucket" "eks-project-bucket-data" {
      bucket = "eks-project-bucket-data"
      }
There blocks of code provision two separate Amazon S3 buckets with specific names using Terraform. S3 buckets are used for storing and managing objects (such as files and data) in the AWS cloud. Each bucket can be uniquely identified by its name and can be configured with various settings and permissions as needed for specific use cases.
After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/s3.png?featherlight=false&width=100pc)
## Create 16_alb.tf
      
      resource "aws_lb" "alb" {
      name               = "application-loadbalancer"
      internal           = true
      load_balancer_type = "application"
      security_groups    = [aws_security_group.alb_sg.id]
      subnets            = [aws_subnet.public-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id,]
      enable_deletion_protection = false
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
This block of code provisions an AWS Application Load Balancer with specified configurations such as name, internal setting, type, security groups, subnets, deletion protection, and tags using Terraform. The ALB distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple availability zones to ensure high availability and fault tolerance for your applications.
After terraform created successfully , we have achieve result below:
![ECR ](/images/2.2/Alb1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/Alb2.png?featherlight=false&width=100pc)
## Create 17_cloudfront.tf
      resource "aws_cloudfront_origin_access_identity" "cloudfront_s3" {
      comment = "Cloudfront distribution"
      }
      resource "aws_cloudfront_distribution" "cloudfront_s3_distribution" {
      origin {
      domain_name = aws_s3_bucket.eks-project-front-end-source.bucket_domain_name
      origin_id   = aws_s3_bucket.eks-project-front-end-source.id
      s3_origin_config {
      origin_access_identity = aws_cloudfront_origin_access_identity.cloudfront_s3.cloudfront_access_identity_path
      }
      }
      origin {
      domain_name = aws_lb.alb.dns_name
      origin_id   = aws_lb.alb.id
      custom_origin_config {
      http_port              = 80
      https_port             = 443
      origin_protocol_policy = "https-only"
      origin_ssl_protocols   = ["TLSv1.2"]
      }
      }
      enabled             = true
      default_root_object = "index.html"
      default_cache_behavior {
      allowed_methods  = ["GET", "HEAD"]
      cached_methods   = ["GET", "HEAD"]
      target_origin_id = aws_s3_bucket.eks-project-front-end-source.id
      forwarded_values {
      query_string = true
      cookies {
      forward = "none"
      }
      }
      viewer_protocol_policy = "https-only"
      min_ttl                = 0
      default_ttl            = 3600
      max_ttl                = 86400
      }
      price_class = "PriceClass_100"
      restrictions {
      geo_restriction {
      restriction_type = "none"
      }
      }
      
      viewer_certificate {
      cloudfront_default_certificate = true
      }
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

This block of code provisions an AWS CloudFront distribution with specified configurations such as origins, cache behavior, price class, restrictions, and tags using Terraform. This distribution can be used to cache and distribute content globally to users, improving the performance and availability of web applications and content.
After terraform created successfully , we have achieve result below:
![CloudFront ](/images/2.2/CloudFront.png?featherlight=false&width=100pc)
![CloudFront ](/images/2.2/CloudFront2.png?featherlight=false&width=100pc)
## Create 18_waf.tf
      resource "aws_wafv2_web_acl" "WafWebAcl" {
      name  = "wafv2-web-acl"
      scope = "REGIONAL"
      
      default_action {
      allow {
      }
      }
      
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "WAF_Common_Protections"
      sampled_requests_enabled   = true
      }
      
      rule {
      name     = "AWS-AWSManagedRulesCommonRuleSet"
      priority = 0
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesCommonRuleSet"
      vendor_name = "AWS"
      
            }
          }
          visibility_config {
            cloudwatch_metrics_enabled = true
            metric_name                = "AWS-AWSManagedRulesCommonRuleSet"
            sampled_requests_enabled   = true
          }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesLinuxRuleSet"
      priority = 1
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesLinuxRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesLinuxRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesAmazonIpReputationList"
      priority = 2
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesAmazonIpReputationList"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesAmazonIpReputationList"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesAnonymousIpList"
      priority = 3
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesAnonymousIpList"
      vendor_name = "AWS"
      
            }
          }
          visibility_config {
            cloudwatch_metrics_enabled = true
            metric_name                = "AWS-AWSManagedRulesAnonymousIpList"
            sampled_requests_enabled   = true
          }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesKnownBadInputsRuleSet"
      priority = 4
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesKnownBadInputsRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesKnownBadInputsRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesUnixRuleSet"
      priority = 5
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesUnixRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesUnixRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesWindowsRuleSet"
      priority = 6
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesWindowsRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesWindowsRuleSet"
      sampled_requests_enabled   = true
      }
      }
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      
      resource "aws_cloudwatch_log_group" "WafWebAclLoggroup" {
      name              = "aws-waf-logs-wafv2-web-acl"
      retention_in_days = 30
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      
      resource "aws_wafv2_web_acl_logging_configuration" "WafWebAclLogging" {
      log_destination_configs = [aws_cloudwatch_log_group.WafWebAclLoggroup.arn]
      resource_arn            = aws_wafv2_web_acl.WafWebAcl.arn
      depends_on = [
      aws_wafv2_web_acl.WafWebAcl,
      aws_cloudwatch_log_group.WafWebAclLoggroup
      ]
      
      }
      
      resource "aws_wafv2_web_acl_association" "WafWebAclAssociation" {
      resource_arn = aws_lb.alb.arn
      web_acl_arn  = aws_wafv2_web_acl.WafWebAcl.arn
      depends_on = [
      aws_wafv2_web_acl.WafWebAcl,
      aws_cloudwatch_log_group.WafWebAclLoggroup
      ]
      }
This code block provisions AWS WAFv2 resources including web ACLs, logging configurations, and associations with associated CloudWatch logging for monitoring and logging web ACL activity, thereby enhancing the security posture of web applications deployed behind the Application Load Balancer.

After terraform created successfully , we have achieve result below:
![CloudFront ](/images/2.2/Waf1.png?featherlight=false&width=100pc)
![CloudFront ](/images/2.2/Waf2.png?featherlight=false&width=100pc)



