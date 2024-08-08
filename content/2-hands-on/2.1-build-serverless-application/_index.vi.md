---
title : "Xây dựng ứng dụng ghi chú AWS Serverless "
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1 </b> "
image: "/images/1/LambdaArchitechture.svg" # The path to your image
---


#### Tổng quan

![Build-Serverless-Application](/images/1/LambdaArchitechture.svg?featherlight=false&width=100pc)

Dự án ứng dụng Take Note là một ứng dụng không có máy chủ được thiết kế để cung cấp cho người dùng một nền tảng thuận tiện để ghi và quản lý ghi chú. Được xây dựng bằng dịch vụ AWS, dự án tận dụng các hàm Lambda cho logic phụ trợ và DynamoDB để lưu trữ dữ liệu. Ngoài ra, nó còn tích hợp với GitHub để triển khai tự động bằng AWS CodeBuild.

**Các thành phần và tính năng chính của Dự án ứng dụng Take Note bao gồm:**

+ **Kiến trúc không có máy chủ:** Dự án tuân theo kiến trúc không có máy chủ, loại bỏ nhu cầu quản lý máy chủ và cơ sở hạ tầng. Các hàm AWS Lambda được sử dụng để thực thi logic phụ trợ nhằm phản hồi các sự kiện được kích hoạt bởi hành động của người dùng.
Hàm Lambda: Hàm Lambda đóng vai trò là xương sống của ứng dụng, xử lý nhiều chức năng khác nhau như tạo, truy xuất, cập nhật và xóa ghi chú. Các chức năng này được thiết kế để tự động mở rộng quy mô dựa trên nhu cầu, đảm bảo tính sẵn sàng cao và hiệu suất tối ưu.
+ **Tích hợp DynamoDB:** DynamoDB được sử dụng làm cơ sở dữ liệu NoSQL để lưu trữ và quản lý dữ liệu ghi chú. Việc tích hợp giữa các hàm Lambda và DynamoDB cho phép tương tác liền mạch với cơ sở dữ liệu, cho phép người dùng lưu trữ và truy xuất ghi chú của họ một cách hiệu quả.
+ **Tích hợp GitHub:** Dự án sử dụng AWS CodeBuild để tích hợp và triển khai liên tục (CI/CD) bằng cách tích hợp với kho lưu trữ GitHub. Bất cứ khi nào thay đổi được đẩy tới kho lưu trữ GitHub, CodeBuild sẽ tự động kích hoạt quá trình xây dựng, đảm bảo rằng phiên bản mới nhất của ứng dụng được triển khai trên môi trường AWS.
Xác thực và ủy quyền người dùng: Ứng dụng có thể kết hợp các cơ chế xác thực và ủy quyền người dùng để đảm bảo quyền truy cập an toàn vào dữ liệu ghi chú. Điều này có thể đạt được bằng cách sử dụng AWS Cognito để quản lý và xác thực người dùng, cho phép người dùng đăng nhập an toàn và truy cập ghi chú của họ với các quyền thích hợp.
+ **Giao diện người dùng:** Mặc dù không được đề cập rõ ràng nhưng dự án có thể bao gồm giao diện người dùng, chẳng hạn như ứng dụng web hoặc ứng dụng di động, qua đó người dùng tương tác với Ứng dụng Take Note. Giao diện này giao tiếp với các hàm Lambda phụ trợ để thực hiện các thao tác CRUD trên các ghi chú được lưu trữ trong DynamoDB.

**Tổng quan:**, Dự án ứng dụng Take Note thể hiện sức mạnh và tính linh hoạt của kiến trúc serverless trong việc xây dựng các ứng dụng có thể mở rộng và tiết kiệm chi phí. Bằng cách tận dụng các dịch vụ AWS như Lambda, DynamoDB, CodeBuild và các dịch vụ tiềm năng khác, dự án cung cấp nền tảng mạnh mẽ để người dùng quản lý ghi chú của họ một cách liền mạch, đồng thời tự động hóa quy trình triển khai cho quy trình phát triển hợp lý.

#### Nội dung

[2.1.1 Tạo VPC, Mạng con, Bảng định tuyến](2.1.1.-create-vpc-subnet-route-table/)

[2.1.2. Tạo điểm cuối VPC](2.1.2.-create-vpc-endpoint/)

[2.1.3. Phát triển hàm Lambda bằng ngôn ngữ Java](2.1.3.-develop-lambda-function-with-java-language/)

[2.1.4. Tạo hàm Lambda và triển khai](2.1.4.-create-lambda-function-and-deploy/)

[2.1.5. Tạo DynamoDB](2.1.5.-create-dynamodb/)

[2.1.6. Tạo Cổng API](2.1.6.-create-api-gateway/)

[2.1.7. Giao diện người dùng phát triển](2.1.7.-development-frontend/)

[2.1.8 CICD Tự động triển khai trang tĩnh bằng GitHub](2.1.8-cicd-auto-deploy-static-page-with-github/)

[2.1.9. Tích hợp CICD với CodeBuild Tự động triển khai hàm Lambda](2.1.9.-cicd-integration-with-codebuild-auto-deploy-lambda-function/)