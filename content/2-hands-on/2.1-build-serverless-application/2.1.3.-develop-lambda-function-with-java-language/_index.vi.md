---
title : "Cách Tạo và Triển Khai Lambda Function Trong AWS: Hướng Dẫn Chi Tiết"
description : "Hướng dẫn chi tiết cách tạo và triển khai Lambda Function trong AWS. Tìm hiểu quy trình và thực hành tốt nhất để triển khai ứng dụng serverless hiệu quả."
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.1.3 </b> "
image: "images/1/java&lambda.png"
---



#### Cấu trúc dự án
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

Tham khảo Kho lưu trữ Gihub của tôi và Hãy kiểm tra mã nguồn để hiểu cách tạo các hàm Lambda bằng Java.

**Git Url: https://github.com/daotq2000/aws-handson**


Dưới đây là bảng phân tích các thư mục và tệp có liên quan:

**note-app-serverless:**

+ Đây dường như là thư mục gốc của dự án của bạn. Trong thư mục này có hai thư mục con chính: aws-resource và aws-serverless-note-app.

**aws-resource:**

+ Thư mục này chứa các tài nguyên liên quan đến việc thiết lập cơ sở hạ tầng AWS của bạn. Bên trong aws-resource, có các thư mục con như api-gw, code-deploy và lambda.api-gw có thể chứa các cấu hình và định nghĩa liên quan đến API Gateway.code-deploy dường như chứa tệp định nghĩa quy trình cho AWS CodeDeploy.lambda có thể chứa cấu hình hoặc định nghĩa cho hàm Lambda của bạn.

**aws-serverless-note-app:**

+ Thư mục này dường như chứa mã nguồn cho ứng dụng ghi chú serverless của bạn. Bên trong aws-serverless-note-app có các thư mục như src và target, điển hình của cấu trúc dự án Maven. Thư mục src chứa mã nguồn Java của bạn được sắp xếp hợp lý vào thư mục chính và kiểm tra.

Thư mục chính chứa mã Java chính của ứng dụng, bao gồm các lớp như ApplicationMain.java, LambdaAddDataFunction.java, LambdaDeleteDataFunction.java, v.v. Thư mục tài nguyên có thể chứa các tệp cấu hình ứng dụng, chẳng hạn như application.properties.
 
#### Thực hành

Hãy bắt đầu với **pom.xml**
Chúng ta cần 2 dependency để phát triển core cho lambda với Java Spring Boot Project

**Pom.xml**

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
Tiếp theo, chúng ta phải thêm hàm lambda biên dịch plugin vào tệp **Pom.xml**


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
Đây là tệp phiên bản đầy đủ **https://github.com/daotq2000/aws-handson/blob/main/note-app-serverless/aws-serverless-note-app/pom.xml**

**Lưu ý:** Chúng ta cần tạo 3 chức năng: Thêm, Cập nhật dữ liệu và Tìm nạp dữ liệu và xóa dữ liệu. Chúng ta phải viết mã theo forder: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp

##### Đầu tiên, hãy tạo Hàm **LambdaAddDataFunction** trên tệp **LambdaAddDataFunction.java**

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
**Giải thích:** Do Thông tin của Bảng DynamoDB, các Khu vực có thể thay đổi trong RunTime, nên chúng ta cần đưa nó ra môi trường bằng cách sử dụng **System.getenv(“AWS_REGION_VALUE”)** và **System.getenv(“AWS_DYNAMO_TABLE_NAME_VALUE ”)** để lấy các biến môi trường khi chạy.

        private static final String TABLE_NAME = System.getenv("AWS_DYNAMO_TABLE_NAME_VALUE");
        public LambdaAddDataFunction() {
                ddb = (AmazonDynamoDBClient) AmazonDynamoDBClient.builder().withRegion(System.getenv("AWS_REGION_VALUE")).build();
        }

##### Thứ hai, hãy tạo Hàm **LambdaDeleteDataFunction** trên tệp **LambdaDeleteDataFunction.java**

Đường dẫn: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp/LambdaDeleteDataFunction.java


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


##### Thứ ba, hãy tạo Hàm **LambdaFetchDataFunction** trên tệp **LambdaFetchDataFunction.java**

Đường dẫn: note-app-serverless/aws-serverless-note-app/src/main/java/org/example/awsserverlessnoteapp/LambdaFetchDataFunction.java


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

Sau khi hoàn tất, chúng ta có thể xây dựng dự án bằng lệnh **Maven**

     cd note-app-serverless/aws-serverless-note-app && mvn clean package

Nếu bạn không có **Maven**,
+ các bạn có thể tải về tại Trang chủ https://maven.apache.org/install.html
+ Cài đặt maven bằng ubuntu: **sudo apt install maven**

Nếu bạn nhận được phản hồi bên dưới, bạn đã biên dịch thành công project này

        INFO] Replacing original artifact with shaded artifact.
                [INFO] Replacing /home/ubuntu-server/Documents/git/github/aws-handson/note-app-serverless/aws-serverless-note-app/target/aws-serverless-note-app-0.0.1-SNAPSHOT.jar with /home/ubuntu-server/Documents/git/github/aws-handson/note-app-serverless/aws-serverless-note-app/target/aws-serverless-note-app-0.0.1-SNAPSHOT-shaded.jar
                [INFO] ------------------------------------------------------------------------
                [INFO] BUILD SUCCESS
                [INFO] ------------------------------------------------------------------------
                [INFO] Total time:  4.656 s
                [INFO] Finished at: 2024-03-05T00:46:40+07:00
                [INFO] ------------------------------------------------------------------------