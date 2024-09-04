---
title : "3. Tối ưu hiệu năng cho Java Application xử lý hàng triệu traffic mỗi ngày "
description: "Khám phá Introduce Series 100 AWS Hands-On Labs, được thiết kế cho người mới bắt đầu tìm hiểu AWS với các bài tập thực hành từng bước"
date : "`r Sys.Date()`"
weight : 3
chapter : false
image: "images/1/logo.png"
---

## Tổng quan kiến trúc 
![PMS_Overview.png](/images/4/PMS_Overview.png)

Hệ thống PMS là hệ thống được phát triển dựa trên kiến trúc microservice , mỗi service sẽ thực hiện các nhiệm vụ, chịu trách nhiệm dựa trên domain business logic mà nó được định nghĩa. Hệ thống được xây dựng trên core Java 
Application sử dụng Spring Boot Framework, Spring Data JPA, Spring Cache, Hibernate... và các thư viện  kinh điển như Apache Kafka, Elastic Search ...

Việc phân chia ứng dụng ra các service khác nhau giúp việc quản lý dễ dàng hơn trong việc maintain, xử lý dữ liệu , giúp tăng hiệu quả trong quá trình scale up ứng dụng không bị giới hạn như kiến trúc mothonic kiểu cũ.
Ứng dụng hiện đang chạy trên PRODUCTION, vì là ứng dụng cho toàn bộ nhân viên công ty X  nên hằng ngày sẽ có 8.000 - 10.000 user hoạt động mỗi ngày, với lượng request cần xử lý lên tới hàng triệu traffic mỗi ngày.
## Hiện trạng
Trong 1 thời dài gian chạy, ứng dụng phát triển lên hàng chục nghìn user, phát sinh hàng triệu request mỗi này,  nên việc đọc/ghi dữ liệu liên tục khiến lượng data lớn được lưu trữ trong database, CPU Database 
thường xuyên đạt ngưỡng 100%, các APIs có thời gian phản hồi chậm, người dùng thường xuyên phàn nàn ứng dụng đơ, lác, thời gian chờ đợi lâu, ghi nhận các apis có thời gian phản hồi trên 10s lớn.

![Danh sách các api,service có thời gian phản hồi lớn > 5s, 10s.](/images/4/elk_full_cpu.png)
Danh sách các api,service có thời gian phản hồi lớn > 5s, 10s.


![CPU database thường xuyên đạt ngưỡng 100%.](/images/4/BeforeOptimize.png)

CPU database thường xuyên đạt ngưỡng 100%.

## Nguyên nhân của việc quá tải 

### 1. Lưu lượng truy cập tăng
![Lưu lượng truy cập tăng](/images/4/traffic.png)
Việc ứng dụng phát triển đem lại lợi ích nhưng cũng đồng thời là thách thức cho việc đảm bảo ứng dụng hoạt động tốt, trơn tru, việc có hàng **100- 10000** người dùng mỗi ngày sẽ làm số lượng request tăng đột 
biến, liên quan đến việc mở rộng ứng dụng kịp thời đáp ứng lưu lượng truy cập cao 

### 2. Code chưa tối ưu
Nguyên nhân 1 phần lớn do việc phát triển nóng, các tính năng được phản triển liên tục theo mô hình agile chưa có nhiều thời gian để tối ưu, việc kiểm soát code vẫn chưa chặt khiến những dòng code chưa thực sự tốt,
chưa thực sự tối ưu đã được đưa lên production khiến ứng dụng đã chậm nay còn chậm thêm, APIs liên tục bị **[503-Service Unavailable](https://fptshop.com.vn/tin-tuc/danh-gia/loi-503-service-unavailable-la-gi-155247), [504-Gateway Time-out](https://cloud.z.com/vn/news/504-gateway-time-out/#:~:text=out%20l%C3%A0%20g%C3%AC%3F-,L%E1%BB%97i%20504%20Gateway%20Time%2Dout%20xu%E1%BA%A5t%20hi%E1%BB%87n%20khi%20c%C3%B3%20s%E1%BB%B1,kho%E1%BA%A3ng%20th%E1%BB%9Di%20gian%20quy%20%C4%91%E1%BB%8Bnh.)**
do lượng truy cập tăng đột biến, ứng dụng k kịp scale nên sẽ xảy ra tình trạng quá tải.

### 3. SQL Query chưa tối ưu.
Nguyên nhân phần lớn của việc quá tải này là do SQL Query có [query cost](https://wecommit.com.vn/sql-execution-plan-trong-toi-uu-sql/) lớn, chiếm lượng lớn CPU trong quá trình thực hiện, dưới đây có thể là 1 số nguyên nhân:

+ Các câu query được sử dụng nhiều , query cost lớn chưa tối ưu. 
+ Sử dụng quá nhiều subquery khiến query chậm chạp.
+ Select toàn bộ bảng, thiếu việc chọn lọc các trường cần thiết. 
+ Query full scan table, chưa thiết kế indexed hợp lý, khiến các câu query ít được sử dụng lại đánh query, còn các query return lượng dữ liệu lớn lại không dùng hoặc không áp dụng index.
+ Không chia partition với các bảng dữ liệu lớn, không limit kết quả theo truy vấn where ....
+ Lạm dụng việc group by, having 