---
title : "Create VPC, Subnet, Route Table"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1.1 </b> "
---


#### Overview

Let’s delve into the concepts of VPC, Subnet, Route Table, and Security Group:

**1.1 Virtual Private Cloud (VPC):**

+ A Virtual Private Cloud (VPC) is a virtual network environment provided by Amazon Web Services (AWS).

+ It allows you to provision a logically isolated section of the AWS cloud where you can launch AWS resources such as EC2 instances, RDS databases, and Lambda functions.

+ With a VPC, you have complete control over your network environment, including IP address ranges, subnets, route tables, and network gateways.

+ VPCs provide security by enabling you to define network access control policies, set up VPN connections, and use security groups and network ACLs to restrict traffic.

**1.2 Subnet:**

+ A subnet is a segmented portion of a VPC’s IP address range in which you can place AWS resources.

+ Subnets allow you to organize resources within a VPC and define separate network segments with their own routing configurations, access control policies, and availability zones.

+ Each subnet is associated with a specific availability zone (AZ) within a region, and resources deployed in that subnet reside in the corresponding AZ.

+ Subnets can be public or private, depending on whether they have a route to an internet gateway.

**1.3 Route Table:**

+ A route table is a set of rules (routes) that determine where network traffic is directed within a VPC.

+ Each subnet in a VPC must be associated with a route table, which defines how traffic is routed in and out of the subnet.

+ Route tables contain routes to specific destinations, such as internet gateways, virtual private gateways (for VPN connections), VPC peering connections, or NAT gateways.

+ By configuring routes in the route table, you can control how traffic flows between subnets within the VPC and to external networks.

**1.4 Security Group:**

+ A security group acts as a virtual firewall for your instances to control inbound and outbound traffic.

+ It operates at the instance level and can be associated with multiple instances within a VPC.

+ Security groups allow you to define rules that permit or deny traffic based on protocols, ports, and IP addresses.

+ By default, all inbound traffic is denied, and all outbound traffic is allowed, but you can customize these rules to meet your specific security requirements.

+ Security groups are stateful, meaning that if you allow traffic for a specific protocol and port, the return traffic is automatically allowed regardless of the outbound rules.

**In summary, VPCs provide isolated network environments within AWS, subnets allow you to segment your VPC into smaller networks, route tables determine how traffic is routed within the VPC, and security groups control the traffic flow to and from your instances. These components work together to provide a secure and scalable networking infrastructure in the AWS cloud.**

#### Hands-on
Let’s **Create VPC** follow instructions below:
![CreateVPC1](/images/2/CreateVPC1.jpeg?featherlight=false&width=100pc)
+ Go to https://us-east-1.console.aws.amazon.com/vpcconsole/home?region=us-east-1#vpcs choose **Create VPC**
![CreateVPC2](/images/2/CreateVPC2.jpeg?featherlight=false&width=100pc)
+ Select VPC and More
+ Select IPv4 CIDR: 10.0.0.0/16
+ Choose AZ: 1
+ Choose number public subnet 1
+ Choose private subnet 1
+ Choose NAT gateway: none, because we are connecting within AWS resources, so we don’t need any connection to the internet. NAT gateway suitable for third party resources on the internet
![CreateVPC3](/images/2/CreateVPC3.jpeg?featherlight=false&width=100pc)
After fulfilling these options click to **Create VPC**. Waiting a minute to resources provisioning, After done, we have a vpc like this picture above.

