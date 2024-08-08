---
title : "Xây dựng website nghe nhạc online không máy chủ trên AWS Cloud"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
image: "/images/3/3.0/architechture.svg" # The path to your image
---
## 1.Overview Architechture
![AWS DESIGN ARCHITECTURE](/images/3/3.0/architechture.svg?featherlight=false&width=100pc)

### Thử thách 3: Xây dựng ứng dụng nghe nhạc online Serverless trên Amazon Web Services (AWS)
**Objective:**

Phát triển một ứng dụng không có máy chủ đầy đủ để phát trực tuyến và quản lý nhạc. Ứng dụng phải tận dụng sức mạnh của AWS để triển khai, mở rộng quy mô và quản lý, đảm bảo tính sẵn sàng và hiệu suất cao.

**- Backend:**
+ Công nghệ: Java
+ Framework: Khởi động mùa xuân
+ Mô tả: Triển khai backend sử dụng Spring Boot để xử lý việc streaming nhạc, quản lý người dùng và các chức năng cốt lõi khác. Phần phụ trợ sẽ hiển thị các API RESTful để giao diện người dùng tương tác.
**-Frontend:**
+ Công nghệ: ReactJS
+ Mô tả: Phát triển frontend sử dụng ReactJS để cung cấp giao diện người dùng năng động và đáp ứng. Giao diện người dùng sẽ sử dụng API RESTful do chương trình phụ trợ cung cấp để hiển thị và quản lý nội dung âm nhạc.
**-Database:**
+ Công nghệ: MySQL
+ Dịch vụ: Amazon Aurora MySQL
+ Mô tả: Sử dụng Amazon Aurora MySQL làm giải pháp cơ sở dữ liệu để lưu trữ dữ liệu người dùng, siêu dữ liệu âm nhạc, danh sách phát và các thông tin liên quan khác. Aurora MySQL cung cấp hiệu suất cao và khả năng mở rộng để xử lý lượng lớn dữ liệu.
**-Deployment:**
+ Platform: Amazon Web Services (AWS)

**- Services:Amazon Fargate:**

+ Triển khai các dịch vụ phụ trợ trong môi trường serverless container để đơn giản hóa việc quản lý cơ sở hạ tầng.
+ Amazon Aurora MySQL: Sử dụng dịch vụ cơ sở dữ liệu được quản lý toàn phần này để lưu trữ dữ liệu an toàn và có thể mở rộng.
+ Amazon ElastiCache (Redis): Triển khai Redis cho bộ nhớ đệm nhằm cải thiện hiệu suất và tốc độ của ứng dụng.
+ Cân bằng tải ứng dụng (ALB): Phân phối lưu lượng truy cập đến trên nhiều mục tiêu, đảm bảo tính sẵn sàng cao và khả năng chịu lỗi.
+ Internet Gateway: Kích hoạt tính năng truy cập internet cho ứng dụng.
+ Auto Scaling Group: Tự động mở rộng quy mô ứng dụng theo nhu cầu, đảm bảo hiệu suất ổn định và tiết kiệm chi phí.

**- Infrastructure as Code (IaC):**
+ Công nghệ: Terraform
+ Mô tả: Sử dụng Terraform để xác định và quản lý cơ sở hạ tầng cần thiết cho ứng dụng. Các tập lệnh Terraform sẽ tự động hóa việc cung cấp tài nguyên AWS, đảm bảo môi trường nhất quán và có thể tái tạo.

### Đề bài:
Xây dựng website nghe nhạc online trên nền tảng Amazon Web Service, bằng các công nghệ dưới đây: 

Backend:
- Phát triển API RESTful bằng Spring Boot.
- Thực hiện cơ chế xác thực và ủy quyền.
- Tích hợp với Amazon Aurora MySQL để lưu trữ dữ liệu.
- Sử dụng Redis để lưu vào bộ nhớ đệm dữ liệu được truy cập thường xuyên.

- Frontend:
+ Xây dựng giao diện người dùng đáp ứng bằng ReactJS.
+ Sử dụng API phụ trợ để truy xuất và thao tác dữ liệu.
+ Đảm bảo trải nghiệm người dùng liền mạch với quản lý trạng thái hiệu quả.

- Triển khai và mở rộng quy mô:
+ Chứa các dịch vụ phụ trợ và triển khai chúng bằng Amazon Fargate.
+ Thiết lập Amazon Aurora MySQL để quản lý cơ sở dữ liệu đáng tin cậy và có thể mở rộng.
+ Định cấu hình bộ đệm Redis bằng Amazon ElastiCache.
+ Triển khai Cân bằng tải ứng dụng để quản lý phân phối lưu lượng truy cập.
+ Thiết lập Cổng Internet để truy cập internet.
+ Tạo Nhóm tự động chia tỷ lệ để tự động xử lý các tải khác nhau.
+ Sử dụng Terraform để xác định và cung cấp toàn bộ cơ sở hạ tầng.

- Tài liệu và thử nghiệm:

+ Ghi lại các quy trình kiến trúc, thiết lập và triển khai.
+ Viết bài kiểm tra đơn vị và bài kiểm tra tích hợp cho phần phụ trợ.
+ Đảm bảo kiểm tra kỹ lưỡng giao diện người dùng để duy trì trải nghiệm người dùng không có lỗi.
+ Khi kết thúc thử thách này, bạn sẽ có một ứng dụng âm nhạc không có máy chủ, đầy đủ chức năng chạy trên AWS, với phần phụ trợ mạnh mẽ, giao diện người dùng năng động và cơ sở hạ tầng có thể mở rộng.



