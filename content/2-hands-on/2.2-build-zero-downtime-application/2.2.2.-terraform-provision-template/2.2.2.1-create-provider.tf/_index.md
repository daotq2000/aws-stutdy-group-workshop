---
title : "Create provider.tf"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.2.1 </b> "
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