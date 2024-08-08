---
title : "Testing Application"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.2.6 </b> "
---
Mặc dù chúng tôi đã thiết lập tài nguyên AWS theo địa hình nhưng chúng chứa nhiều thành phần, có độ phức tạp lớn nhưng
chúng tôi thậm chí cần phải định cấu hình một số tài nguyên để ứng dụng hoạt động mà không gặp lỗi.
## 1. Cấu hình nhóm bảo mật để liên lạc giữa các tài nguyên riêng tư
Ý của bước này là cho phép liên lạc giữa các tài nguyên trong mạng con riêng tư, theo mặc định, các tài nguyên trong mạng con riêng tư sẽ
từ chối mọi kết nối nếu chúng tôi không có cấu hình bảo mật.

Bước cấu hình được chuyển tới **Bảng điều khiển AWS => EC2 => Nhóm bảo mật**
![create_admin_user.png](/images/2.4-config/sg-eks-remote.png)
### Chỉnh sửa quy tắc gửi đến
Nhấp để chỉnh sửa quy tắc gửi đến
![create_admin_user.png](/images/2.4-config/inbound.png)
![create_admin_user.png](/images/2.4-config/inbound1.png)

Cấu hình gửi đến thành công
![create_admin_user.png](/images/2.4-config/config-inboumd-success.png)

### Chỉnh sửa quy tắc gửi đi
![create_admin_user.png](/images/2.4-config/outbound.png)
![create_admin_user.png](/images/2.4-config/outboundSuccessively.png)

## 2. Cấu hình nhóm mục tiêu cho cân bằng tải ứng dụng
Mục tiêu cấu hình cho bộ cân bằng tải, bộ cân bằng tải sẽ chuyển tiếp yêu cầu từ cloudfront đến Nginx-Controller bên trong
Cụm EKS.
Thực hiện theo các bước:
1. Nhấp vào `alb-target-group`
   ![create_admin_user.png](/images/2.4-config/configALB.png)
2. Đăng ký nhóm đối tượng
   ![create_admin_user.png](/images/2.4-config/config-targetGroup.png)
- Port `30080` là NodePort của Nginx Controller.
3. Tại tab Kiểm tra sức khỏe, nhấp vào `Chỉnh sửa`
   ![create_admin_user.png](/images/2.4-config/success-config-alb.png)
   ![create_admin_user.png](/images/2.4-config/config-heathy.png)
4. Kiểm tra sức khỏe cấu hình thành công
   ![create_admin_user.png](/images/2.4-config/success-config-alb.png)

Sau khi cấu hình kiểm tra tình trạng thành công, bộ cân bằng tải ứng dụng sẽ chuyển tiếp yêu cầu tới bộ điều khiển nginx.