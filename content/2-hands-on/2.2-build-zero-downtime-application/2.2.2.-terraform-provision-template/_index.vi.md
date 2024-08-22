---
title : "Terraform Provision Template for Zero Downtime Applications"
description: "Khám phá cách tạo mẫu cung cấp Terraform để xây dựng các ứng dụng  Zero Downtime Applications. Tìm hiểu hướng dẫn từng bước và các biện pháp thực hành tốt nhất để tự động hóa cơ sở hạ tầng AWS của bạn"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.2 </b> "
image: "images/2.2/teraform-aws.jpeg"
---

## Terraform là gì?
Terraform là một công cụ phần mềm cơ sở hạ tầng nguồn mở dưới dạng mã (IaC) được tạo bởi HashiCorp. Nó cho phép người dùng xác định và cung cấp cơ sở hạ tầng trung tâm dữ liệu bằng ngôn ngữ cấu hình khai báo được gọi là Ngôn ngữ cấu hình HashiCorp (HCL) hoặc JSON tùy chọn.
## Tại sao chúng ta cần sử dụng Terraform?
1. **Cơ sở hạ tầng dưới dạng mã (IaC):** Nó cho phép bạn xác định cơ sở hạ tầng của mình bằng mã, có thể được kiểm soát phiên bản, sử dụng lại và chia sẻ với người khác. Cách tiếp cận này tạo điều kiện thuận lợi cho tính nhất quán và trách nhiệm giải trình, giúp đảm bảo rằng việc triển khai cơ sở hạ tầng có thể lặp lại và ngăn chặn tình trạng lệch giữa các môi trường.

2. **Cú pháp khai báo:** HCL của Terraform là khai báo, nghĩa là bạn mô tả trạng thái cuối cùng mong muốn của cơ sở hạ tầng của mình và Terraform tìm ra cách đạt được trạng thái đó. Điều này giúp loại bỏ các bước thủ tục cần thiết để triển khai, đơn giản hóa đáng kể việc quản lý cơ sở hạ tầng.

3. **Nền tảng bất khả tri:** Terraform có thể quản lý cơ sở hạ tầng trên nhiều nền tảng đám mây cũng như tại chỗ. Khả năng này cho phép bạn quản lý môi trường kết hợp hoặc nhiều đám mây một cách liền mạch chỉ bằng một bộ công cụ duy nhất.

4. **Tính mô-đun:** Cấu hình Terraform có thể bao gồm các mô-đun, giúp bạn dễ dàng đóng gói và đóng gói một tập hợp tài nguyên cũng như tái sử dụng chúng trên các dự án hoặc bộ phận khác nhau trong cơ sở hạ tầng của bạn.

5. **Quản lý trạng thái:** Terraform duy trì một tệp trạng thái chứa trạng thái hiện tại của cơ sở hạ tầng mà Terraform đang quản lý. Trạng thái này cho phép Terraform ánh xạ các tài nguyên trong thế giới thực tới cấu hình của bạn, theo dõi siêu dữ liệu và thực hiện các phụ thuộc tài nguyên cũng như lập kế hoạch thay đổi một cách chính xác.
6. **Tự động hóa và điều phối thay đổi:** Lập kế hoạch và áp dụng các thay đổi theo cách nhất quán và có thể dự đoán được. Nó tính toán sự khác biệt giữa trạng thái hiện tại và trạng thái mong muốn, đồng thời thực hiện các hành động cần thiết để làm cho cơ sở hạ tầng trong thế giới thực khớp với trạng thái mong muốn, xử lý độ phân giải phụ thuộc tài nguyên.
7. **Cộng tác và quy trình làm việc:** Bằng cách tích hợp dễ dàng với các hệ thống kiểm soát phiên bản và hỗ trợ các chương trình phụ trợ trạng thái từ xa, Terraform tạo điều kiện cho sự cộng tác trong và giữa các nhóm. Nó hỗ trợ một quy trình làm việc tiêu chuẩn để lập kế hoạch, xem xét và áp dụng các thay đổi, giúp nó tương thích với các quy trình Tích hợp liên tục/Triển khai liên tục (CI/CD).
8. **Khả năng mở rộng:** Terraform được thiết kế để xử lý cơ sở hạ tầng quy mô lớn, khiến nó phù hợp với cả dự án nhỏ và môi trường doanh nghiệp lớn.
9. **Cộng đồng và hệ sinh thái:** Terraform được hưởng lợi từ một cộng đồng tích cực đóng góp cho hệ sinh thái rộng lớn gồm các nhà cung cấp (plugin cho các dịch vụ và API khác nhau) và mô-đun (cấu hình đóng gói sẵn cho các mẫu thiết lập phổ biến).
10. **An toàn và có thể dự đoán:** Terraform tạo kế hoạch thực hiện trước khi thực hiện bất kỳ thay đổi nào, nghĩa là bạn có thể xem lại những gì Terraform sẽ làm trước khi thực hiện, giúp tránh những thay đổi không mong muốn.
## Thiết lập aws cil và định cấu hình thông tin xác thực
### 1.Cài đặt aws-cli với Hệ điều hành WINDOW
1. Tải xuống và chạy trình cài đặt AWS CLI MSI cho Windows (64-bit): https://awscli.amazonaws.com/AWSCLIV2.msi
   Ngoài ra, bạn có thể chạy lệnh msiexec để chạy trình cài đặt MSI.

   msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi

Để biết các tham số khác nhau có thể được sử dụng với msiexec, hãy xem msiexec trên trang web Microsoft Docs. Ví dụ: bạn có thể sử dụng cờ /qn để cài đặt im lặng.

     msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi /qn

