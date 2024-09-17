---
title : "Introduction to AWS Compute Services: EC2, Lambda, and More"
description : "Learn about AWS compute services, including EC2 and Lambda. Discover how these services can optimize your cloud computing needs and enhance scalability"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 1.2 </b> "
image: "images/1/aws-compute-services.png" # The path to your image
---

## Introduction AWS Compute Services
Compute services offered by AWS provide scalable computing power for various workloads, enabling businesses to deploy and manage applications efficiently in the cloud. Here's an introduction to some of the key compute services provided by AWS:

## Content
### 1. Amazon EC2 (Elastic Compute Cloud):

Amazon EC2 is a web service that provides resizable compute capacity in the cloud. It allows users to quickly scale up or down based on demand, paying only for the resources they use.
Users can choose from a variety of instance types optimized for different workloads, such as general-purpose, compute-optimized, memory-optimized, and storage-optimized instances.
EC2 instances can be used for a wide range of applications including web hosting, application development, data processing, and machine learning.

![EC2](/images/1/ec2.png?featherlight=false&width=50pc)
**EC2 General-Purpose Instance Types** \
Here are several general-purpose examples from which we can pick:

    T2. micro: The most well-known instance in AWS is t2. micro, which gives 1 CPU and 1 GB of memory with low to moderate network performance. It is also free and highly helpful for individuals first starting AWS.

    M6a Instance: The third-generation AMD EPYC processors used in the M6 instance are perfect for general-purpose tasks. In m6a there are different sizes like m6a.large, m6a.2xlarge, m6a.4xlarge, and so on. m6a large offers 2 CPUs, 8GiB memory, and network performance up to 12.5 Gigabit.

    M5 instance: The newest generation of general-purpose instances, known as M5, are powered by Intel’s Xeon Platinum 8175 processors. Its M5 divisions include m5. large, m5.12xlarge, and m5.24 large, and the sort of M5 service we select will depend on memory, CPUs, storage, and network speed.

#### Use in which situation?
**Applications Web Servers**: The web servers can be hosted in General-purpose instances.EC2 instances provide a flexible and scalable platform for web applications.
Development and Test Environment: The developers can use these General-purpose instances to build, test and deploy the applications. It is a cost-effective solution for running this environment.

**Content delivery**: The hosting of content delivery networks (CDNs) that distribute content to users all over the world is possible using general-purpose instances. EC2 instances can be set up to provide content with low latency and great performance.
A popular option for many businesses, AWS EC2 general-purpose instances offer a versatile and scalable platform for a variety ###of applications.

### 2. AWS Lambda:
AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS) that enables users to run code without provisioning or managing servers. It allows developers to focus on writing code for their applications without worrying about the underlying infrastructure. Here's a detailed overview of AWS Lambda:
![Lambda](/images/1/lambda.png?featherlight=false&width=50pc)
#### Serverless Computing:

With Lambda, users can upload their code and AWS takes care of provisioning, scaling, and managing the infrastructure needed to run that code.
Users are charged only for the compute time consumed by their code, measured in milliseconds, making Lambda a cost-effective solution for various workloads.
#### Event-Driven Architecture:

Lambda functions are invoked in response to events such as HTTP requests, changes to data in Amazon S3 buckets, updates to Amazon DynamoDB tables, messages from Amazon SNS (Simple Notification Service), and many others.
This event-driven architecture allows developers to build reactive and scalable applications that respond to changes or triggers in real-time.

#### Supported Runtimes and Languages:

Lambda supports multiple programming languages including Node.js, Python, Java, Go, .NET Core, and Ruby, allowing developers to use their preferred language for writing Lambda functions.
Each Lambda function runs in an isolated environment called a "runtime", which provides the necessary dependencies and libraries for the chosen language.

#### Automatic Scaling:

Lambda automatically scales the execution environment to handle incoming requests or events. It can handle a few requests per day to thousands of requests per second without any manual intervention. Scaling is handled by AWS based on the number of incoming events and the configured concurrency settings.
#### Stateless Execution:
Lambda functions are stateless, meaning they do not maintain any persistent state between invocations. Each invocation of a Lambda function is independent of previous invocations.If state persistence is required, developers can use external storage services like Amazon DynamoDB or Amazon S3 to store and retrieve state information.
#### Integration with AWS Services:

Lambda integrates seamlessly with other AWS services, allowing developers to build complex workflows and applications. For example, Lambda functions can be used to process data from Amazon S3, trigger notifications via Amazon SNS, or interact with Amazon RDS databases.

AWS provides built-in integration with various services through AWS SDKs and event sources, making it easy to connect Lambda functions with other AWS resources.
**Security and Access Control:**

Lambda functions can be secured using AWS IAM (Identity and Access Management) policies, which control who can invoke the function and what resources it can access.
Developers can also use AWS Key Management Service (KMS) to encrypt sensitive data within Lambda functions or use environment variables to store configuration settings securely.
#### Use lamdba in which situation?
**Microservices Architecture:**

Lambda functions can be used to implement microservices in a serverless architecture, where each function represents a specific component or functionality of the application.
By breaking down your application into smaller, independent functions, you can achieve better scalability, flexibility, and maintainability, while reducing the operational overhead of managing infrastructure.

**Real-time Data Processing:**

Lambda is well-suited for real-time data processing tasks such as stream processing, data enrichment, and analytics. It can be used to process streaming data from services like Amazon Kinesis and Amazon Managed Streaming for Apache Kafka (Amazon MSK). By processing data in real-time with Lambda, you can derive insights, trigger actions, and make decisions based on up-to-date information, enabling reactive and data-driven applications.

**Scheduled Tasks and Cron Jobs:**

Lambda functions can be scheduled to run at specified intervals using CloudWatch Events or EventBridge (formerly CloudWatch Events). This makes Lambda a convenient solution for running scheduled tasks, cron jobs, and batch processing jobs without the need for dedicated servers or infrastructure.
You can use Lambda to perform periodic data backups, generate reports, clean up resources, or execute any other recurring tasks.

**Web Application Backends:**

Lambda can serve as the backend for web applications, handling HTTP requests and executing application logic in response. When combined with API Gateway, Lambda enables developers to build serverless RESTful APIs quickly and easily.
Lambda functions can handle user authentication, data validation, business logic, and database interactions, providing a scalable and cost-effective solution for web application development.
### 3. Amazon ECS (Elastic Container Service):
Amazon ECS is a fully managed container orchestration service that allows users to run, stop, and manage Docker containers on a cluster of EC2 instances.
Users can easily deploy, manage, and scale containerized applications using ECS. It integrates with other AWS services like Elastic Load Balancing, IAM, and CloudWatch for enhanced functionality.
ECS enables users to build microservices architectures and modernize their applications by leveraging containers.

### 4. Amazon EKS (Elastic Kubernetes Service):
Amazon EKS is a fully managed Kubernetes service that makes it easy to deploy, manage, and scale containerized applications using Kubernetes on AWS.
It provides a highly available and secure Kubernetes control plane without the overhead of managing the infrastructure.
With Amazon EKS, users can run Kubernetes applications with the same tools and APIs they use on-premises, while benefiting from the scalability and reliability of AWS.Real-time Data Processing:.

