---
title: "Giới Thiệu Các Dịch Vụ Lưu Trữ AWS: S3, EBS, Glacier, EFS"
description : "Khám phá các dịch vụ lưu trữ của AWS như S3, EBS và Glacier. Tìm hiểu cách các giải pháp này có thể đáp ứng nhu cầu lưu trữ dữ liệu của bạn với khả năng mở rộng và độ tin cậy."
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 1.3 </b> "
image: "images/1/s3.png"
---


## Giới thiệu dịch vụ lưu Trữ AWS: S3, EBS, Glacier, EFS
AWS cung cấp một bộ dịch vụ lưu trữ toàn diện được thiết kế để đáp ứng nhu cầu đa dạng của các doanh nghiệp và nhà phát triển, từ lưu trữ đối tượng đơn giản đến lưu trữ khối hiệu suất cao.
## Nội dung
### 1. Amazon S3 Storage(Dịch vụ lưu trữ đơn giản):
Amazon S3 là dịch vụ lưu trữ đối tượng có khả năng mở rộng được thiết kế để lưu trữ và truy xuất mọi lượng dữ liệu từ mọi nơi trên web. Nó có độ bền cao, an toàn và tiết kiệm chi phí.
S3 cung cấp các tính năng như lập phiên bản, mã hóa, kiểm soát truy cập và quản lý vòng đời để quản lý dữ liệu một cách hiệu quả.

Nó thường được sử dụng cho nhiều trường hợp sử dụng, bao gồm sao lưu và khôi phục, lưu trữ dữ liệu, phân phối nội dung và lưu trữ các trang web tĩnh.

![Tổng Quan về Các Dịch Vụ Lưu Trữ AWS bao gồm S3, EBS và Glacier](/images/1/s3.png?featherlight=false&width=50pc)
#### Lớp lưu trữ đối tượng(Object Storage Classes):

**Amazon S3 Standard (S3 Standard):** S3 Standard cung cấp khả năng lưu trữ đối tượng có độ bền cao, tính khả dụng và hiệu suất cao cho dữ liệu được truy cập thường xuyên. Vì mang lại độ trễ thấp và thông lượng cao nên S3 Standard phù hợp với nhiều trường hợp sử dụng khác nhau, bao gồm ứng dụng đám mây, trang web động, phân phối nội dung, ứng dụng di động và trò chơi cũng như phân tích dữ liệu lớn. 

Các tính năng chính:
+ Lưu trữ mục đích chung cho dữ liệu được truy cập thường xuyên
+ Độ trễ thấp và hiệu suất thông lượng cao
+ Được thiết kế để cung cấp mức độ khả dụng 99,99% với SLA khả dụng là 99,9%

**Amazon S3 Intelligent-Tiering (S3 Intelligent-Tiering):** thường được viết tắt là S3 Intelligence-Tiering, là lớp lưu trữ được Amazon Web Services (AWS) giới thiệu cho Dịch vụ lưu trữ đơn giản (S3) của họ. Nó được thiết kế để tự động tối ưu hóa chi phí lưu trữ bằng cách di chuyển dữ liệu giữa hai tầng truy cập: truy cập thường xuyên và truy cập không thường xuyên, dựa trên mô hình sử dụng.

Cách thức hoạt động của Phân bậc thông minh S3 (S3 Intelligent-Tiering):

+ Phân bậc tự động (Automatic Tiering): S3 Phân bậc thông minh tự động giám sát các kiểu truy cập dữ liệu của bạn và di chuyển các đối tượng giữa hai tầng truy cập: truy cập thường xuyên và truy cập không thường xuyên. Dữ liệu được truy cập thường xuyên vẫn ở cấp truy cập thường xuyên, trong khi dữ liệu không được truy cập trong một khoảng thời gian sẽ được chuyển sang cấp truy cập không thường xuyên.

+ Tối ưu hóa chi phí(Cost Optimization): Bằng cách tự động di chuyển dữ liệu giữa các tầng truy cập, S3 Phân bậc thông minh giúp tối ưu hóa chi phí lưu trữ. Bạn không cần quản lý thủ công việc di chuyển dữ liệu giữa các tầng, giảm chi phí vận hành.

