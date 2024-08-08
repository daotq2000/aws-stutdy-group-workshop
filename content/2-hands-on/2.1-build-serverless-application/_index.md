---
title : "Build Note Application AWS Serverless"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1 </b> "
image: "/images/1/LambdaArchitechture.svg" # The path to your image
---


### Challenge 1: Building Serverless Appication with AWS Lambda
![Build-Serverless-Application](/images/1/LambdaArchitechture.svg?featherlight=false&width=100pc)

The Take Note App Project is a serverless application designed to provide users with a convenient platform for taking and managing notes. Built using AWS services, the project leverages Lambda functions for backend logic and DynamoDB for data storage. Additionally, it integrates with GitHub for automatic deployment using AWS CodeBuild.

**Key components and features of the Take Note App Project include:**

+ **Serverless Architecture:** The project follows a serverless architecture, eliminating the need for managing servers and infrastructure. AWS Lambda functions are used to execute backend logic in response to events triggered by user actions.
Lambda Functions: Lambda functions serve as the backbone of the application, handling various functionalities such as creating, retrieving, updating, and deleting notes. These functions are designed to scale automatically based on demand, ensuring high availability and optimal performance.
+ **DynamoDB Integration:** DynamoDB is utilized as the NoSQL database for storing and managing notes data. The integration between Lambda functions and DynamoDB enables seamless interaction with the database, allowing users to store and retrieve their notes efficiently.
+ **GitHub Integration:** The project utilizes AWS CodeBuild for continuous integration and deployment (CI/CD) by integrating with GitHub repositories. Whenever changes are pushed to the GitHub repository, CodeBuild automatically triggers the build process, ensuring that the latest version of the application is deployed to the AWS environment.
User Authentication and Authorization: The application can incorporate user authentication and authorization mechanisms to ensure secure access to notes data. This can be achieved using AWS Cognito for user management and authentication, allowing users to sign in securely and access their notes with proper permissions.
+ **Frontend Interface:** While not explicitly mentioned, the project likely includes a frontend interface, such as a web application or mobile app, through which users interact with the Take Note App. This interface communicates with the backend Lambda functions to perform CRUD operations on notes stored in DynamoDB.

**Overall**, the Take Note App Project demonstrates the power and flexibility of serverless architecture in building scalable and cost-effective applications. By leveraging AWS services such as Lambda, DynamoDB, CodeBuild, and potentially others, the project provides a robust platform for users to manage their notes seamlessly while automating the deployment process for streamlined development workflows.

#### Content

[2.1.1 Create VPC, Subnet, Route Table](2.1.1.-create-vpc-subnet-route-table/)

[2.1.2. Create VPC endpoint](2.1.2.-create-vpc-endpoint/)

[2.1.3. Develop Lambda Function with Java Language](2.1.3.-develop-lambda-function-with-java-language/)

[2.1.4. Create Lambda function and deploy](2.1.4.-create-lambda-function-and-deploy/)

[2.1.5. Create DynamoDB](2.1.5.-create-dynamodb/)

[2.1.6. Create API Gateway](2.1.6.-create-api-gateway/)

[2.1.7. Development Frontend](2.1.7.-development-frontend/)

[2.1.8 CICD Auto Deploy Static Page With GitHub](2.1.8-cicd-auto-deploy-static-page-with-github/)

[2.1.9. CICD Integration with CodeBuild Auto Deploy Lambda Function](2.1.9.-cicd-integration-with-codebuild-auto-deploy-lambda-function/)
