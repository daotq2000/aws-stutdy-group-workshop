---
title : "Infrastructure AWS as Terraform"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.3.3 </b> "
---
## Sản phẩm chính thức: https://spotify-a74af.web.app/
## Github: https://github.com/daotq2000/terraform-spotify-serverless
## Cơ sở hạ tầng
![AWS DESIGN KIẾN TRÚC](/images/3/3.0/architechture.svg?featherlight=false&width=100pc)
![fe.png](/images/3/3.0/prod.jpeg)
## Giới thiệu:

Cơ sở hạ tầng dưới dạng mã (IaC) là một cách tiếp cận mạnh mẽ để quản lý và cung cấp tài nguyên máy tính thông qua các tệp cấu hình mà máy có thể đọc được thay vì cấu hình phần cứng vật lý hoặc các công cụ cấu hình tương tác. Terraform, một công cụ IaC nguồn mở do HashiCorp phát triển, cho phép các nhà phát triển xác định và cung cấp cơ sở hạ tầng một cách nhất quán, có thể lặp lại và tự động. Đối với dự án Ứng dụng Music Serverless này, Terraform sẽ được sử dụng để quản lý toàn bộ cơ sở hạ tầng AWS, đảm bảo việc triển khai, mở rộng quy mô và bảo trì liền mạch.

**Các tính năng chính của Terraform:**

+ Cấu hình khai báo: Terraform sử dụng ngôn ngữ cấu hình cấp cao (HCL) cho phép người dùng xác định cơ sở hạ tầng sẽ trông như thế nào hơn là cách xây dựng nó.
+ Quy hoạch cơ sở hạ tầng: Terraform tạo kế hoạch thực hiện, hiển thị những hành động sẽ được thực hiện để đạt được trạng thái mong muốn, cung cấp quy trình làm việc rõ ràng và minh bạch.
+ Quản lý phụ thuộc: Terraform tự động xử lý các phụ thuộc giữa các tài nguyên, đảm bảo chúng được tạo, cập nhật và hủy theo đúng thứ tự.
+ Quản lý trạng thái: Terraform duy trì một tệp trạng thái để theo dõi cơ sở hạ tầng hiện tại, cho phép cập nhật chính xác và hiệu quả.
+ Mô-đun hóa: Terraform hỗ trợ sử dụng các mô-đun, các thành phần có thể tái sử dụng giúp đơn giản hóa các cấu hình phức tạp và thúc đẩy các phương pháp hay nhất.
+ Nguồn lực cần thiết:

Để thiết lập cơ sở hạ tầng AWS cho Ứng dụng Music Serverless, các tài nguyên sau sẽ được xác định và quản lý bằng Terraform:

**Tài nguyên tính toán:**

+ Amazon Fargate: Để triển khai và chạy các dịch vụ phụ trợ được đóng gói trong container mà không cần quản lý máy chủ.

**Cơ sở dữ liệu:**

+ Amazon Aurora MySQL: Giải pháp cơ sở dữ liệu có tính sẵn sàng cao và có khả năng mở rộng để lưu trữ dữ liệu ứng dụng.

**Bộ nhớ đệm:**

+ Amazon ElastiCache (Redis): Để cải thiện hiệu suất ứng dụng thông qua bộ nhớ đệm.

**Kết nối mạng:**

+ VPC (Virtual Private Cloud): Dùng để tạo mạng an toàn và tách biệt cho ứng dụng.

+ Mạng con: Để tổ chức và phân chia tài nguyên trong VPC.

+ Internet Gateway: Để cho phép ứng dụng truy cập internet.

**Cân bằng tải và tự động chia tỷ lệ:**

+ Cân bằng tải ứng dụng (ALB): Dùng để phân phối lưu lượng truy cập đến trên nhiều mục tiêu.

+ Auto Scaling Group: Để tự động chia tỷ lệ số instance theo nhu cầu.

**Bảo vệ:**

+ Nhóm bảo mật: Để kiểm soát lưu lượng truy cập vào và ra tới tài nguyên.

+ Vai trò và chính sách IAM: Để quản lý quyền và kiểm soát truy cập.

**Quá trình phát triển:**

**Địa hình thiết lập:**

+ Cài đặt Terraform trên môi trường phát triển cục bộ.

+ Định cấu hình thông tin xác thực AWS và cài đặt khu vực.

**Xác định tài nguyên:**

+ Tạo tệp cấu hình Terraform (tệp .tf) để xác định tất cả tài nguyên AWS cần thiết.

+ Chỉ định các thuộc tính và phụ thuộc của tài nguyên.

**Khởi tạo Terraform:**

+ Chạy terraform init để khởi tạo thư mục làm việc của Terraform và tải các plugin của nhà cung cấp.

**Lên kế hoạch và nộp đơn:**

+ Thực hiện terraform plan để tạo và xem xét kế hoạch thực hiện, đảm bảo những thay đổi mong muốn là chính xác.

+ Áp dụng cấu hình bằng terraform apply, cung cấp các tài nguyên được chỉ định trong AWS.

**Quản lý trạng thái:**

+ Sử dụng chương trình phụ trợ từ xa (ví dụ: AWS S3) để lưu trữ tệp trạng thái Terraform, cho phép cộng tác và quản lý trạng thái giữa các nhóm.

**Mô-đun hóa:**

+ Chia cấu hình thành các mô-đun có thể sử dụng lại để tổ chức và bảo trì tốt hơn.

+ Sử dụng đầu vào và đầu ra của mô-đun để tham số hóa cấu hình và cho phép sử dụng lại.

**Đầu ra và các biến:**

+ Xác định các biến để tham số hóa cấu hình, làm cho chúng linh hoạt và có thể tái sử dụng.
+ Sử dụng các giá trị đầu ra để trích xuất và chia sẻ thông tin về tài nguyên được tạo.

**Kiểm soát phiên bản:**

Lưu trữ cấu hình Terraform trong hệ thống kiểm soát phiên bản (ví dụ: Git) để theo dõi các thay đổi và cộng tác với các thành viên trong nhóm.