+ Không có phí truy xuất ( No Retrieval Fees): Không giống như một số lớp lưu trữ khác trong S3, Phân bậc thông minh S3 không phát sinh phí truy xuất khi truy cập dữ liệu từ bậc truy cập không thường xuyên. Điều này làm cho nó đặc biệt hiệu quả về mặt chi phí đối với dữ liệu có kiểu truy cập không thể đoán trước..

+ Độ bền vững và tính khả dụng(Durability and Availability): S3 Phân bậc thông minh cung cấp độ bền và tính khả dụng cao tương tự như các lớp lưu trữ S3 khác, đảm bảo rằng dữ liệu của bạn được bảo vệ trước các lỗi phần cứng và có thể truy cập được khi cần.

+ Giám sát và phân tích(Monitoring and Analytics): AWS cung cấp các công cụ để giám sát và phân tích các mẫu truy cập dữ liệu của bạn được lưu trữ trong Phân bậc thông minh S3. Điều này cho phép bạn hiểu rõ hơn về việc sử dụng dữ liệu của mình và đưa ra quyết định sáng suốt về tối ưu hóa bộ nhớ.

####  Sử dụng S3 trong tình huống nào?
+ Sao lưu và lưu trữ dữ liệu: S3 cung cấp giải pháp đáng tin cậy và tiết kiệm chi phí để lưu trữ các bản sao lưu và lưu trữ dữ liệu. Độ bền cao của nó đảm bảo dữ liệu được an toàn trong thời gian dài, khiến nó trở nên lý tưởng cho các yêu cầu lưu trữ lâu dài.

+ Lưu trữ trang web tĩnh: S3 có thể lưu trữ các trang web tĩnh bằng cách lưu trữ HTML, CSS, JavaScript và các nội dung web khác. Nó tích hợp hoàn hảo với các dịch vụ AWS khác như Amazon Route 53 để định tuyến DNS và Amazon CloudFront để phân phối nội dung, cho phép lưu trữ trang web nhanh chóng và có thể mở rộng quy mô.

+ Phân phối nội dung: S3 kết hợp với Amazon CloudFront cho phép bạn phân phối nội dung trên toàn cầu với độ trễ thấp và tốc độ truyền cao. Điều này hữu ích khi phân phối các tệp lớn, phương tiện truyền phát trực tuyến hoặc nội dung trang web cho người dùng trên toàn thế giới.

+ Lưu trữ dữ liệu ứng dụng: Nhiều ứng dụng sử dụng S3 làm kho lưu trữ trung tâm cho nhiều loại dữ liệu khác nhau, chẳng hạn như nội dung do người dùng tạo, tệp phương tiện, nhật ký ứng dụng và tệp cấu hình. Khả năng mở rộng và độ tin cậy của S3 giúp nó phù hợp để lưu trữ khối lượng lớn các loại dữ liệu đa dạng.

+ Phân tích dữ liệu lớn: S3 đóng vai trò là hồ dữ liệu để lưu trữ lượng lớn dữ liệu có cấu trúc và phi cấu trúc có thể được phân tích bằng các dịch vụ phân tích AWS như Amazon Athena, Amazon Redshift Spectrum hoặc AWS Glue. Dữ liệu được lưu trữ trong S3 có thể được truy vấn trực tiếp mà không cần di chuyển dữ liệu.

+ Khôi phục sau thảm họa: S3 có thể là một phần của chiến lược khắc phục thảm họa, trong đó các bản sao lưu dữ liệu quan trọng được lưu trữ trong các nhóm S3 trên các khu vực AWS khác nhau. Trong trường hợp xảy ra thảm họa, dữ liệu có thể được khôi phục nhanh chóng từ S3 để duy trì hoạt động kinh doanh liên tục.

