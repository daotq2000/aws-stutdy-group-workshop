---
title : "Tạo tệp provider.tf"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.2.1 </b> "
---
## Tạo tệp provider.tf
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
## Giải thích
Block code ở trên khai báo terraform của nhà cung cấp sẽ tương tác với chúng như GCP, AWS, Kubernetes