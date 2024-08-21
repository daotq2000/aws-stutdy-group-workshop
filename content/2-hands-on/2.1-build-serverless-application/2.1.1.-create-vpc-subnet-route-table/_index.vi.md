---
title : "Cách Tạo VPC, Subnet và Route Table Trong AWS: Hướng Dẫn Chi Tiết"
description : "Khám phá cách tạo Virtual Private Cloud (VPC), subnet và route table trong AWS với hướng dẫn chi tiết này. Hiểu quy trình và thực hành tốt nhất để cấu hình cơ sở hạ tầng mạng của bạn."
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1.1 </b> "
image: "images/1/vpc.png"
---


## Giới Thiệu Về Mạng AWS

Hãy cùng đi sâu vào các khái niệm về VPC, Subnet, Route Table và Security Group:

### **1.1 VPC (Virtual Private Cloud) Là Gì?:**

+ Virtual Private Cloud (VPC) là môi trường mạng ảo được cung cấp bởi Amazon Web Services (AWS).

+ Nó cho phép bạn cung cấp một phần riêng biệt về mặt logic của đám mây AWS nơi bạn có thể khởi chạy các tài nguyên AWS như phiên bản EC2, cơ sở dữ liệu RDS và hàm Lambda.

+ Với VPC, bạn có toàn quyền kiểm soát môi trường mạng của mình, bao gồm dải địa chỉ IP, mạng con, bảng định tuyến và cổng mạng.

+ VPC cung cấp bảo mật bằng cách cho phép bạn xác định chính sách kiểm soát truy cập mạng, thiết lập kết nối VPN và sử dụng các nhóm bảo mật cũng như ACL mạng để hạn chế lưu lượng truy cập.

### **1.2 Subnet(Mạng con) là gì? **

+ Mạng con là một phần được phân đoạn trong dải địa chỉ IP của VPC mà bạn có thể đặt tài nguyên AWS trong đó.

+ Mạng con cho phép bạn tổ chức các tài nguyên trong VPC và xác định các phân đoạn mạng riêng biệt với cấu hình định tuyến, chính sách kiểm soát truy cập và vùng khả dụng của riêng chúng.

+ Mỗi mạng con được liên kết với một vùng sẵn sàng (AZ) cụ thể trong một khu vực và các tài nguyên được triển khai trong mạng con đó nằm trong AZ tương ứng.

+ Mạng con có thể là công khai hoặc riêng tư, tùy thuộc vào việc chúng có đường dẫn đến cổng internet hay không.

### **1.3 Route Table (Bảng lộ trình) là gì ?:**

+ Bảng định tuyến là một bộ quy tắc (tuyến đường) xác định nơi lưu lượng mạng được hướng tới trong VPC.

+ Mỗi mạng con trong VPC phải được liên kết với một bảng định tuyến, bảng này xác định cách thức lưu lượng truy cập vào và ra khỏi mạng con.

+ Bảng lộ trình chứa các tuyến đến các đích cụ thể, chẳng hạn như cổng internet, cổng riêng ảo (dành cho kết nối VPN), kết nối ngang hàng VPC hoặc cổng NAT.

+ Bằng cách định cấu hình các tuyến trong bảng tuyến, bạn có thể kiểm soát cách lưu lượng truy cập giữa các mạng con trong VPC và với các mạng bên ngoài.

### **1.4 Nhóm bảo mật(Security Group) là gì ?:**

+ Nhóm bảo mật hoạt động như một tường lửa ảo cho các phiên bản của bạn để kiểm soát lưu lượng vào và ra.

+ Nó hoạt động ở cấp độ phiên bản và có thể được liên kết với nhiều phiên bản trong một VPC.

+ Nhóm bảo mật cho phép bạn xác định các quy tắc cho phép hoặc từ chối lưu lượng truy cập dựa trên giao thức, cổng và địa chỉ IP.

+ Theo mặc định, tất cả lưu lượng truy cập vào đều bị từ chối và tất cả lưu lượng truy cập ra đều được phép, nhưng bạn có thể tùy chỉnh các quy tắc này để đáp ứng các yêu cầu bảo mật cụ thể của mình.

+ Nhóm bảo mật có trạng thái, nghĩa là nếu bạn cho phép lưu lượng truy cập cho một giao thức và cổng cụ thể thì lưu lượng quay lại sẽ tự động được phép bất kể quy tắc gửi đi.

**Tóm lại, VPC cung cấp môi trường mạng biệt lập trong AWS, mạng con cho phép bạn phân chia VPC thành các mạng nhỏ hơn, bảng định tuyến xác định cách định tuyến lưu lượng trong VPC và các nhóm bảo mật kiểm soát luồng lưu lượng đến và đi từ phiên bản của bạn. Các thành phần này phối hợp với nhau để cung cấp cơ sở hạ tầng mạng an toàn và có thể mở rộng trên đám mây AWS.**

#### Thực hành
Hãy **Tạo VPC** làm theo hướng dẫn bên dưới:
![CreateVPC1](/images/2/CreateVPC1.jpeg?featherlight=false&width=100pc)
+ Truy cập https://us-east-1.console.aws.amazon.com/vpcconsole/home?region=us-east-1#vpcs Chọn **Create VPC**
![CreateVPC2](/images/2/CreateVPC2.jpeg?featherlight=false&width=100pc)
+ Chọn VPC and More
+ Chọn IPv4 CIDR: 10.0.0.0/16
+ Chọn AZ: 1
+ Chọn number public subnet 1
+ Chọn private subnet 1
+ Chọn NAT gateway: none, vì chúng tôi đang kết nối trong tài nguyên AWS nên không cần kết nối internet. Cổng NAT phù hợp với tài nguyên của bên thứ ba trên internet
![CreateVPC3](/images/2/CreateVPC3.jpeg?featherlight=false&width=100pc)
Sau khi hoàn thành các tùy chọn này, hãy nhấp vào **Tạo VPC**. Chờ một phút để cung cấp tài nguyên, Sau khi hoàn tất, chúng ta có một vpc như hình trên.

**Kết luận: Trong hướng dẫn này, chúng tôi sẽ hướng dẫn bạn cách tạo Virtual Private Cloud (VPC), subnet và route table trong AWS. VPC cho phép bạn định nghĩa một mạng ảo trong môi trường AWS, trong khi các subnet giúp phân chia mạng và route tables quản lý lưu lượng giữa chúng. Làm theo các bước này để cấu hình cơ sở hạ tầng mạng của bạn hiệu quả và đảm bảo ứng dụng của bạn hoạt động mượt mà.**