+ Kho dữ liệu: S3 có thể hoạt động như một khu vực tổ chức cho dữ liệu được đưa vào kho dữ liệu như Amazon Redshift hoặc Amazon Athena. Dữ liệu trước tiên được tải vào S3, sau đó được xử lý bởi các dịch vụ phân tích, cho phép lưu trữ dữ liệu có thể mở rộng và tiết kiệm chi phí.

+ Ứng dụng di động và IoT: S3 thường được sử dụng làm phụ trợ lưu trữ cho các ứng dụng di động và IoT để lưu trữ nội dung, dữ liệu cảm biến và nhật ký ứng dụng do người dùng tạo. Khả năng mở rộng và khả năng tương thích với AWS SDK giúp dễ dàng tích hợp với nhiều nền tảng ứng dụng khác nhau.

### 2. Amazon EBS (Elastic Block Store):
Amazon Elastic Block Store (EBS) là dịch vụ lưu trữ cấp khối do Amazon Web Services (AWS) cung cấp, được thiết kế để sử dụng với các phiên bản Amazon EC2 (Elastic Computing Cloud). 

**Block-Level Storage:**
+ Amazon EBS cung cấp các ổ lưu trữ cấp khối tương tự như ổ cứng vật lý hoặc SSD, cho phép người dùng tạo và đính kèm các ổ lưu trữ vào phiên bản EC2.
+ Các ổ đĩa này xuất hiện dưới dạng thiết bị khối thô đối với phiên bản EC2 và có thể được định dạng và gắn kết giống như bất kỳ thiết bị lưu trữ khối nào khác.

**Persistence and Durability:**
+ EBS volumes được thiết kế cho độ bền và độ tin cậy. Dữ liệu được lưu trữ trên các ổ đĩa EBS được sao chép trong cùng Vùng sẵn sàng để đảm bảo độ bền của dữ liệu.
+ EBS volumes được giữ lâu dài, nghĩa là dữ liệu vẫn tồn tại ngay cả sau khi phiên bản EC2 liên kết bị chấm dứt. Người dùng có thể tách các ổ đĩa EBS khỏi một phiên bản EC2 và gắn chúng vào một phiên bản EC2 khác mà không làm mất dữ liệu.

**High Performance:**
+ EBS volumes cung cấp các tùy chọn lưu trữ hiệu suất cao, bao gồm ổ đĩa hỗ trợ SSD (EBS-SSD) và ổ đĩa từ tính (EBS-HDD), để đáp ứng yêu cầu hiệu suất của các khối lượng công việc khác nhau.
+ SSD-backed volumes cung cấp độ trễ thấp và IOPS (Hoạt động đầu vào/đầu ra mỗi giây) cao cho các ứng dụng đòi hỏi hiệu suất cao, trong khi ổ đĩa từ tính cung cấp khả năng lưu trữ tiết kiệm chi phí cho khối lượng công việc ít đòi hỏi hơn.

**Snapshot and Backup:**
+ EBS volumes hỗ trợ Snapshot, là bản sao lưu tại thời điểm của dữ liệu ổ đĩa được lưu trữ trong Amazon S3. Người dùng có thể tạo ảnh chụp nhanh của khối EBS theo cách thủ công hoặc tự động bằng cách sử dụng ảnh chụp nhanh theo lịch trình.
+ Snapshot tăng dần, nghĩa là chỉ các khối đã thay đổi kể từ ảnh chụp nhanh cuối cùng mới được lưu trữ, giúp giảm chi phí lưu trữ và thời gian sao lưu.

**Encryption and Security:**
+ EBS volumes hỗ trợ mã hóa bằng AWS Key Management Service (KMS), cho phép người dùng mã hóa dữ liệu ở trạng thái lưu trữ để đáp ứng các yêu cầu về tuân thủ và bảo mật.
+ Người dùng cũng có thể chỉ định cài đặt mã hóa khi tạo khối EBS mới hoặc mã hóa các khối hiện có bằng ảnh chụp nhanh.

