---
title : "How to Create and Configure DynamoDB for Serverless Applications"
description : "Learn how to create and configure DynamoDB for your serverless applications. Follow our step-by-step guide to set up a scalable database in AWS."
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 2.1.5 </b> "
image: "images/1/db.png"
---
## DynamoDB là gì?

Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được quản lý toàn phần, cung cấp hiệu suất nhanh và có thể dự đoán được với khả năng mở rộng liền mạch. Trong hướng dẫn này, chúng tôi sẽ hướng dẫn bạn quy trình tạo và đặt cấu hình DynamoDB cho các ứng dụng serverless trên AWS. Hãy làm theo các bước sau để thiết lập bảng DynamoDB và tích hợp bảng đó với kiến trúc serverless của bạn.

#### Tạo DynamoDB Database
![Tạo DynamoDB Database](/images/2/CreateLambda10.jpeg?featherlight=false&width=50pc)
![Tạo DynamoDB Database](/images/2/CreateLambda11.jpeg?featherlight=false&width=50pc)
![Tạo DynamoDB Database](/images/2/CreateLambda12.jpeg?featherlight=false&width=50pc)

Bây giờ, chúng ta đã gần như hoàn thành việc thiết lập hàm lambda, bước tiếp theo chúng ta cần nguồn dữ liệu để hàm lambda có thể hoạt động và hoạt động chính xác theo sau.

+ Đăng nhập vào Bảng điều khiển quản lý AWS: Truy cập https://console.aws.amazon.com/ và đăng nhập vào tài khoản AWS của bạn.
+ Mở Bảng điều khiển DynamoDB: Sau khi bạn đăng nhập, hãy điều hướng đến dịch vụ DynamoDB bằng cách nhập “DynamoDB” vào thanh tìm kiếm hoặc bằng cách định vị nó trong phần “Cơ sở dữ liệu” trong menu dịch vụ.
+ Tạo Table: **note_tbl** (Bảng này chúng ta đã config ở biến môi trường)
+ Bấm vào nút “Tạo bảng”.
+ Bạn sẽ được nhắc nhập thông tin chi tiết cho bảng mới của mình:
Tên bảng:
Khóa chính: Xác định khóa chính cho bảng của bạn. Điều này bao gồm khóa phân vùng và tùy chọn khóa sắp xếp. DynamoDB sử dụng khóa chính để xác định duy nhất từng mục trong bảng.
Dung lượng được cung cấp so với dung lượng theo yêu cầu: Chọn xem bạn muốn cung cấp dung lượng theo cách thủ công hay sử dụng dung lượng theo yêu cầu.
Nếu chọn dung lượng được cung cấp, bạn sẽ cần chỉ định đơn vị dung lượng đọc và ghi.
Nếu bạn chọn dung lượng theo yêu cầu, DynamoDB sẽ tự động điều chỉnh dung lượng đọc và ghi dựa trên khối lượng công việc của bạn.
Khi bạn đã nhập tất cả các chi tiết cần thiết, hãy nhấp vào “Tạo”.

Sau khi tạo bảng, chúng ta đã thiết lập thành công dynamodb, bước tiếp theo chúng ta cần tạo REST API bằng API Gateway để thực thi hàm lambda và tương tác với Dynamo DB