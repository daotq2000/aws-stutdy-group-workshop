---
title : "Create variables.tf"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.2.3 </b> "
---
## Create file variables.tf
    
    variable "vpc_cidr_block" {
    type = string
    default = "10.0.0.0/16"
    }
    variable "ssh_access_key" {
    type = string
    default = "public-bastion-host"
    }
    variable "aws_role_code_pipeline" {
    type = string
    default = "arn:aws:iam::846338211683:role/service-role/aws-code-pipeline"
    }
    variable "registry_credential" {
    type = string
    default = "registry_credential"
    }
    variable "registry_credential_provider" {
    type = string
    default = "registry_credential_provider"
    }
    variable "repository" {
    type = map
    default = {
    source: "GITHUB",
    url:"https://github.com/daotq2000/aws-spring-sqs-queue.git"
    }
    }
    variable "region" {
    type = string
    default = "us-east1"
    }
    variable "az" {
    type = set(string)
    default = ["us-east-1a","us-east-1b","us-east-1c"]
    }
## Explain


This code snippet is written in HashiCorp Configuration Language (HCL) and is defining several variables with default values. Let's break it down:

variable **"vpc_cidr_block":** This line defines a variable named "vpc_cidr_block" which represents the CIDR block for the VPC (Virtual Private Cloud) to be created. It is of type string and has a default value of "10.0.0.0/16".

variable **"ssh_access_key":** This line defines a variable named "ssh_access_key" which represents the SSH access key for the Bastion host. It is of type string and has a default value of "public-bastion-host".

variable **"aws_role_code_pipeline":** This line defines a variable named "aws_role_code_pipeline" which represents the IAM role for AWS CodePipeline. It is of type string and has a default value of "arn:aws:iam::846338211683:role/service-role/aws-code-pipeline".

variable **"registry_credential":** This line defines a variable named "registry_credential" which represents credentials for accessing a registry. It is of type string and has a default value of "registry_credential".

variable **"registry_credential_provider"**: This line defines a variable named "registry_credential_provider" which represents the provider for registry credentials. It is of type string and has a default value of "registry_credential_provider".

variable **"repository"**: This line defines a variable named "repository" which represents information about a repository. It is of type map and has a default value of a map containing two key-value pairs: "source" with the value "GITHUB" and "url" with the value "https://github.com/daotq2000/aws-spring-sqs-queue.git".

variable **"region":** This line defines a variable named "region" which represents the AWS region to deploy resources in. It is of type string and has a default value of "us-east1".

variable **"az":** This line defines a variable named "az" which represents the availability zones within the specified region. It is of type set(string) (a set of strings) and has a default value of a set containing three strings: "us-east-1a", "us-east-1b", and "us-east-1c".

These variables can be used later in the Terraform configuration to parameterize the resource definitions and make the configuration more flexible and reusable. Users can override these default values when running Terraform commands if necessary.




