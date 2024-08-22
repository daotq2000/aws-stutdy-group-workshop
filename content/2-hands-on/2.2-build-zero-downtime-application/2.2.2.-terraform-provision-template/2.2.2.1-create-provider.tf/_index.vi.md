---
title : "Tạo tệp provider.tf"
description: "Khám phá cách tạo mẫu cung cấp Terraform để xây dựng các ứng dụng  Zero Downtime Applications. Tìm hiểu hướng dẫn từng bước và các biện pháp thực hành tốt nhất để tự động hóa cơ sở hạ tầng AWS của bạn"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.2.1 </b> "
image: "images/2.2/teraform-aws.jpeg"

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