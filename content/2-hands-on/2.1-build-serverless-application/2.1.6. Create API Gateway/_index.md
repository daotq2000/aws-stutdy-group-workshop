---
title : "How to Create and Configure AWS API Gateway for Serverless Applications"
description : "Learn how to create and configure AWS API Gateway for serverless applications. Follow our comprehensive guide to set up a scalable API with ease"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.1.6 </b> "
image: "images/2/apigw.png"
---
## What is API Gateway?
AWS API Gateway is a fully managed service that makes it easy to create, publish, maintain, monitor, and secure APIs at any scale. In this guide, we'll walk you through the process of creating and configuring an API Gateway for your serverless applications on AWS. Follow these steps to set up a scalable API that integrates seamlessly with your serverless architecture.

#### Step-by-Step Guide to Creating API Gateway
To create an API Gateway in AWS, follow these steps:

##### 1. Go to https://console.aws.amazon.com/ and sign in to your AWS account.
##### 2. Go to https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1
##### 3. Click to Create API, choose REST API click BuildChoose New API and fill yours API name, API endpoint type choose Regional and Create API

In Amazon API Gateway, there are three types of API endpoints that you can choose from when creating a REST API: Regional, Edge-optimized, and Private.

+ Regional Endpoint:A regional endpoint is deployed in a specific AWS Region and is accessible only from within that Region.It’s suitable for applications where all API traffic originates from within the same AWS Region as the API Gateway deployment.Regional endpoints provide low latency and are ideal for backend services that are also deployed within the same AWS Region.

+ Edge-Optimized Endpoint:An edge-optimized endpoint is deployed to AWS CloudFront edge locations, which are distributed globally.Requests to the API are routed to the nearest CloudFront edge location, reducing latency for clients located worldwide.This endpoint type leverages the AWS global network infrastructure provided by CloudFront, making it ideal for globally distributed applications.It’s the default endpoint type when creating an API in API Gateway.

+ Private Endpoint:A private endpoint is accessible only from within your Amazon Virtual Private Cloud (VPC) using an interface VPC endpoint.It’s suitable for scenarios where you want to expose your API to resources within your VPC or other connected networks via AWS Direct Connect or AWS VPN.Private endpoints provide enhanced security by keeping traffic within the private network, reducing exposure to the public internet.
##### 4. Create resource to create main Path for APIs.
![Create resource to create main Path for APIs](/images/2/CreateAPIGW1.jpeg?featherlight=false&width=80pc)
Next, config yours path like below and remember CORS (Cross Origin Resource Sharing)
Cross-Origin Resource Sharing (CORS) is a security feature implemented by web browsers to prevent scripts running on one domain from making requests to another domain. When you’re using Amazon API Gateway to create a REST API, you might need to configure CORS to allow your API to be accessed by client applications running in web browsers from different origins.
After created resources, now we have to create some method mapping with lambda function, click Create Method
##### 5. After created resources, now we have to create some method mapping with lambda function, click Create Method
![Create Method for APIs](/images/2/CreateAPIGW2.jpeg?featherlight=false&width=80pc)
At here, we choose
Method type: GET/POST/DELETE
Integration type: Lambda Function
Lambda function: choose your lambda function create at step previous
Click to Create method , after created we have method like image below.
![Create Method for APIs](/images/2/CreateAPIGW3.jpeg?featherlight=false&width=80pc)
##### 6. Click to tab Test to test API, click Test to test APIs
![test APIs](/images/2/CreateAPIGW4.jpeg?featherlight=false&width=80pc)
![test APIs](/images/2/CreateAPIGW5.jpeg?featherlight=false&width=80pc)
We have received responses like the image above. Next create similar method with method **POST, DELETE** integration with **LambdaDeleteDataFunction, LambdaAddDataFunction**
##### 7. After done, we have to click Deploy API to expose API over internet
![test APIs](/images/2/Deploy5.jpeg?featherlight=false&width=50pc)
**Click Deploy API** 
![test APIs](/images/2/Deploy1.jpeg?featherlight=false&width=50pc)
**Fill your Stage name and Deployment description, after click Deploy we have received response below**
![test APIs](/images/2/Deploy3.jpeg?featherlight=false&width=50pc)

- Let’s inspect Invoke URL: https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/prod this is public URL we can call on the public internet.
After get URL public API, you can test with POSTMAN, TERMINAL tool
You can try with Curl Request with pattern: curl -X GET {{YourInvoke URL}}/{{Stage}}/{{YourAPIs}}

Example: 

    curl -X GET https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/dev/notes

**You also test with POSTMAN, TERMINAL tool**