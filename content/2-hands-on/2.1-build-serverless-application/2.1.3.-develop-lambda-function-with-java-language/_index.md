---
title : "How to Create and Deploy a Lambda Function in AWS: A Step-by-Step Guide"
description : "Learn how to create and deploy a Lambda function in AWS with this step-by-step guide. Understand the process and best practices for serverless application deployment."
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.1.3 </b> "
image: "images/1/java&lambda.png"
---


#### Project Structure
        ├── note-app-serverless
        │   ├── aws-resource
        │   │   ├── api-gw
        │   │   │   ├── api-config.json
        │   │   │   ├── api-output.json
        │   │   │   ├── api-resouce-out-put.json
        │   │   │   ├── instructions.md
        │   │   │   ├── Note Resources APP API-dev-oas30 (1).json
        │   │   │   └── Note Resources APP API-dev-oas30-postman (1).json
        │   │   ├── code-deploy
        │   │   │   └── pipeline.yaml
        │   │   └── lambda
        │   │       ├── LambdaAddDataFunction.yaml
        │   │       ├── LambdaDeleteNote.yaml
        │   │       └── LambdaFetchDataFunction.yaml
        │   └── aws-serverless-note-app
        │       ├── mvnw
        │       ├── mvnw.cmd
        │       ├── pom.xml
        │       ├── res.txt
        │       ├── src
        │       │   ├── main
        │       │   │   ├── java
        │       │   │   │   └── org
        │       │   │   │       └── example
        │       │   │   │           └── awsserverlessnoteapp
        │       │   │   │               ├── ApplicationMain.java
        │       │   │   │               ├── enviromentVariable.sh
        │       │   │   │               ├── LambdaAddDataFunction.java
        │       │   │   │               ├── LambdaDeleteDataFunction.java
        │       │   │   │               ├── LambdaFetchDataFunction.java
        │       │   │   │               └── LambdaUpdateDataFunction.java
        │       │   │   └── resources
        │       │   │       └── application.properties
        │       │   └── test
        │       │       └── java
        │       │           └── org
        │       │               └── example
        │       │                   └── awsserverlessnoteapp
        │       │                       └── AwsServerlessNoteAppApplicationTests.java
        │       └── target
        │           ├── aws-serverless-note-app-0.0.1-SNAPSHOT.jar
        │           ├── classes
        │           │   ├── application.properties
        │           │   └── org
        │           │       └── example
        │           │           └── awsserverlessnoteapp
        │           │               ├── ApplicationMain.class
        │           │               ├── LambdaAddDataFunction.class
        │           │               ├── LambdaDeleteDataFunction.class
        │           │               ├── LambdaFetchDataFunction.class
        │           │               └── LambdaUpdateDataFunction.class
        │           ├── generated-sources
        │           │   └── annotations
        │           ├── generated-test-sources
        │           │   └── test-annotations
        │           ├── maven-archiver
        │           │   └── pom.properties
        │           ├── maven-status
        │           │   └── maven-compiler-plugin
        │           │       ├── compile
        │           │       │   └── default-compile
        │           │       │       ├── createdFiles.lst
        │           │       │       └── inputFiles.lst
        │           │       └── testCompile
        │           │           └── default-testCompile
        │           │               ├── createdFiles.lst
        │           │               └── inputFiles.lst
        │           ├── original-aws-serverless-note-app-0.0.1-SNAPSHOT.jar
        │           └── test-classes
        └── README.md
Refer My Gihub Repository and Let’s inspect the source code to understand how to create Lambda functions with Java.

**Git Url: https://github.com/daotq2000/aws-handson**


Here’s a breakdown of the relevant directories and files:

**note-app-serverless:**

+ This appears to be the root directory of your project.Inside this directory, there are two main subdirectories: aws-resource and aws-serverless-note-app.

**aws-resource:**

