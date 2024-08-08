---
title : "Tạo hàm Lambda và triển khai"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.1.2 </b> "
---


#### Tạo hàm Lambda

![Create Lambda function and deploy](/images/2/CreateLambda1.jpeg?featherlight=false&width=80pc)

Dưới đây là các bước để tạo hàm Lambda bằng Bảng điều khiển quản lý AWS:

+ Truy cập https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions and choose **Create function**

![Create Lambda function and deploy 2](/images/2/CreateLambda2.jpeg?featherlight=false&width=80pc)

Fill yours function infomation: 

+ Chọn: Author from scratch
+ Function name: điền tên hàm của bạn. ví dụ: LambdaFetchDataFunction
+ Runtime: Java11
+ Architecture: arm64
+ Advanced settings: Chọn Bật VPC
+ VPC: [Chọn VPC chúng ta đã tạo ở bước 1](../2.1.1.-create-vpc-subnet-route-table/)
+ Subnets: Chọn private-subnet
+ Security groups: khuyến nghị mặc định

Sau khi thực hiện xong chọn **Tạo hàm**, Tương tự, chúng ta tiếp tục tạo 3 hàm như hình bên dưới theo thứ tự: **LambdaFetchDataFunction**, **LambdaDeleteDataFunction**, **LambdaFetchDataFunction**

![Create Lambda function and deploy 3](/images/2/CreateLambda3.jpeg?featherlight=false&width=80pc)

#### Chi Tiết Cấu Hình Cho Từng Function

![Create Lambda function and deploy 3](/images/2/LamDaDetail1.png?featherlight=false&width=80pc)

Sau khi làm xong, chúng ta có 3 hàm, bây giờ hãy nhấp vào LambdaFetchDataFunction
![Create Lambda function and deploy 3](/images/2/LamDaDetail2.png?featherlight=false&width=80pc)
Để chức năng hoạt động, chúng ta cần tải mã lên lambda, nhấp vào **Upload** từ tại tab **Code**..
![Create Lambda function and deploy 3](/images/2/CreateLambda4.jpeg?featherlight=false&width=80pc)

Chúng ta có 2 lựa chọn:

+ Upload trực tiếp file .jar, là chúng tôi đã build gói mvn clean ở bước 3
+ Tải lên qua bảng điều khiển S3 và nhận URI công khai rồi điền vào đó

![Create Lambda function and deploy 3](/images/2/CreateLambda5.jpeg?featherlight=false&width=80pc)

Tiếp theo, chúng ta phải cấu hình môi trường thời gian chạy để lambda có thể thực thi đúng chức năng, Nhấp vào **Edit** 
![Create Lambda function and deploy 3](/images/2/CreateLambda6.jpeg?featherlight=false&width=80pc)

Dán : **org.example.awsserverlessnoteapp.LambdaFetchDataFunction::handleRequest**

![Create Lambda function and deploy 3](/images/2/CreateLambda7.jpeg?featherlight=false&width=80pc)
Bây giờ, hãy kiểm tra mã nguồn của chúng tôi. Chúng tôi có một Class tên LambdaFetchDataFunction, Lớp LambdaFetchDataFunction trong **package: org.example.awsserverlessnoteapp**. Lớp LambdaFetchDataFunction có hàm handRequest, thực thi tìm nạp dữ liệu từ DynamoDB
![Create Lambda function and deploy 3](/images/2/CreateLambda8.jpeg?featherlight=false&width=80pc)

Tiếp theo chúng ta sẽ nhấp vào tab Cấu hình, nhấp vào Biến môi trường, chúng ta có thể định cấu hình nhiều giá trị tại đây.

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

Hãy nhấp vào Chỉnh sửa và thêm 2 biến môi trường vào đó.

    Key: AWS_DYNAMO_TABLE_NAME_VALUE, Value: note_tbl
    Key: AWS_REGION_VALUE, Value: us-east-1

Tương tự thiết lập hàm lambda LambdaAddDataFunction,LambdaDeleteDataFunction theo hướng dẫn ở trên.
