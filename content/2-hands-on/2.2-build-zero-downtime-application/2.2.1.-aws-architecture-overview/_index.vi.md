---
title : "Thiết kế kiến trúc AWS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.1 </b> "
---
![KIẾN TRÚC THIẾT KẾ AWS](/images/2.2/HA_AWS_DESIGN.png?featherlight=false&width=100pc)

### Thiết kế phân tích Kiến trúc AWS
Kiến trúc được mô tả trong hình ảnh thể hiện giải pháp điện toán đám mây AWS với nhiều thành phần khác nhau, mỗi thành phần có một mục đích cụ thể.
Dưới đây là các thành phần và ý nghĩa của chúng:

**Trình duyệt web:** Thể hiện khía cạnh phía máy khách, nơi người dùng tương tác với hệ thống.

**Tường lửa ứng dụng web (WAF):**
Bảo vệ ứng dụng khỏi các mối đe dọa dựa trên web.

**Mặt trận đám mây:**
Mạng phân phối nội dung (CDN) của AWS phân phối nội dung đến người dùng với tính sẵn sàng cao và tốc độ cao.

**Cân bằng tải ứng dụng (ALB):**
Phân phối lưu lượng truy cập ứng dụng đến trên nhiều mục tiêu, chẳng hạn như phiên bản EC2, trong nhiều Vùng sẵn sàng.

**Cụm EKS của Amazon:**
Dịch vụ Kubernetes được quản lý để chạy các ứng dụng trong vùng chứa.

**Mạng con riêng tư với nhóm nút được quản lý EC2:**
Lưu trữ an toàn và có thể mở rộng cho các phiên bản EC2 tạo thành một phần của cụm EKS.

**Cơ sở dữ liệu RDS chính:** Dịch vụ cơ sở dữ liệu quan hệ để quản lý dữ liệu chính.

**Cơ sở dữ liệu RDS bản sao:** Cơ sở dữ liệu quan hệ thứ cấp được sử dụng để chuyển đổi dự phòng và khả năng mở rộng đọc.

**Cụm bộ đệm Redis:** Kho lưu trữ dữ liệu trong bộ nhớ được sử dụng để lưu vào bộ nhớ đệm và làm cơ sở dữ liệu, trình trung chuyển thư và hàng đợi để cải thiện hiệu suất của ứng dụng.
Về mặt quản lý và vận hành cơ sở hạ tầng:

**Terraform:** Cơ sở hạ tầng dưới dạng công cụ mã để xây dựng, thay đổi và tạo phiên bản cơ sở hạ tầng.

**CloudWatch:** Dịch vụ giám sát tài nguyên và ứng dụng đám mây AWS.

**CloudTrail:** Dịch vụ cung cấp bản ghi các hành động được thực hiện bởi người dùng, vai trò hoặc dịch vụ AWS.

**Grafana:** Phân tích và ứng dụng web trực quan hóa tương tác cung cấp biểu đồ, đồ thị và cảnh báo.

**Đăng ký vùng chứa đàn hồi (ECR):** Sổ đăng ký vùng chứa Docker để lưu trữ, quản lý và triển khai hình ảnh vùng chứa.

**Hàng đợi SQS của Amazon:** Dịch vụ xếp hàng tin nhắn để tách rời và mở rộng quy mô các vi dịch vụ, hệ thống phân tán và ứng dụng không có máy chủ.
Và để lưu trữ và triển khai:

**S3:** Dung lượng lưu trữ có thể mở rộng trên đám mây AWS.

**CodeCommit:** Dịch vụ kiểm soát nguồn lưu trữ các kho lưu trữ an toàn dựa trên Git.

Mỗi thành phần này đóng một vai trò quan trọng trong cơ sở hạ tầng đám mây toàn diện, có thể mở rộng và an toàn nhằm khai thác sức mạnh của dịch vụ AWS.**** Dịch vụ cung cấp bản ghi các hành động được thực hiện bởi người dùng, vai trò hoặc dịch vụ AWS.