---
title : "Tạo tệp main.tf"
description: "Khám phá cách tạo mẫu cung cấp Terraform để xây dựng các ứng dụng  Zero Downtime Applications. Tìm hiểu hướng dẫn từng bước và các biện pháp thực hành tốt nhất để tự động hóa cơ sở hạ tầng AWS của bạn"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.2.2 </b> "
image: "images/2.2/teraform-aws.jpeg"

---
## Tạo tệp main.tf

    module "aws-resources" {
    source = "./aws-resources"
    }

## Explain

Đoạn mã bạn cung cấp được viết bằng Ngôn ngữ cấu hình HashiCorp (HCL) và là một phần của tệp cấu hình để quản lý tài nguyên AWS (Amazon Web Services) bằng Terraform, một cơ sở hạ tầng phổ biến làm công cụ mã.

**Hãy phân tích từng thành phần :**

Module "aws-resources": Dòng này bắt đầu định nghĩa mô-đun Terraform có tên là "aws-resources". Các mô-đun trong Terraform là các gói cấu hình Terraform có thể tái sử dụng, thường được sử dụng để đóng gói một tập hợp tài nguyên với chức năng cụ thể.

Kết hợp tất cả lại với nhau, khối mã này xác định mô-đun Terraform có tên là "aws-resources" và chỉ định rằng cấu hình của nó phải được tải từ thư mục cục bộ "./aws-resources". Trong thư mục đó, bạn sẽ tìm thấy một tập hợp các tệp cấu hình Terraform xác định tài nguyên AWS sẽ được mô-đun này quản lý.