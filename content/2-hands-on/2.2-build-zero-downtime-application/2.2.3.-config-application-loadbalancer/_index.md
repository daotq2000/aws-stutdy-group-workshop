---
title : "Configuring AWS Application Load Balancer for Zero Downtime Applications"
description: "Learn how to configure AWS Application Load Balancer for zero downtime applications. This guide covers setup, best practices, and optimization tips for ensuring high availability and reliability"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.3 </b> "
image: "/images/2.2/alb.png"
---
Although we have set up resources AWS by terraform, there are contains many component, with big complexity but 
we even need to config some resource to application working without errors.
## 1. Config security group to communicate between private resources
The mean this step is enable communication between resource in private subnet, by default resources in private subnet will 
refuse any connection if we don't have config security.

Step config is go to **AWS console => EC2 => Security Group**
![create_admin_user.png](/images/2.4-config/sg-eks-remote.png)
###  Edit inbound rules
Click to Edit inbound rules
![create_admin_user.png](/images/2.4-config/inbound.png)
![create_admin_user.png](/images/2.4-config/inbound1.png)

Inbound configure successfully
![create_admin_user.png](/images/2.4-config/config-inboumd-success.png)

###  Edit outbound rules
![create_admin_user.png](/images/2.4-config/outbound.png)
![create_admin_user.png](/images/2.4-config/outboundSuccessfully.png)

## 2. Config Target Group For Application Load Balancer
Config target for load balancer, load balancer will forward request from cloudfront to Nginx-Controller inside 
EKS Cluster.
Follow steps: 
1. Click to `alb-target-group`
![create_admin_user.png](/images/2.4-config/configALB.png)
2. Register target group
![create_admin_user.png](/images/2.4-config/config-targetGroup.png)
- Port `30080` is NodePort of Nginx Controller.
3. At tab Health checks, click to `Edit`
![create_admin_user.png](/images/2.4-config/success-config-alb.png)
![create_admin_user.png](/images/2.4-config/config-heathy.png)
4. Config heath check successfully
![create_admin_user.png](/images/2.4-config/success-config-alb.png)

After successfully config health check, application load balancer will forward request to nginx-controller.
