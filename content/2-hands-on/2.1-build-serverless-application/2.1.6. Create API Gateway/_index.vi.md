---
title : "Tạo API Gateway"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.1.6 </b> "
---


#### Tạo API Gateway
Để tạo API Gateway trong AWS, hãy làm theo các bước sau:

##### 1. Truy cập https://console.aws.amazon.com/ và đăng nhập vào tài khoản AWS của bạn.
##### 2. Truy cập https://us-east-1.console.aws.amazon.com/apigateway/main/apis?khu vực=us-east-1
##### 3. Nhấp để Tạo API, chọn REST API nhấp vào Xây dựngChọn API mới và điền tên API của bạn, loại điểm cuối API chọn Khu vực và Tạo API

Trong Amazon API Gateway, có ba loại điểm cuối API mà bạn có thể chọn khi tạo API REST: Khu vực, Tối ưu hóa biên và Riêng tư.

+ Điểm cuối khu vực: Điểm cuối khu vực được triển khai trong một Khu vực AWS cụ thể và chỉ có thể truy cập được từ trong Khu vực đó. Điểm cuối khu vực này phù hợp với các ứng dụng mà tất cả lưu lượng API đều bắt nguồn từ trong cùng Khu vực AWS khi triển khai Cổng API. Điểm cuối khu vực cung cấp độ trễ thấp và lý tưởng cho các dịch vụ phụ trợ cũng được triển khai trong cùng Khu vực AWS.

+ Điểm cuối được tối ưu hóa cho biên: Một điểm cuối được tối ưu hóa cho biên được triển khai tới các vị trí biên của AWS CloudFront, được phân phối trên toàn cầu. Các yêu cầu tới API được định tuyến đến vị trí biên của CloudFront gần nhất, giúp giảm độ trễ cho các máy khách trên toàn thế giới. Loại điểm cuối này tận dụng AWS cơ sở hạ tầng mạng toàn cầu do CloudFront cung cấp, lý tưởng cho các ứng dụng được phân phối trên toàn cầu. Đây là loại điểm cuối mặc định khi tạo API trong API Gateway.

+ Điểm cuối riêng tư: Điểm cuối riêng tư chỉ có thể truy cập được từ bên trong Amazon Virtual Private Cloud (VPC) của bạn bằng cách sử dụng điểm cuối VPC giao diện. Nó phù hợp với các tình huống mà bạn muốn hiển thị API của mình cho các tài nguyên trong VPC hoặc các mạng được kết nối khác thông qua AWS Direct Connect hoặc AWS VPN. Điểm cuối riêng tư cung cấp khả năng bảo mật nâng cao bằng cách duy trì lưu lượng truy cập trong mạng riêng, giảm khả năng tiếp xúc với Internet công cộng.

##### 4. Tạo tài nguyên để tạo Đường dẫn chính cho API.
![Create resource to create main Path for APIs](/images/2/CreateAPIGW1.jpeg?featherlight=false&width=80pc)
Tiếp theo, định cấu hình đường dẫn của bạn như bên dưới và ghi nhớ CORS (Chia sẻ tài nguyên nguồn gốc chéo)
Chia sẻ tài nguyên giữa các nguồn gốc (CORS) là một tính năng bảo mật được trình duyệt web triển khai để ngăn các tập lệnh chạy trên một miền thực hiện yêu cầu sang một miền khác. Khi bạn đang sử dụng Amazon API Gateway để tạo API REST, bạn có thể cần định cấu hình CORS để cho phép các ứng dụng khách chạy trên trình duyệt web có nguồn gốc khác nhau truy cập API của bạn.
Sau khi tạo tài nguyên, bây giờ chúng ta phải tạo một số ánh xạ phương thức với hàm lambda, nhấp vào Tạo phương thức

##### 5. Sau khi tạo xong tài nguyên, bây giờ chúng ta tạo một số phương thức ánh xạ với hàm lambda, nhấn Create Method
![Create Method for APIs](/images/2/CreateAPIGW2.jpeg?featherlight=false&width=80pc)
Tại đây, chúng tôi chọn
Loại phương thức: GET/POST/DELETE
Loại tích hợp: Hàm Lambda
Hàm Lambda: chọn hàm lambda của bạn tạo ở bước trước
Bấm vào Tạo phương thức, sau khi tạo xong chúng ta có phương thức như hình bên dưới.
![Create Method for APIs](/images/2/CreateAPIGW3.jpeg?featherlight=false&width=80pc)
##### 6. Click to tab Test to test API, click Test to test APIs
![test APIs](/images/2/CreateAPIGW4.jpeg?featherlight=false&width=80pc)
![test APIs](/images/2/CreateAPIGW5.jpeg?featherlight=false&width=80pc)
Chúng tôi đã nhận được phản hồi như hình trên. Tiếp theo, tạo phương thức tương tự với phương thức **POST, DELETE** tích hợp với **LambdaDeleteDataFunction, LambdaAddDataFunction**

##### 7. After done, we have to click Deploy API to expose API over internet
![test APIs](/images/2/Deploy5.jpeg?featherlight=false&width=50pc)
**Click Deploy API** 
![test APIs](/images/2/Deploy1.jpeg?featherlight=false&width=50pc)
**Điền tên Stage và mô tả Triển khai của bạn, sau khi nhấp vào Triển khai, chúng tôi đã nhận được phản hồi bên dưới**
![test APIs](/images/2/Deploy3.jpeg?featherlight=false&width=50pc)

- Hãy kiểm tra URL gọi: https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/prod đây là URL công khai mà chúng tôi có thể gọi trên internet công cộng.
Sau khi nhận được API công khai URL, bạn có thể kiểm tra bằng công cụ POSTMAN, TERMINAL
Bạn có thể thử với Yêu cầu cuộn tròn với mẫu: cuộn tròn -X GET {{YourInvoke URL}}/{{Stage}}/{{YourAPIs}}

Example: 

    curl -X GET https://q3q8l57ui9.execute-api.us-east-1.amazonaws.com/dev/notes
