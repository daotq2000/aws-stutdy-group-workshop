---
title : "CI/CD Integration with CodeBuild: Auto-Deploy Lambda Function"
description : "Discover how to integrate CI/CD with AWS CodeBuild for auto-deploying Lambda functions. This guide covers setup, configuration, and best practices for a seamless deployment process."
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 2.1.9 </b> "
image: "images/2/111111111111111111111.png"
---


### Create CodeBuild Project

Next step, we need to create a CodeBuild Project to integrate with github webhook to auto deploy lambda functions when a new commit is detected.
1. Go to https://us-east-1.console.aws.amazon.com/codesuite/codebuild/start?region=us-east-1
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/BE3.png?featherlight=false&width=80pc)
2. Click **Create project** 
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/BE1.png?featherlight=false&width=80pc)
3. Fill your Project configuration, Source
4. Choose source: S3/GitHub, at here we choose GitHub
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/BE2.png?featherlight=false&width=80pc)
5. At Environment, we select **lambda** to build image
6. Choose Operating system: **Amazon Lunix**
7. Choose Runtime(s): **Java 11**
8. Choose Image: **aws/codebuild/amazonlinux-aarch64-lambda-standard:corretto11**
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Build1.png?featherlight=false&width=50pc)
9. On Buildspec we select **Insert build commands** and choose **Switch to editor**
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Build3.png?featherlight=false&width=50pc)
10. It see like this, now we need config pipeline using code below to build successfully project

        version: 0.2
        phases:
        install:
            runtime-versions:
            java: corretto11
            maven: 3.9.6
            # commands:
            #   - export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
            #   - export PATH=$JAVA_HOME/bin:$PATH
        build:
            commands:
            - ls
            - cd note-app-serverless/aws-serverless-note-app
            - ls -lrt
            - mvn clean package -DskipTests=true
            - ls
            # Upload JAR file to S3
            - aws s3 cp target/aws-serverless-note-app-0.0.1-SNAPSHOT.jar s3://cf-templates-1ohbdpx0bstp8-us-east-1/aws-serverless-note-app-0.0.1-SNAPSHOT.jar
            - aws lambda update-function-code --function-name LambdaAddDataFunction --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
            - aws lambda update-function-code --function-name LambdaDeleteNote --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
            - aws lambda update-function-code --function-name LambdaFetchDataFunction --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
            - echo 'Successfully Build new version'



aws s3 cp target/aws-serverless-note-app-0.0.1-SNAPSHOT.jar s3://cf-templates-1ohbdpx0bstp8-us-east-1/aws-serverless-note-app-0.0.1-SNAPSHOT.jar

 **Explain: This command have meaning is coppy .jar file to S3 bucket**

+ **target/aws-serverless-note-app-0.0.1-SNAPSHOT.jar** is output file, after run **mvn clean package -DskipTests=true**
+ **s3://cf-templates-1ohbdpx0bstp8-us-east-1/aws-serverless-note-app-0.0.1-SNAPSHOT.jar** is s3 bucket url, you save file on s3

aws lambda update-function-code --function-name LambdaDeleteNote --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar

 **Explain: This command have meaning is update source for lambda funtion LambdaDeleteNote from jar file, you have puted in s3**

### Create Pipeline
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline1.png?featherlight=false&width=50pc)
1. Go to https://us-east-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=us-east-1
2. Click Create pipeline

We have complete 4 steps:
+ Step 1 :Choose pipeline settings
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline1.jpeg?featherlight=false&width=50pc)
+ Step 2: Add source 
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline2.jpeg?featherlight=false&width=50pc)
+ Step 3: Add build 
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline3.jpeg?featherlight=false&width=50pc)
+ Step 4:Add deploy 
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline4.jpeg?featherlight=false&width=50pc)
+ Step 5: Review and Create

**Now, almost set up done, we will test by publish new commit change and pipeline auto execution**

