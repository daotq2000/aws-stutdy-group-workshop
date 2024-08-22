---
title : "Cấu hình AWS Application Load Balancer cho các ứng dụng Zero Down Time"
description: "Tìm hiểu cách cấu hình AWS Application Load Balancer cho các ứng dụng Zero Down Time. Hướng dẫn này bao gồm thiết lập, các biện pháp thực hành tốt nhất và mẹo tối ưu hóa để đảm bảo tính khả dụng và độ tin cậy cao"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.3 </b> "
image: "images/2.2/alb.png"
---
Mặc dù chúng tôi đã thiết lập tài nguyên AWS bằng terraform, nhưng có chứa nhiều thành phần, với độ phức tạp lớn nhưng
chúng tôi thậm chí cần cấu hình một số tài nguyên để ứng dụng hoạt động mà không có lỗi.
## 1. Cấu hình nhóm bảo mật để giao tiếp giữa các tài nguyên riêng tư
Ý nghĩa của bước này là cho phép giao tiếp giữa các tài nguyên trong mạng con riêng tư, theo mặc định, các tài nguyên trong mạng con riêng tư sẽ
từ chối mọi kết nối nếu chúng tôi không có cấu hình bảo mật.

Bước cấu hình là vào **AWS console => EC2 => Security Group**
![create_admin_user.png](/images/2.4-config/sg-eks-remote.png)
### Chỉnh sửa quy tắc gửi đến
Nhấp để Chỉnh sửa quy tắc gửi đến
![create_admin_user.png](/images/2.4-config/inbound.png)
![create_admin_user.png](/images/2.4-config/inbound1.png)

Cấu hình gửi đến thành công
![create_admin_user.png](/images/2.4-config/config-inboumd-success.png)

### Chỉnh sửa quy tắc gửi đi
![create_admin_user.png](/images/2.4-config/outbound.png)
![create_admin_user.png](/images/2.4-config/outboundSuccessfully.png)

## 2. Cấu hình nhóm mục tiêu cho Application Load Balancer
Cấu hình mục tiêu cho bộ cân bằng tải, bộ cân bằng tải sẽ chuyển tiếp yêu cầu từ cloudfront đến Nginx-Controller bên trong
EKS Cluster.
Thực hiện theo các bước sau:
1. Nhấp vào `alb-target-group`
   ![create_admin_user.png](/images/2.4-config/configALB.png)
2. Đăng ký nhóm mục tiêu
   ![create_admin_user.png](/images/2.4-config/config-targetGroup.png)
- Cổng `30080` là NodePort của Nginx Controller.
3. Tại tab Kiểm tra tình trạng sức khỏe, nhấp vào `Chỉnh sửa`
   ![create_admin_user.png](/images/2.4-config/success-config-alb.png)
   ![create_admin_user.png](/images/2.4-config/config-heathy.png)
4. Cấu hình kiểm tra tình trạng sức khỏe thành công
   ![create_admin_user.png](/images/2.4-config/success-config-alb.png)

Sau khi cấu hình kiểm tra tình trạng sức khỏe thành công, bộ cân bằng tải ứng dụng sẽ chuyển tiếp yêu cầu đến nginx-controller.