2. Để xác nhận cài đặt, hãy mở menu Bắt đầu, tìm kiếm cmd để mở một
   cửa sổ dấu nhắc lệnh và tại dấu nhắc lệnh, hãy sử dụng lệnh aws --version.

### 2.Cài đặt aws-cli với Hệ điều hành Linux
**Linux x86 (64-bit)**

Linux ARM
Ghi chú
(Tùy chọn) Khối lệnh sau sẽ tải xuống và cài đặt AWS CLI mà không cần xác minh tính toàn vẹn của bản tải xuống của bạn trước tiên. Để xác minh tính toàn vẹn của quá trình tải xuống của bạn, hãy làm theo hướng dẫn từng bước bên dưới.

Để cài đặt AWS CLI, hãy chạy các lệnh sau.

   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

   unzip awscliv2.zip

   sudo ./aws/install`


Để cập nhật bản cài đặt AWS CLI hiện tại của bạn, hãy thêm thông tin về trình cài đặt và liên kết tượng trưng hiện có của bạn để xây dựng lệnh cài đặt bằng cách sử dụng các tham số --bin-dir, --install-dir và --update. Khối lệnh sau sử dụng liên kết tượng trưng mẫu của /usr/local/bin và vị trí trình cài đặt mẫu của /usr/local/aws-cli.
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   
   unzip awscliv2.zip
   
   sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
### 3. Config aws credentials
Sử dụng lệnh bên dưới để định cấu hình thông tin xác thực aw

      aws configure
Nhập thông tin đăng nhập của bạn, hãy lấy nó tại bảng điều khiển AWS

1. Truy cập https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/users
2. Tạo người dùng mới , sau khi tạo xong, chúng sẽ hiện thị tại trang danh sách người dùng.
   ![Create new user](/images/2.2/aws.png?featherlight=false&width=100pc)
3. Xem thông tin và nhận thông tin xác thực
   ![Create new user](/images/2.2/aws2.png?featherlight=false&width=100pc)
4. Định cấu hình thông tin xác thực của bạn như bên dưới (thay thế `XXX` bằng thông tin của bạn)

            `@daotq1:~$ aws configure
         
            AWS Access Key ID [****************]: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
         
            AWS Secret Access Key [****************]: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
         
            Default region name [us-east-1]: us-east-1
         
            Default output format [None]:`


5. Testing info is correct.
   Sử dụng lệnh bên dưới để kiểm tra thông tin xác thực hoạt động
   Nếu bạn nhận được phản hồi như bên dưới nghĩa là nó đã được kết nối thành công.

         luongtx@daotq1:~$ aws iam list-roles
         {
         "Roles": [
         {
         "Path": "/service-role/",
         "RoleName": "aws-code-pipeline",
         "RoleId": "AROA4KDNQH5RRHWH2N6E3",
         "Arn": "arn:aws:iam::846338211683:role/service-role/aws-code-pipeline",
         "CreateDate": "2024-03-04T07:08:45+00:00",
         "AssumeRolePolicyDocument": {
         "Version": "2012-10-17",
         "Statement": [
         {
         "Effect": "Allow",
         "Principal": {
         "Service": "codebuild.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
         }
         ]


## Cấu trúc dự án
Chúng ta sẽ có các thư mục và tệp theo cấu trúc sau: 

    ├── aws-resources
    │   ├── 00_main.tf
    │   ├── 01_vpc.tf
    │   ├── 02_subnet.tf
    │   ├── 03_ig.tf
    │   ├── 04_routeTable.tf
    │   ├── 05_sg.tf
    │   ├── 06_bastionHost.tf
    │   ├── 07_nlb.tf
    │   ├── 08_nat.tf
    │   ├── 09_iam-role.tf
    │   ├── 10_eks-cluster.tf
    │   ├── 11_eks-nodes.tf
    │   ├── 12_registry.tf
    │   ├── 13_database.tf
    │   ├── 14_caching.tf
    │   ├── 15_sqs..tf
    │   ├── 16_s3.tf
    │   ├── 17_alb.tf
    │   ├── 18_cloudfront.tf
    │   ├── 19_waf.tf
    │   ├── 20_code-build.tf
    │   ├── deploy-out.tf
    │   └── variables.tf
    ├── deploy.tfplan
    ├── error.txt
    ├── Infastructure-Design.png
    ├── main.tf
    ├── provider.tf
    ├── readme.md
    ├── script
    │   ├── jenkins-install.sh
    │   └── terraform.tfstate
    ├── ssh
    ├── terraform
    ├── terraform-apply.sh
    ├── terraform-destroy.sh
    ├── terraform-plan.sh
    ├── terraform.tfstate
    └── terraform.tfstate.backup

1. **aws-resources** là thư mục chứa file .tf dùng để tạo các tài nguyên trên AWS như vpc, eks, waf, nat
2. **provider.tf** là tệp chứa các phần phụ thuộc như aws, kubernetes...
3. **terraform-plan.sh** chúng tôi đang sử dụng tệp này để chạy lệnh terraform plan
4. **terraform-apply.sh** chúng tôi đang sử dụng tệp này để chạy lệnh terraform apply
5. **terraform.tfstate** để lưu trữ các liên kết giữa các đối tượng trong hệ thống từ xa và các phiên bản tài nguyên được khai báo trong cấu hình của bạn.
6. **provider.tf** cho phép Terraform tương tác với nhà cung cấp đám mây, nhà cung cấp SaaS và các API khác

## Tham khảo source code mà tôi đã viết
**Github repository: ** https://github.com/daotq2000/aws-iaac-terraform 