### Test
Now, inspect source code and make any change to test pipeline work correctly.

        luongtx@daotq1:~/Workspace/GITHUB/aws-handson$ git status
        On branch main
        Your branch is up to date with 'origin/main'.

        Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git restore <file>..." to discard changes in working directory)
                modified:   note-app-serverless/aws-resource/code-deploy/pipeline.yaml

        no changes added to commit (use "git add" and/or "git commit -a")
        luongtx@daotq1:~/Workspace/GITHUB/aws-handson$ git add .
        luongtx@daotq1:~/Workspace/GITHUB/aws-handson$ git commit -m "test: pipeline auto build"
        [main ac6d839] test: pipeline auto build
        1 file changed, 1 insertion(+), 1 deletion(-)
        luongtx@daotq1:~/Workspace/GITHUB/aws-handson$ git push 
        Enumerating objects: 11, done.
        Counting objects: 100% (11/11), done.
        Delta compression using up to 12 threads
        Compressing objects: 100% (5/5), done.
        Writing objects: 100% (6/6), 553 bytes | 184.00 KiB/s, done.
        Total 6 (delta 2), reused 0 (delta 0), pack-reused 0
        remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
        To github.com:daotq2000/aws-handson.git
        2bfe657..ac6d839  main -> main

![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline5.jpeg?featherlight=false&width=100pc)
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline6.jpeg?featherlight=false&width=100pc)
![CICD Integration with CodeBuild Auto Deploy Lambda Function](/images/2/Pipeline7.jpeg?featherlight=false&width=100pc)

Open your CodeBuild Project and Click to detail and view the build console.

