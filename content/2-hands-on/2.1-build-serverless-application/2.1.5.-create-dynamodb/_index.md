---
title : "Create DynamoDB"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 2.1.5 </b> "
---


#### Create DynamoDB
![Create DynamoDB](/images/2/CreateLambda10.jpeg?featherlight=false&width=50pc)
![Create DynamoDB](/images/2/CreateLambda11.jpeg?featherlight=false&width=50pc)
![Create DynamoDB](/images/2/CreateLambda12.jpeg?featherlight=false&width=50pc)

Now, we almost completed set up lambda function, next step we need datasource to lambda function can active and work correct follow.

+ Sign in to the AWS Management Console: Go to https://console.aws.amazon.com/ and sign in to your AWS account.
+ Open DynamoDB Console: Once you’re logged in, navigate to the DynamoDB service by either typing “DynamoDB” in the search bar or by locating it under the “Database” section in the services menu.
+ Create a Table: **note_tbl** (This table we have configured at environment variable)
+ Click on the “Create table” button.
+ You will be prompted to enter details for your new table:
Table name:
Primary key: Define a primary key for your table. This consists of the partition key and optionally a sort key. DynamoDB uses the primary key to uniquely identify each item in the table.
Provisioned capacity vs. On-demand capacity: Choose whether you want to provision capacity manually or use on-demand capacity.
If you choose provisioned capacity, you’ll need to specify the read and write capacity units.
If you choose on-demand capacity, DynamoDB will automatically adjust the read and write capacity based on your workload.
Once you’ve entered all the necessary details, click “Create”.

After created tables, we have successfully setting dynamodb, next step we need create REST API using API Gateway to execute lambda function and interact with Dynamo DB