---
title : "Tạo VPC endpoint"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.2 </b> "
---


#### Tổng quan

![Build-Serverless-Application](/images/1/LambdaArchitechture.svg?featherlight=false&width=100pc)

Chúng tôi thấy VPC Endpoint kết nối giữa hàm Lambda và DynamoDB.

**Tại sao cần VPC Endpoint để kết nối?**
+ Cách ly VPC: Khi các chức năng Lambda được định cấu hình để chạy trong VPC, chúng sẽ bị cô lập với internet và chỉ có thể truy cập các tài nguyên trong VPC hoặc những tài nguyên có thể truy cập thông qua VPC Endpoint. Điều này đảm bảo mức độ bảo mật cao hơn bằng cách hạn chế quyền truy cập từ bên ngoài.
+ Điểm cuối DynamoDB: DynamoDB là dịch vụ AWS nằm trên cơ sở hạ tầng mạng AWS. Khi được truy cập từ bên trong VPC, các hàm Lambda cần giao tiếp với DynamoDB thông qua giao diện được gọi là VPC Endpoint. VPC Endpoint hoạt động như một cổng cho phép giao tiếp riêng tư giữa các tài nguyên trong các dịch vụ VPC và AWS như DynamoDB mà không cần truy cập internet.
Truy cập mạng con riêng tư: Việc đặt cả hai hàm Lambda và DynamoDB trong cùng một mạng con riêng tư sẽ đảm bảo rằng hoạt động giao tiếp giữa chúng vẫn ở chế độ riêng tư và không truyền qua Internet công cộng. Điều này càng nâng cao tính bảo mật của ứng dụng.
Bằng cách tạo VPC Endpoint cho DynamoDB trong VPC của bạn, các hàm Lambda trong cùng một VPC có thể giao tiếp an toàn với DynamoDB mà không cần truy cập Internet. Thiết lập này đảm bảo rằng việc truyền dữ liệu giữa Lambda và DynamoDB vẫn an toàn và riêng tư, đồng thời tối ưu hóa hiệu suất mạng bằng cách tránh định tuyến Internet không cần thiết.

**Tóm lại, mặc dù các hàm Lambda và DynamoDB có thể cùng tồn tại trong cùng một mạng con riêng tư trong VPC, nhưng các hàm Lambda yêu cầu VPC Endpoint để thiết lập kết nối với DynamoDB khi hoạt động trong môi trường VPC. Điều này đảm bảo liên lạc an toàn và riêng tư giữa hai dịch vụ mà không cần truy cập Internet và không phải trả phí cho đường truyền Internet đi từ Mô hình định giá AWS.**

#### Thực hành

Hãy **Tạo VPC Endpoint** làm theo hướng dẫn bên dưới:
![CreateVPCEndPoint2](/images/2/CreateVPCEndPoint2.jpeg?featherlight=false&width=100pc)
+ Truy cập https://us-east-1.console.aws.amazon.com/vpcconsole/home?region=us-east-1#Endpoints: choose **Create Endpoint**
![CreateVPCEndPoint3](/images/2/CreateVPCEndPoint3.jpeg?featherlight=false&width=100pc)

**Chọn Tùy chọn bên dưới như hình ảnh:**

+ Name tag – optional: Điền tên endpoint VPC
+ Service category: Dịch vụ AWS
+ Services: com.amazonaws.us-east-1.dynamodb
+ VPC: chọn vpc chúng ta đã tạo ở [Step 2.1.1](../2.1.1.-create-vpc-subnet-route-table/)
