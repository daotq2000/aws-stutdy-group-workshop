---
title : "Create Lambda function and deploy"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.1.2 </b> "
---


#### Create Lambda Function 

![Create Lambda function and deploy](/images/2/CreateLambda1.jpeg?featherlight=false&width=80pc)

Here are the steps to create a Lambda function using the AWS Management Console:

+ Go to https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions and choose **Create function**

![Create Lambda function and deploy 2](/images/2/CreateLambda2.jpeg?featherlight=false&width=80pc)

Fill yours function infomation: 

+ Select: Author from scratch
+ Function name: fill your function name. ex: LambdaFetchDataFunction
+ Runtime: Java11
+ Architecture: arm64
+ Advanced settings: Select Enable VPC
+ VPC: Choose VPC we are created at step 1
+ Subnets: Choose private-subnet
+ Security groups: recommend default

After fulfill choose **Create function**, Similar let continue create 3 functions like image below order: **LambdaFetchDataFunction**, **LambdaDeleteDataFunction**, **LambdaFetchDataFunction**

![Create Lambda function and deploy 3](/images/2/CreateLambda3.jpeg?featherlight=false&width=80pc)

#### Config Detail For Each Function

![Create Lambda function and deploy 3](/images/2/LamDaDetail1.png?featherlight=false&width=80pc)

After do that, we have 3 function, now click to LambdaFetchDataFunction
![Create Lambda function and deploy 3](/images/2/LamDaDetail2.png?featherlight=false&width=80pc)
To function working, we need upload code to lambda, click Upload From at Code tab..
![Create Lambda function and deploy 3](/images/2/CreateLambda4.jpeg?featherlight=false&width=80pc)

We have 2 choices:

+ Upload the file directly .jar file, that we have built mvn clean package at step 3
+ Upload via S3 console and get a public URI and fill that

![Create Lambda function and deploy 3](/images/2/CreateLambda5.jpeg?featherlight=false&width=80pc)

Next, we have to config runtime environment to lambda can execute correct function, Click **Edit** 
![Create Lambda function and deploy 3](/images/2/CreateLambda6.jpeg?featherlight=false&width=80pc)

Patse Handle is: **org.example.awsserverlessnoteapp.LambdaFetchDataFunction::handleRequest**

![Create Lambda function and deploy 3](/images/2/CreateLambda7.jpeg?featherlight=false&width=80pc)
Now, let’s inspect our source code. We have a class name LambdaFetchDataFunction, The class LambdaFetchDataFunction under **package: org.example.awsserverlessnoteapp**. The class LambdaFetchDataFunction has function handleRequest, that execute fetch data from DynamoDB
![Create Lambda function and deploy 3](/images/2/CreateLambda8.jpeg?featherlight=false&width=80pc)

Next we will click on the Configuration tab, click to Environment variables, we can configure multiple values here.

+ General configuration
+ Triggers
+ Permissions
+ Destinations
+ Function URL
+ **Environment variables**
+ Tags
+ VPC
+ Monitoring and operations tools
+ Concurrency
+ Asynchronous invocation
+ Code signing
+ RDS databases
+ File systems
+ State machines

Let’s click Edit and add 2 environment variables to it.

    Key: AWS_DYNAMO_TABLE_NAME_VALUE, Value: note_tbl
    Key: AWS_REGION_VALUE, Value: us-east-1

Similarly, set up the lambda function LambdaAddDataFunction,LambdaDeleteDataFunction according to the instructions above.

