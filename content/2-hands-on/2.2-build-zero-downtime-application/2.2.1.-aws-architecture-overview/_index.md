---
title : "AWS Architecture Overview"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2.1 </b> "
---
 

![AWS DESIGN ARCHITECTURE](/images/1/ArchitechtureDesign.svg?featherlight=false&width=100pc)

### Analytics Design AWS Architecture
The architecture depicted in the image represents an AWS cloud computing solution with various components, each with a specific purpose.
Here are the components and their meanings:

**Web Browser:** Represents the client-side aspect where users interact with the system.

**Web Application Firewall (WAF):**
Protects the application from web-based threats.

**CloudFront:**
AWS's content delivery network (CDN) that distributes content to users with high availability and high speed.

**Application Load Balancer (ALB):**
Distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones.

**Amazon EKS Cluster:**
Managed Kubernetes service for running containerized applications.

**Private Subnets with EC2 Managed Node Groups:**
Secure and scalable hosting for EC2 instances that form part of the EKS cluster.

**Primary RDS Database:** Relational database service for primary data management.

**Replica RDS Database:** Secondary relational database used for failover and read scalability.

**Redis Cache Cluster:** In-memory data store used for caching and as a database, message broker, and queue for improving the performance of applications.
On the infrastructure management and operational side:

**Terraform:** Infrastructure as code tool for building, changing, and versioning infrastructure.

**CloudWatch:** Monitoring service for AWS cloud resources and applications.

**CloudTrail:** Service that provides a record of actions taken by a user, role, or AWS service.

**Grafana:** Analytics and interactive visualization web application that provides charts, graphs, and alerts.

**Elastic Container Registry (ECR):** Docker container registry for storing, managing, and deploying container /images.

**Amazon SQS queue:** Message queuing service to decouple and scale microservices, distributed systems, and serverless applications.
And for storage and deployment:

**S3:** Scalable storage in the AWS cloud.

**CodeCommit:** Source control service that hosts secure Git-based repositories.

Each of these components plays a critical role in a comprehensive, scalable, and secure cloud infrastructure that harnesses the power of AWS services.**** Service that provides a record of actions taken by a user, role, or AWS service.
