---
title : "CICD Tự động triển khai trang tĩnh với GitHub"
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 2.1.8 </b> "
---

#### Tại sao chúng ta cần CICD Tự động phát triển khai trang tĩnh với GitHub
Tích hợp liên tục/Triển khai liên tục (CI/CD) và triển khai tự động các trang tĩnh với GitHub mang lại một số lợi ích:

+ Hiệu quả: Tự động hóa quá trình triển khai giúp giảm bớt sự can thiệp thủ công, giúp tiết kiệm thời gian và công sức cho nhà phát triển. Họ có thể tập trung nhiều hơn vào việc viết mã và ít tập trung hơn vào hậu cần triển khai.

+ Tính nhất quán: Triển khai tự động đảm bảo mỗi lần cập nhật lên trang tĩnh đều được triển khai nhất quán. Điều này làm giảm nguy cơ lỗi của con người có thể xảy ra khi triển khai thủ công.

+ Thời gian đưa ra thị trường nhanh hơn: Quy trình CI/CD có thể tự động kích hoạt triển khai ngay khi các thay đổi được đẩy vào kho lưu trữ. Điều này cho phép chu kỳ phát hành nhanh hơn và thời gian tiếp thị các tính năng hoặc bản cập nhật mới nhanh hơn.

+ Độ tin cậy: Triển khai tự động với quy trình CI/CD thường đáng tin cậy hơn so với triển khai thủ công. Quy trình này được tiêu chuẩn hóa và có thể bao gồm các thử nghiệm tự động để phát hiện lỗi trước khi chúng được đưa vào sản xuất.

+ Tích hợp kiểm soát phiên bản: GitHub cung cấp khả năng kiểm soát phiên bản cho cơ sở mã và việc tích hợp CI/CD với GitHub cho phép triển khai các thay đổi một cách liền mạch. Nhà phát triển có thể dễ dàng theo dõi các thay đổi, quay lại phiên bản trước nếu cần và cộng tác hiệu quả hơn.

+ Khả năng mở rộng: Khi dự án phát triển, việc triển khai tự động sẽ mở rộng quy mô một cách dễ dàng. Cho dù đó là một dự án cá nhân nhỏ hay một ứng dụng doanh nghiệp lớn, quy trình CI/CD có thể xử lý việc triển khai ở mọi quy mô.

+ Hiệu quả về chi phí: Bằng cách tự động hóa quy trình triển khai, tổ chức có thể tiết kiệm chi phí liên quan đến lao động thủ công và giảm nguy cơ ngừng hoạt động hoặc sai sót có thể dẫn đến tổn thất tài chính.

+ Khả năng hiển thị và giám sát: Quy trình CI/CD cung cấp khả năng hiển thị về quá trình triển khai, bao gồm nhật ký và số liệu. Điều này cho phép các nhóm giám sát việc triển khai theo thời gian thực, xác định sự cố nhanh chóng và khắc phục sự cố một cách hiệu quả.

Tóm lại, triển khai tự động CI/CD với GitHub cho các trang tĩnh giúp hợp lý hóa quy trình phát triển, cải thiện độ tin cậy và tăng tốc việc cung cấp các bản cập nhật cho người dùng cuối.
#### Thực hành
![CICD Tự động triển khai trang tĩnh bằng GitHub](/images/2/CICD2.jpeg?featherlight=false&width=80pc)
![CICD Tự động triển khai trang tĩnh bằng GitHub](/images/2/CICD3.jpeg?featherlight=false&width=80pc)
+ Đi tới kho lưu trữ github của bạn, nhấp vào Cài đặt và Nhấp vào Trang.
+ Tiếp theo click vào Static HTML
![CICD Tự động triển khai trang tĩnh bằng GitHub](/images/2/CICD4.jpeg?featherlight=false&width=50pc)
+ Tiếp theo, nhập thông điệp cam kết của bạn và nhấp vào Cam kết thay đổi
![CICD Tự động triển khai trang tĩnh bằng GitHub](/images/2/CICD7.jpeg?featherlight=false&width=50pc)
+ Bây giờ, quay lại tab Code, xem lịch sử triển khai và nhấp vào chi tiết
+ Tuyệt vời, triển khai tại Url: https://daotq2000.github.io/note-app/
![CICD Tự động triển khai trang tĩnh bằng GitHub](/images/2/CICD5.jpeg?featherlight=false&width=50pc)
+ Sau khi mở website của chúng tôi

**Gần như đã hoàn tất, chúng tôi đã tạo thành công ứng dụng serverless với Cổng API và Chức năng Lambda, DynamoDB và Triển khai tự động tích hợp với Trang Github. Tiếp theo chúng ta sẽ tạo CI/CD để tự động triển khai chức năng lambda khi có thay đổi commit git lambda source**