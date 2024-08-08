---
title : "Xây dựng ứng dụng Amazon Zero down time"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2 </b> "
---

![AWS DESIGN ARCHITECTURE](/images/1/ArchitechtureDesign.svg?featherlight=false&width=100pc)
## Video
[![How to build realistic application on AWS](/images/2.2/1.png)](https://youtu.be/xBy9F5qLOYA "Everything Is AWESOME")


### Thử thách 2: Xây dựng Nền tảng thương mại điện tử có khả năng phục hồi AWS

Sự tăng trưởng theo cấp số nhân của mua sắm trực tuyến đòi hỏi các nền tảng thương mại điện tử mạnh mẽ và có thể mở rộng. Lưu lượng truy cập tăng trong thời gian mua sắm cao điểm như Thứ Sáu Đen hoặc Thứ Hai Điện Tử đòi hỏi nền tảng thương mại điện tử phải có khả năng phục hồi và phản hồi cao. Mục tiêu thách thức: Xây dựng một nền tảng thương mại điện tử có khả năng mở rộng cao, linh hoạt và tiết kiệm chi phí trên AWS, có thể xử lý các tải khác nhau, khắc phục các lỗi thành phần mà không có lỗi mà người dùng có thể thấy và duy trì tính nhất quán của dữ liệu.

### Yêu cầu:

#### Cơ sở hạ tầng dưới dạng mã (IaC):
Sử dụng Terraform để xác định và cung cấp toàn bộ cơ sở hạ tầng cần thiết cho nền tảng thương mại điện tử. Kiến trúc phải được khai báo thông qua các biện pháp thực hành IaC để tạo phiên bản và khả năng sử dụng lại.
#### Kiến trúc microservice:
Thiết kế ứng dụng bằng kiến trúc vi dịch vụ, cho phép mở rộng và triển khai độc lập các thành phần dịch vụ riêng lẻ.
Cân bằng tải và tự động điều chỉnh quy mô: Triển khai Cân bằng tải đàn hồi (ELB) để phân phối lưu lượng truy cập ứng dụng đến trên nhiều mục tiêu. Định cấu hình Auto Scaling để tự động điều chỉnh số lượng phiên bản EC2 nhằm đáp ứng khối lượng công việc của ứng dụng.
#### Kubernetes
Việc sử dụng Elastic Kubernetes Service để mở rộng quy mô tài nguyên đảm bảo tính sẵn sàng cao và tính thực tế cho ứng dụng.

#### Cơ sở dữ liệu và bộ nhớ đệm:

Sử dụng Amazon RDS cho cơ sở dữ liệu quan hệ có bản sao chỉ có quyền đọc để xử lý tải đọc cao điểm. Triển khai bộ nhớ đệm bằng Amazon ElastiCache để giảm tải cho cơ sở dữ liệu khi có lưu lượng truy cập cao.
#### Xếp hàng tin nhắn:
Sử dụng Amazon SQS để tách các thành phần ứng dụng
#### Lưu trữ và CDN:
Sử dụng Amazon S3 để lưu trữ nội dung tĩnh và phương tiện sản phẩm. Tích hợp Amazon CloudFront dưới dạng CDN để phân phối nội dung trên toàn cầu với độ trễ thấp. Bảo mật và tuân thủ: Triển khai AWS Identity and Access Management (IAM) để quản lý quyền truy cập. Áp dụng AWS WAF trên ELB và CloudFront để bảo vệ khỏi việc khai thác web. Đảm bảo rằng ứng dụng tuân thủ các tiêu chuẩn và quy định có liên quan như GDPR và PCI DSS.

#### Giám sát, ghi nhật ký và cảnh báo:
Sử dụng AWS CloudWatch để theo dõi các chỉ số về tình trạng và hiệu suất của tài nguyên AWS. Sử dụng AWS CloudTrail để giám sát và ghi nhật ký hoạt động tài khoản. Thiết lập cảnh báo để thông báo cho quản trị viên về mọi vấn đề nghiêm trọng hoặc các hiện tượng bất thường. Khôi phục thảm họa và sao lưu dữ liệu: Thiết lập triển khai nhiều vùng sẵn sàng để đảm bảo tính sẵn sàng cao. Triển khai nhân rộng trên các khu vực địa lý khác nhau để bảo vệ khỏi các lỗi cấp khu vực. Tạo chiến lược sao lưu bằng cách sử dụng chính sách vòng đời của Amazon S3 và Amazon Glacier để lưu trữ dữ liệu lâu dài. Tối ưu hóa chi phí: Thực hiện phân tích chi phí-lợi ích khi sử dụng các dịch vụ khác nhau. Sử dụng AWS Trusted Advisor để biết các đề xuất về cách tiết kiệm chi phí. Triển khai Ngân sách AWS để theo dõi và quản lý chi phí.

#### Sản phẩm bàn giao:

1. Tài liệu thiết kế kiến trúc AWS
2. Tệp mẫu Terraform.
3. Đường ống CI/CD
4. Kiểm tra
5. Báo cáo phân tích chi phí.