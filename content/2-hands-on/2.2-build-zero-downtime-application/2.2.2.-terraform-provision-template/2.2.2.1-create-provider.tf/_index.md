---
title : "Create provider.tf"
description: "Discover how to create a Terraform provision template for building zero downtime applications. Learn step-by-step instructions and best practices for automating your AWS infrastructure"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.2.1 </b> "
image: "images/2.2/teraform-aws.jpeg"

---
## Create file provider.tf
    terraform {
    required_providers {
    aws = {
    source = "hashicorp/aws"
    version = "5.44.0"
    }
    kubernetes = {
    source = "hashicorp/kubernetes"
    version = ">= 2.3.2"
    }
    kubectl = {
    source  = "gavinbunney/kubectl"
    version = ">= 1.11.2"
    }
    helm = {
    source = "hashicorp/helm"
    version = ">= 2.2.0"
    }
    }
    }
    provider "aws" {
    region = "us-east-1"
    }
## Explain
Block code above declare provider terraform will interactive of them such as GCP, AWS, Kubernetes