**Scalability and Flexibility:**
+ EBS volumes có thể được thay đổi kích thước một cách linh hoạt mà không có bất kỳ thời gian ngừng hoạt động nào, cho phép người dùng mở rộng quy mô dung lượng lưu trữ dựa trên việc thay đổi yêu cầu khối lượng công việc.
+ Người dùng có thể chọn từ nhiều loại và kích thước ổ đĩa khác nhau để đáp ứng nhu cầu về hiệu suất và dung lượng cho ứng dụng của mình.

**Integration with AWS Services**
+ EBS volumes có thể được tích hợp với các dịch vụ AWS khác như Amazon EC2, Amazon RDS (Dịch vụ cơ sở dữ liệu quan hệ) và AWS Lambda để cung cấp dung lượng lưu trữ cho nhiều loại ứng dụng và khối lượng công việc khác nhau.
#### Sử dụng EBS trong trường hợp nào?
+ Amazon Elastic Block Store (EBS) được sử dụng trong nhiều trường hợp cần lưu trữ bền vững và có thể mở rộng cho các ứng dụng chạy trên một phiên bản Amazon EC2 duy nhất
### 3. Amazon EFS (Elastic File System)

Amazon EFS là dịch vụ lưu trữ tệp được quản lý hoàn toàn và có thể mở rộng, được thiết kế để cung cấp quyền truy cập chia sẻ và có thể mở rộng vào các tệp từ nhiều phiên bản EC2.
Nó hỗ trợ giao thức NFS (Hệ thống tệp mạng) và có thể được tích hợp liền mạch với các ứng dụng và quy trình công việc hiện có.

EFS phù hợp với các trường hợp sử dụng như quản lý nội dung, xử lý phương tiện, phát triển phần mềm và phân tích dữ liệu yêu cầu lưu trữ tệp dùng chung.
#### Sử dụng EFS trong trường hợp nào?

Amazon EFS thường được sử dụng trong các trường hợp nhiều phiên bản EC2 cần quyền truy cập chung vào một hệ thống tệp chung. Nó cung cấp giải pháp lưu trữ tập trung và có thể mở rộng cho các ứng dụng yêu cầu lưu trữ tệp chia sẻ

### 4. Amazon FSx (Lưu trữ tệp)

Amazon FSx cung cấp các dịch vụ lưu trữ tệp được quản lý toàn phần, được tối ưu hóa cho các trường hợp sử dụng cụ thể, bao gồm Amazon FSx dành cho Windows File Server và Amazon FSx dành cho Lustre.
FSx dành cho Máy chủ tệp Windows cung cấp tính năng chia sẻ tệp Windows có tính sẵn sàng cao, được quản lý đầy đủ, trong khi FSx dành cho Lustre cung cấp hệ thống tệp hiệu suất cao cho khối lượng công việc tính toán chuyên sâu.

FSx phù hợp với các ứng dụng yêu cầu hệ thống tệp hiệu suất cao hoặc lưu trữ tệp tương thích với Windows cho các khối lượng công việc tính toán chuyên sâu như học máy và mô phỏng.
#### Sử dụng FSx trong trường hợp nào?
+ Amazon FSx for Windows File Server được sử dụng trong các trường hợp người dùng cần một hệ thống tệp tương thích với Windows được quản lý hoàn toàn có hỗ trợ giao thức SMB (Server Message Block).
+ Nó cung cấp một hệ thống tệp Windows được quản lý đầy đủ và có thể mở rộng, có thể truy cập được từ các máy khách Windows, Linux và macOS, giúp nó phù hợp để chia sẻ và cộng tác tệp trên các nền tảng khác nhau..


Các dịch vụ lưu trữ của AWS cung cấp các giải pháp đa dạng để đáp ứng nhu cầu lưu trữ dữ liệu. Amazon S3 cung cấp lưu trữ đối tượng linh hoạt cho bất kỳ số lượng dữ liệu nào, trong khi Amazon EBS cung cấp lưu trữ khối cho các phiên bản EC2. Đối với lưu trữ lâu dài với chi phí thấp, Amazon Glacier là giải pháp lý tưởng.