If you received response below, pipeline already build completed and lambda function have been update.

    [Container] 2024/03/05 04:33:08.400929 Running command aws lambda update-function-code --function-name LambdaAddDataFunction --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
    {
        "FunctionName": "LambdaAddDataFunction",
        "FunctionArn": "arn:aws:lambda:us-east-1:846338211683:function:LambdaAddDataFunction",
        "Runtime": "java11",
        "Role": "arn:aws:iam::846338211683:role/LambdaFunctionRole",
        "Handler": "org.example.awsserverlessnoteapp.LambdaAddDataFunction::handleRequest",
        "CodeSize": 18081570,
        "Description": "",
        "Timeout": 15,
        "MemorySize": 512,
        "LastModified": "2024-03-05T04:33:10.000+0000",
        "CodeSha256": "54cOLGA6bBmBfrbZkb6trotsuPRfB1fAhwGikbmJyxs=",
        "Version": "$LATEST",
        "VpcConfig": {
            "SubnetIds": [
                "subnet-0b31adde22bcc2d4e"
            ],
            "SecurityGroupIds": [
                "sg-069f0598b55c8a6b8"
            ],
            "VpcId": "vpc-0bdac14c978619a96",
            "Ipv6AllowedForDualStack": false
        },
        "Environment": {
            "Variables": {
                "AWS_REGION_VALUE": "us-east-1",
                "AWS_DYNAMO_TABLE_NAME_VALUE": "note_tbl"
            }
        },
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "2a239e66-937d-405e-974c-549767398c1f",
        "State": "Active",
        "LastUpdateStatus": "InProgress",
        "LastUpdateStatusReason": "The function is being created.",
        "LastUpdateStatusReasonCode": "Creating",
        "PackageType": "Zip",
        "Architectures": [
            "x86_64"
        ],
        "EphemeralStorage": {
            "Size": 512
        },
        "SnapStart": {
            "ApplyOn": "None",
            "OptimizationStatus": "Off"
        },
        "RuntimeVersionConfig": {
            "RuntimeVersionArn": "arn:aws:lambda:us-east-1::runtime:5376f7691b03f8ade18032dc3906c8e0e7a4101c665b17c8766b6949687df24f"
        }
    }

    [Container] 2024/03/05 04:33:10.259486 Running command aws lambda update-function-code --function-name LambdaDeleteNote --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
    {
        "FunctionName": "LambdaDeleteNote",
        "FunctionArn": "arn:aws:lambda:us-east-1:846338211683:function:LambdaDeleteNote",
        "Runtime": "java11",
        "Role": "arn:aws:iam::846338211683:role/service-role/LambdaDeleteNote-role-fa4amiiz",
        "Handler": "org.example.awsserverlessnoteapp.LambdaDeleteDataFunction::handleRequest",
        "CodeSize": 18081570,
        "Description": "",
        "Timeout": 15,
        "MemorySize": 512,
        "LastModified": "2024-03-05T04:33:11.000+0000",
        "CodeSha256": "54cOLGA6bBmBfrbZkb6trotsuPRfB1fAhwGikbmJyxs=",
        "Version": "$LATEST",
        "VpcConfig": {
            "SubnetIds": [
                "subnet-0b31adde22bcc2d4e"
            ],
            "SecurityGroupIds": [
                "sg-069f0598b55c8a6b8"
            ],
            "VpcId": "vpc-0bdac14c978619a96",
            "Ipv6AllowedForDualStack": false
        },
        "Environment": {
            "Variables": {
                "AWS_REGION_VALUE": "us-east-1",
                "AWS_DYNAMO_TABLE_NAME_VALUE": "note_tbl"
            }
        },
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "d1559a95-7b4d-4c7c-a8d3-3b95ead46cae",
        "State": "Active",
        "LastUpdateStatus": "InProgress",
        "LastUpdateStatusReason": "The function is being created.",
        "LastUpdateStatusReasonCode": "Creating",
        "PackageType": "Zip",
        "Architectures": [
            "arm64"
        ],
        "EphemeralStorage": {
            "Size": 512
        },
        "SnapStart": {
            "ApplyOn": "None",
            "OptimizationStatus": "Off"
        },
        "RuntimeVersionConfig": {
            "RuntimeVersionArn": "arn:aws:lambda:us-east-1::runtime:4953353514423cc08453f25c28e74edd762626ff6bf2abe0c78c5a0e5be8547a"
        }
    }

    [Container] 2024/03/05 04:33:11.791342 Running command aws lambda update-function-code --function-name LambdaFetchDataFunction --s3-bucket cf-templates-1ohbdpx0bstp8-us-east-1 --s3-key aws-serverless-note-app-0.0.1-SNAPSHOT.jar
    {
        "FunctionName": "LambdaFetchDataFunction",
        "FunctionArn": "arn:aws:lambda:us-east-1:846338211683:function:LambdaFetchDataFunction",
        "Runtime": "java11",
        "Role": "arn:aws:iam::846338211683:role/LambdaFunctionRole",
        "Handler": "org.example.awsserverlessnoteapp.LambdaFetchDataFunction::handleRequest",
        "CodeSize": 18081570,
        "Description": "",
        "Timeout": 15,
        "MemorySize": 512,
        "LastModified": "2024-03-05T04:33:13.000+0000",
        "CodeSha256": "54cOLGA6bBmBfrbZkb6trotsuPRfB1fAhwGikbmJyxs=",
        "Version": "$LATEST",
        "VpcConfig": {
            "SubnetIds": [
                "subnet-0b31adde22bcc2d4e"
            ],
            "SecurityGroupIds": [
                "sg-069f0598b55c8a6b8"
            ],
            "VpcId": "vpc-0bdac14c978619a96",
            "Ipv6AllowedForDualStack": false
        },
        "Environment": {
            "Variables": {
                "AWS_REGION_VALUE": "us-east-1",
                "AWS_DYNAMO_TABLE_NAME_VALUE": "note_tbl"
            }
        },
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "9254245f-4d87-42ef-a500-751bee1325b0",
        "State": "Active",
        "LastUpdateStatus": "InProgress",
        "LastUpdateStatusReason": "The function is being created.",
        "LastUpdateStatusReasonCode": "Creating",
        "PackageType": "Zip",
        "Architectures": [
            "arm64"
        ],
        "EphemeralStorage": {
            "Size": 512
        },
        "SnapStart": {
            "ApplyOn": "None",
            "OptimizationStatus": "Off"
        },
        "RuntimeVersionConfig": {
            "RuntimeVersionArn": "arn:aws:lambda:us-east-1::runtime:4953353514423cc08453f25c28e74edd762626ff6bf2abe0c78c5a0e5be8547a"
        }
    }