+ This directory contains resources related to your AWS infrastructure setup. Inside aws-resource, there are subdirectories like api-gw, code-deploy, and lambda.api-gw likely contains configurations and definitions related to API Gateway.code-deploy appears to contain a pipeline definition file for AWS CodeDeploy.lambda likely contains configurations or definitions for your Lambda functions.

**aws-serverless-note-app:**

+ This directory seems to contain the source code for your serverless note-taking app.Inside aws-serverless-note-app, there are directories like src and target, typical of a Maven project structure.The src directory contains your Java source code organized into main and test directories.

The main directory contains the application’s main Java code, including classes like ApplicationMain.java, LambdaAddDataFunction.java, LambdaDeleteDataFunction.java, etc. The resources directory likely contains application configuration files, such as application.properties.


#### Hands-on

Let start with **pom.xml**
We needs 2 dependencies to develop core for lambda with Java Spring Boot Project **Pom.xml**

    <dependency>
           <groupId>com.amazonaws</groupId>
           <artifactId>aws-java-sdk-dynamodb</artifactId>
           <version>1.12.670</version>
    </dependency>
       <!-- https://mvnrepository.com/artifact/com.amazonaws/aws-lambda-java-core -->
    <dependency>
           <groupId>com.amazonaws</groupId>
           <artifactId>aws-lambda-java-core</artifactId>
           <version>1.2.3</version>
    </dependency>
Next, we have to add plugin compile lambda function to **Pom.xml** file

    <plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.5.2</version>
        <configuration>
            <createDependencyReducedPom>false</createDependencyReducedPom>
        </configuration>
        <executions>
            <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            </execution>
        </executions>
        </plugin>
    </plugins>

Here is full version file  **https://github.com/daotq2000/aws-handson/blob/main/note-app-serverless/aws-serverless-note-app/pom.xml**

**Note:** We need create 3 function: Add,Update Data and Fetch data and delete data. We must writing code under forder: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp

##### First, let create Function **LambdaAddDataFunction** on file **LambdaAddDataFunction.java**

Path: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp/LambdaAddDataFunction.java

    package org.example.awsserverlessnoteapp;


    import com.amazonaws.services.dynamodbv2.AmazonDynamoDBClient;
    import com.amazonaws.services.dynamodbv2.model.AttributeValue;
    import com.amazonaws.services.dynamodbv2.model.PutItemRequest;
    import com.amazonaws.services.lambda.runtime.Context;
    import com.amazonaws.services.lambda.runtime.RequestHandler;


    import java.util.HashMap;
    import java.util.Map;


    public class LambdaAddDataFunction implements RequestHandler<Map<String,String>,String> {
    private static final String TABLE_NAME = System.getenv("AWS_DYNAMO_TABLE_NAME_VALUE");
    private AmazonDynamoDBClient ddb;


    public LambdaAddDataFunction() {
        ddb = (AmazonDynamoDBClient) AmazonDynamoDBClient.builder().withRegion(System.getenv("AWS_REGION_VALUE")).build();
    }
    @Override
    public String handleRequest(Map<String, String> itemValues, Context context) {
        Map<String, AttributeValue> item = new HashMap<>();
        itemValues.forEach((k, v) -> item.put(k, new AttributeValue().withS(v.toString())));
        PutItemRequest request = new PutItemRequest();
        request.setTableName(TABLE_NAME);
        request.setItem(item);
        ddb.putItem(request);
        return item.toString();
    }
    }

**Explain:** Because of Information of DynamoDB Table, Regions can change on Run Time, so we need expose it to environment by using **System.getenv(“AWS_REGION_VALUE”)** and **System.getenv(“AWS_DYNAMO_TABLE_NAME_VALUE”)** to get environment variables at runtime. 

    private static final String TABLE_NAME = System.getenv("AWS_DYNAMO_TABLE_NAME_VALUE");
    public LambdaAddDataFunction() {
        ddb = (AmazonDynamoDBClient) AmazonDynamoDBClient.builder().withRegion(System.getenv("AWS_REGION_VALUE")).build();
    }

##### Second, let create Function **LambdaDeleteDataFunction** on file **LambdaDeleteDataFunction.java**

