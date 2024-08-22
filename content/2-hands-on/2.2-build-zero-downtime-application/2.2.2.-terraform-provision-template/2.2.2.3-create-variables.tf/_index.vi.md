---
title : "Tạo tệp variables.tf"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.2.3 </b> "
---
## Tạo tệp variables.tf
    
    variable "vpc_cidr_block" {
    type = string
    default = "10.0.0.0/16"
    }
    variable "ssh_access_key" {
    type = string
    default = "public-bastion-host"
    }
    variable "aws_role_code_pipeline" {
    type = string
    default = "arn:aws:iam::846338211683:role/service-role/aws-code-pipeline"
    }
    variable "registry_credential" {
    type = string
    default = "registry_credential"
    }
    variable "registry_credential_provider" {
    type = string
    default = "registry_credential_provider"
    }
    variable "repository" {
    type = map
    default = {
    source: "GITHUB",
    url:"https://github.com/daotq2000/aws-spring-sqs-queue.git"
    }
    }
    variable "region" {
    type = string
    default = "us-east1"
    }
    variable "az" {
    type = set(string)
    default = ["us-east-1a","us-east-1b","us-east-1c"]
    }
## Giải thích

Đoạn mã này được viết bằng Ngôn ngữ cấu hình HashiCorp (HCL) và đang xác định một số biến có giá trị mặc định. Hãy chia nhỏ nó ra:

biến **"vpc_cidr_block":** Dòng này xác định một biến có tên "vpc_cidr_block" đại diện cho khối CIDR cho VPC (Đám mây riêng ảo) sẽ được tạo. Nó có kiểu chuỗi và có giá trị mặc định là "10.0.0.0/16".

biến **"ssh_access_key":** Dòng này xác định một biến có tên "ssh_access_key" đại diện cho khóa truy cập SSH cho máy chủ Bastion. Nó có kiểu chuỗi và có giá trị mặc định là "public-bastion-host".

biến **"aws_role_code_pipeline":** Dòng này xác định một biến có tên "aws_role_code_pipeline" đại diện cho vai trò IAM cho AWS CodePipeline. Nó thuộc loại chuỗi và có giá trị mặc định là "arn:aws:iam::846338211683:role/service-role/aws-code-pipeline".

biến **"registry_credential":** Dòng này xác định một biến có tên "registry_credential" đại diện cho thông tin xác thực để truy cập sổ đăng ký. Nó có kiểu chuỗi và có giá trị mặc định là "registry_credential".

biến **"registry_credential_provider"**: Dòng này xác định một biến có tên "registry_credential_provider" đại diện cho nhà cung cấp thông tin xác thực đăng ký. Nó có kiểu chuỗi và có giá trị mặc định là "registry_credential_provider".

biến **"kho lưu trữ"**: Dòng này xác định một biến có tên là "kho lưu trữ" thể hiện thông tin về kho lưu trữ. Nó thuộc loại bản đồ và có giá trị mặc định là bản đồ chứa hai cặp khóa-giá trị: "nguồn" với giá trị "GITHUB" và "url" với giá trị "https://github.com/daotq2000/aws-spring -sqs-queue.git".

biến **"khu vực":** Dòng này xác định một biến có tên "khu vực" đại diện cho khu vực AWS để triển khai tài nguyên. Biến này có kiểu chuỗi và có giá trị mặc định là "us-east1".

biến **"az":** Dòng này xác định một biến có tên "az" đại diện cho các vùng sẵn sàng trong khu vực được chỉ định. Nó thuộc loại set(string) (một tập hợp các chuỗi) và có giá trị mặc định là một tập hợp chứa ba chuỗi: "us-east-1a", "us-east-1b" và "us-east-1c" .

Các biến này có thể được sử dụng sau này trong cấu hình Terraform để tham số hóa các định nghĩa tài nguyên và làm cho cấu hình linh hoạt hơn và có thể sử dụng lại được. Người dùng có thể ghi đè các giá trị mặc định này khi chạy lệnh Terraform nếu cần.