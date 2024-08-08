---
title : "Testing Application"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.2.6 </b> "
---
Next, we need testing APIs have been deploy at previous step, before to testing we need follow these steps:

## 1. Add Host to DNS Resolution
The purpose this step for you haven't real domain, you can fake domain by add some config in Helm Values at step [Config project HelmValues](../2.2.5.-cicd-jenkins/2.2.4.1-set-up-helmchart-project/) like config below:

    ingress:
    enabled: true
    annotations:
    kubernetes.io/ingress.class: public
    hosts:
    - host: eks.bootcamp.net
    paths:
      - path: /
      pathType: Prefix

Next, we need go to Application Load Balancer at Amazon Console => EC2 => Load balancers to get DNS of load balancer.

![SSH Successfully to Bastion Host](/images/testing/getLoadBalancer.png?featherlight=false&width=100pc)

Next, let's copy that and use command below to get IP Address of load balancer:

    ping {your_loadBalancer_dns}

![SSH Successfully to Bastion Host](/images/testing/ping.png?featherlight=false&width=100pc)

Next, let's copy IP Address to add in file host to dns resolution

![SSH Successfully to Bastion Host](/images/testing/addHost.png?featherlight=false&width=100pc)
![SSH Successfully to Bastion Host](/images/testing/addHos2t.png?featherlight=false&width=100pc)

After done, let using this domain to testing APIs.

## 2. Testing

### 2.1 Testing API Create Product
![SSH Successfully to Bastion Host](/images/testing/create-prd.png?featherlight=false&width=100pc)
After created, let's check log on service are running, we can see insert command have been executed.
![SSH Successfully to Bastion Host](/images/testing/log-create-prd.png?featherlight=false&width=100pc)

### 2.2 Testing API Upload Image
The image will upload on s3 budget
![SSH Successfully to Bastion Host](/images/testing/uploadImageAPI.png?featherlight=false&width=100pc)
![SSH Successfully to Bastion Host](/images/testing/s3.png?featherlight=false&width=100pc)
### 2.3 Testing API load all products
![SSH Successfully to Bastion Host](/images/testing/listAllPrd.png?featherlight=false&width=100pc)
### 2.4 Testing API Create Order
1. To create order, we need create customer first
   ![SSH Successfully to Bastion Host](/images/testing/customer.png?featherlight=false&width=100pc)
2. Create order with request below
   ![SSH Successfully to Bastion Host](/images/testing/create-orders.png?featherlight=false&width=100pc)
 Order created will send to SQS queue
   ![SSH Successfully to Bastion Host](/images/testing/send-to-sqs.png?featherlight=false&width=100pc)
 Next, we need to run API to consume message from SQS, it will receive message from SQS queue and insert order to database
   ![SSH Successfully to Bastion Host](/images/testing/send-to-sqs.png?featherlight=false&width=100pc)
   ![SSH Successfully to Bastion Host](/images/testing/log-order.png?featherlight=false&width=100pc)
### 2.5 Testing API fetch list order
![SSH Successfully to Bastion Host](/images/testing/fetch-orders.png?featherlight=false&width=100pc)
SQL generate by Hibernate to query order from database:
![SSH Successfully to Bastion Host](/images/testing/log-fetch-orders.png?featherlight=false&width=100pc)