Path: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp/LambdaDeleteDataFunction.java

    package org.example.awsserverlessnoteapp;


    import com.amazonaws.services.dynamodbv2.AmazonDynamoDBClient;
    import com.amazonaws.services.dynamodbv2.model.AttributeValue;
    import com.amazonaws.services.dynamodbv2.model.DeleteItemRequest;
    import com.amazonaws.services.lambda.runtime.Context;
    import com.amazonaws.services.lambda.runtime.RequestHandler;


    import java.util.HashMap;
    import java.util.Map;


    public class LambdaDeleteDataFunction implements RequestHandler<Map<String,String>,String> {
    private static final String TABLE_NAME = System.getenv("AWS_DYNAMO_TABLE_NAME_VALUE");
    private AmazonDynamoDBClient ddb;


    public LambdaDeleteDataFunction() {
        ddb = (AmazonDynamoDBClient) AmazonDynamoDBClient.builder().withRegion(System.getenv("AWS_REGION_VALUE")).build();
    }
    @Override
    public String handleRequest(Map<String, String> itemValues, Context context) {
        Map<String, AttributeValue> key = new HashMap<>();
        key.put("id", new AttributeValue().withS(itemValues.get("id")));
        DeleteItemRequest request = new DeleteItemRequest()
                .withTableName(TABLE_NAME)
                .withKey(key);
        ddb.deleteItem(request);
        return "Delete successfully";
    }
    }
##### Third, let create Function **LambdaFetchDataFunction** on file **LambdaFetchDataFunction.java** 

Path: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp/LambdaFetchDataFunction.java

    package org.example.awsserverlessnoteapp;


    import com.amazonaws.services.dynamodbv2.AmazonDynamoDBClient;
    import com.amazonaws.services.dynamodbv2.model.*;
    import com.amazonaws.services.lambda.runtime.Context;
    import com.amazonaws.services.lambda.runtime.RequestHandler;


    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;


    public class LambdaFetchDataFunction implements RequestHandler<Map<String,String>, List<Map<String, String>>> {
    private static final String TABLE_NAME = System.getenv("AWS_DYNAMO_TABLE_NAME_VALUE");
    private AmazonDynamoDBClient ddb;


    public LambdaFetchDataFunction() {
        ddb = (AmazonDynamoDBClient) AmazonDynamoDBClient.builder().withRegion(System.getenv("AWS_REGION_VALUE")).build();
    }
    @Override
    public List<Map<String,String>> handleRequest(Map<String, String> itemValues, Context context) {
        ScanRequest request = new ScanRequest()
                .withTableName(TABLE_NAME);


        ScanResult response = ddb.scan(request);


        List<Map<String, String>> result = new ArrayList<>();
        for (Map<String, AttributeValue> item : response.getItems()) {
            Map<String, String> newItem = new HashMap<>();
            for (Map.Entry<String, AttributeValue> entry : item.entrySet()) {
                newItem.put(entry.getKey(), entry.getValue().getS());
            }
            result.add(newItem);
        }
        return result;
    }
    }

After done, we can build project with **Maven** command

    cd note-app-serverless/aws-serverless-note-app && mvn clean package

If you don't have **Maven**, 
+ you can download at Home Page https://maven.apache.org/install.html
+ Install maven with ubuntu: **sudo apt install maven**

If you received response below, you are successfully compile these function

        INFO] Replacing original artifact with shaded artifact.
        [INFO] Replacing /home/ubuntu-server/Documents/git/github/aws-handson/note-app-serverless/aws-serverless-note-app/target/aws-serverless-note-app-0.0.1-SNAPSHOT.jar with /home/ubuntu-server/Documents/git/github/aws-handson/note-app-serverless/aws-serverless-note-app/target/aws-serverless-note-app-0.0.1-SNAPSHOT-shaded.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  4.656 s
        [INFO] Finished at: 2024-03-05T00:46:40+07:00
        [INFO] ------------------------------------------------------------------------