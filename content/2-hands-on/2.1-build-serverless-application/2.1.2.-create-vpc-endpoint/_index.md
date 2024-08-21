---
title : "How to Create a VPC Endpoint in AWS: A Step-by-Step Tutorial"
description : "Learn how to create a VPC endpoint in AWS with this detailed step-by-step tutorial. Understand the process and benefits of setting up VPC endpoints for secure and efficient network connections."
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.2 </b> "
image: "/images/2.2/vpcs-diagram.png"
---


#### Overview

![Build-Serverless-Application](/images/1/LambdaArchitechture.svg?featherlight=false&width=100pc)

We see VPC Endpoint connect between Lambda function and DynamoDB.
## Introduction to VPC Endpoints
**Why need a VPC endpoint to connect it?**

+ VPC Isolation: When Lambda functions are configured to run within a VPC, they are isolated from the internet and can only access resources within the VPC or those reachable through VPC endpoints. This ensures a higher level of security by restricting external access.
+ DynamoDB Endpoint: DynamoDB is an AWS service that resides on the AWS network infrastructure. When accessed from within a VPC, Lambda functions need to communicate with DynamoDB through an interface known as a VPC endpoint. A VPC endpoint acts as a gateway that enables private communication between resources in your VPC and AWS services like DynamoDB without traversing the internet.
Private Subnet Access: Placing both Lambda functions and DynamoDB in the same private subnet ensures that communication between them remains private and does not traverse the public internet. This further enhances the security posture of the application.
By creating a VPC endpoint for DynamoDB within your VPC, Lambda functions in the same VPC can securely communicate with DynamoDB without the need for internet access. This setup ensures that data transmission between Lambda and DynamoDB remains secure and private, while also optimizing network performance by avoiding unnecessary internet routing.

**In summary, while Lambda functions and DynamoDB can coexist in the same private subnet within a VPC, Lambda functions require a VPC endpoint to establish connectivity with DynamoDB when operating within a VPC environment. This ensures secure and private communication between the two services without internet access to doesn't pay fee for internet outbound from AWS Pricing Model.**

#### Hands-on
Let’s **Create VPC Endpoint** follow instructions below:
![CreateVPCEndPoint2](/images/2/CreateVPCEndPoint2.jpeg?featherlight=false&width=100pc)
+ Go to https://us-east-1.console.aws.amazon.com/vpcconsole/home?region=us-east-1#Endpoints: choose **Create Endpoint**
![CreateVPCEndPoint3](/images/2/CreateVPCEndPoint3.jpeg?featherlight=false&width=100pc)

**Choose Option below like picture:** 

+ Name tag – optional: Fill name of VPC endpoint
+ Service category: AWS services
+ Services: com.amazonaws.us-east-1.dynamodb
+ VPC: select vpc we created at [Step 2.1.1](../2.1.1.-create-vpc-subnet-route-table/)

**Conclusion: This guide will walk you through the process of creating a VPC endpoint in AWS. A VPC endpoint allows you to connect your VPC directly to AWS services, enhancing security and performance. Follow these steps to set up your VPC endpoint and ensure efficient network connections.**