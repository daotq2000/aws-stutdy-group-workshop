---
title : "Dịch vụ điện toán"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---

## Giới thiệu Dịch vụ điện toán Amazon Web Services
Các dịch vụ điện toán do AWS cung cấp cung cấp khả năng tính toán có thể mở rộng cho nhiều khối lượng công việc khác nhau, cho phép doanh nghiệp triển khai và quản lý ứng dụng một cách hiệu quả trên đám mây. Sau đây là phần giới thiệu về một số dịch vụ điện toán chính do AWS cung cấp.

## Nội dung
### 1. Amazon EC2 (Elastic Compute Cloud):

Amazon EC2 là một dịch vụ web cung cấp khả năng tính toán có thể thay đổi kích thước trên đám mây. Nó cho phép người dùng nhanh chóng tăng hoặc giảm quy mô dựa trên nhu cầu, chỉ trả tiền cho những tài nguyên họ sử dụng.
Người dùng có thể chọn từ nhiều loại phiên bản được tối ưu hóa cho các khối lượng công việc khác nhau, chẳng hạn như phiên bản có mục đích chung, tối ưu hóa điện toán, tối ưu hóa bộ nhớ và tối ưu hóa lưu trữ.
Bạn có thể sử dụng phiên bản EC2 cho nhiều ứng dụng, bao gồm lưu trữ web, phát triển ứng dụng, xử lý dữ liệu và AI.

![EC2](/images/1/ec2.png?featherlight=false&width=50pc)

**Các loại phiên bản EC2 ** \
Dưới đây là một số ví dụ có mục đích chung mà từ đó chúng ta có thể chọn:

    T2. micro: Phiên bản nổi tiếng nhất trong AWS là t2. micro, cung cấp 1 CPU và 1 GB bộ nhớ với hiệu suất mạng từ thấp đến trung bình. Nó cũng miễn phí và rất hữu ích cho những cá nhân mới bắt đầu sử dụng AWS.

    M6a Instance: Bộ xử lý AMD EPYC thế hệ thứ ba được sử dụng trong phiên bản M6 là hoàn hảo cho các tác vụ đa năng. Trong m6a có nhiều kích thước khác nhau như m6a.large, m6a.2xlarge, m6a.4xlarge, v.v. m6a lớn cung cấp 2 CPU, bộ nhớ 8GiB và hiệu suất mạng lên tới 12,5 Gigabit.

    M5 instance: Thế hệ phiên bản đa năng mới nhất, được gọi là M5, được trang bị bộ xử lý Xeon Platinum 8175 của Intel. Các phân khu M5 của nó bao gồm m5. lớn, m5.12xlarge và m5.24 lớn và loại dịch vụ M5 mà chúng tôi chọn sẽ phụ thuộc vào bộ nhớ, CPU, bộ lưu trữ và tốc độ mạng.

#### Dùng trong hoàn cảnh nào?
**Ứng dụng Máy chủ Web**: Máy chủ web có thể được lưu trữ trong các phiên bản đa dụng. Phiên bản EC2 cung cấp nền tảng linh hoạt và có thể mở rộng cho các ứng dụng web.
Môi trường thử nghiệm và phát triển: Nhà phát triển có thể sử dụng các phiên bản đa năng này để xây dựng, thử nghiệm và triển khai ứng dụng. Đây là một giải pháp tiết kiệm chi phí để vận hành môi trường này.

**Phân phối nội dung**: Có thể lưu trữ các mạng phân phối nội dung (CDN) phân phối nội dung cho người dùng trên toàn thế giới bằng cách sử dụng các phiên bản có mục đích chung. Phiên bản EC2 có thể được thiết lập để cung cấp nội dung có độ trễ thấp và hiệu suất cao.
Là một lựa chọn phổ biến cho nhiều doanh nghiệp, phiên bản đa năng AWS EC2 cung cấp nền tảng linh hoạt và có thể mở rộng cho nhiều ứng dụng.

### 2. AWS Lambda:
AWS Lambda là dịch vụ điện toán không có máy chủ do Amazon Web Services (AWS) cung cấp, cho phép người dùng chạy mã mà không cần cung cấp hay quản lý máy chủ. Nó cho phép các nhà phát triển tập trung vào việc viết mã cho ứng dụng của họ mà không phải lo lắng về cơ sở hạ tầng cơ bản. Dưới đây là tổng quan chi tiết về AWS Lambda:

![Lambda](/images/1/lambda.png?featherlight=false&width=50pc)

#### Serverless Computing:

Với Lambda, người dùng có thể tải code của họ lên và AWS sẽ đảm nhiệm việc cung cấp, mở rộng quy mô và quản lý cơ sở hạ tầng cần thiết để chạy mã đó.
Người dùng chỉ bị tính phí cho thời gian tính toán mà mã của họ sử dụng, tính bằng mili giây, khiến Lambda trở thành giải pháp tiết kiệm chi phí cho nhiều khối lượng công việc khác nhau.
#### Kiến trúc hướng sự kiện(Event-Driven Architecture):

Hàm Lambda được gọi để phản hồi các sự kiện như yêu cầu HTTP, thay đổi dữ liệu trong bộ chứa Amazon S3, cập nhật lên bảng Amazon DynamoDB, tin nhắn từ Amazon SNS(Simple Notification Service), và nhiều dịch vụ khác.
Kiến trúc hướng sự kiện này cho phép các nhà phát triển xây dựng các ứng dụng có khả năng phản hồi và có thể mở rộng nhằm đáp ứng các thay đổi hoặc trình kích hoạt trong thời gian thực.

#### Thời gian chạy và ngôn ngữ được hỗ trợ(Supported Runtimes and Languages):

Lambda hỗ trợ nhiều ngôn ngữ lập trình bao gồm Node.js, Python, Java, Go, .NET Core và Ruby, cho phép các nhà phát triển sử dụng ngôn ngữ ưa thích của họ để viết các hàm Lambda.
Mỗi hàm Lambda chạy trong một môi trường biệt lập gọi là "thời gian chạy", cung cấp các phần phụ thuộc và thư viện cần thiết cho ngôn ngữ đã chọn.
#### Tự động mở rộng (Automatic Scaling):

Lambda tự động điều chỉnh quy mô môi trường thực thi để xử lý các yêu cầu hoặc sự kiện đến. Nó có thể xử lý một vài yêu cầu mỗi ngày đến hàng nghìn yêu cầu mỗi giây mà không cần bất kỳ sự can thiệp thủ công nào. Việc thay đổi quy mô được AWS xử lý dựa trên số lượng sự kiện đến và cài đặt đồng thời đã định cấu hình.
#### Thực thi phi trạng thái(Stateless Execution):
Hàm Lambda không có trạng thái, nghĩa là chúng không duy trì bất kỳ trạng thái liên tục nào giữa các lần gọi. Mỗi lệnh gọi hàm Lambda độc lập với các lệnh gọi trước đó. Nếu cần có sự duy trì trạng thái, nhà phát triển có thể sử dụng các dịch vụ lưu trữ bên ngoài như Amazon DynamoDB hoặc Amazon S3 để lưu trữ và truy xuất thông tin trạng thái.#### Integration with AWS Services:

Lambda tích hợp liền mạch với các dịch vụ AWS khác, cho phép nhà phát triển xây dựng các ứng dụng và quy trình làm việc phức tạp. Ví dụ: hàm Lambda có thể được sử dụng để xử lý dữ liệu từ Amazon S3, kích hoạt thông báo qua Amazon SNS hoặc tương tác với cơ sở dữ liệu Amazon RDS.

AWS cung cấp khả năng tích hợp tích hợp với nhiều dịch vụ khác nhau thông qua AWS SDK và các nguồn sự kiện, giúp dễ dàng kết nối các chức năng Lambda với các tài nguyên AWS khác.
**Kiểm soát bảo mật và truy cập(Security and Access Control)**:

Các hàm Lambda có thể được bảo mật bằng chính sách AWS IAM (Quản lý danh tính và quyền truy cập), chính sách này kiểm soát ai có thể gọi hàm và những tài nguyên nào hàm đó có thể truy cập.
Nhà phát triển cũng có thể sử dụng AWS Key Management Service (KMS) để mã hóa dữ liệu nhạy cảm trong các hàm Lambda hoặc sử dụng các biến môi trường để lưu trữ cài đặt cấu hình một cách an toàn.
#### Sử dụng lambda trong tình huống nào?
** Kiến trúc Microservices (Microservices Architecture):**

Các hàm Lambda có thể được sử dụng để triển khai các vi dịch vụ trong kiến ​​trúc serverless, trong đó mỗi hàm đại diện cho một thành phần hoặc chức năng cụ thể của ứng dụng.
Bằng cách chia nhỏ ứng dụng của bạn thành các chức năng nhỏ hơn, độc lập, bạn có thể đạt được khả năng mở rộng, tính linh hoạt và khả năng bảo trì tốt hơn, đồng thời giảm chi phí hoạt động của việc quản lý cơ sở hạ tầng.

**Xử lý dữ liệu thời gian thực (Real-time Data Processing):**

Lambda rất phù hợp cho các tác vụ xử lý dữ liệu theo thời gian thực như xử lý luồng, làm giàu dữ liệu và phân tích. Nó có thể được sử dụng để xử lý dữ liệu phát trực tuyến từ các dịch vụ như Amazon Kinesis và Amazon Managed Streaming cho Apache Kafka (Amazon MSK). Bằng cách xử lý dữ liệu trong thời gian thực với Lambda, bạn có thể rút ra thông tin chuyên sâu, kích hoạt hành động và đưa ra quyết định dựa trên thông tin cập nhật, hỗ trợ các ứng dụng phản ứng và dựa trên dữ liệu.

**Nhiệm vụ theo lịch trình và công việc định kỳ(Scheduled Tasks and Cron Jobs):**

Các hàm Lambda có thể được lên lịch để chạy theo các khoảng thời gian được chỉ định bằng cách sử dụng CloudWatch Events hoặc EventBridge (trước đây là CloudWatch Events). Điều này làm cho Lambda trở thành một giải pháp thuận tiện để chạy các tác vụ theo lịch trình, tác vụ định kỳ và tác vụ xử lý hàng loạt mà không cần đến máy chủ hoặc cơ sở hạ tầng chuyên dụng.
Bạn có thể sử dụng Lambda để thực hiện sao lưu dữ liệu định kỳ, tạo báo cáo, dọn dẹp tài nguyên hoặc thực hiện bất kỳ tác vụ định kỳ nào khác.

**Máy chủ Backend Web(Web Application Backends):**

Lambda có thể đóng vai trò là phần phụ trợ cho các ứng dụng web, xử lý các yêu cầu HTTP và thực thi logic ứng dụng để phản hồi. Khi kết hợp với API Gateway, Lambda cho phép các nhà phát triển xây dựng API RESTful không máy chủ một cách nhanh chóng và dễ dàng.
Các hàm Lambda có thể xử lý việc xác thực người dùng, xác thực dữ liệu, logic nghiệp vụ và tương tác cơ sở dữ liệu, cung cấp giải pháp có thể mở rộng và tiết kiệm chi phí để phát triển ứng dụng web.
### 3. Amazon ECS (Elastic Container Service):
Amazon ECS là dịch vụ điều phối bộ chứa được quản lý toàn phần, cho phép người dùng chạy, dừng và quản lý bộ chứa Docker trên một cụm phiên bản EC2.
Người dùng có thể dễ dàng triển khai, quản lý và mở rộng quy mô các ứng dụng được đóng gói bằng ECS. Nó tích hợp với các dịch vụ AWS khác như Cân bằng tải đàn hồi, IAM và CloudWatch để nâng cao chức năng.
ECS cho phép người dùng xây dựng kiến ​​trúc vi dịch vụ và hiện đại hóa ứng dụng của họ bằng cách tận dụng các vùng chứa.

### 4. Amazon EKS (Elastic Kubernetes Service):
Amazon EKS là dịch vụ Kubernetes được quản lý toàn phần giúp dễ dàng triển khai, quản lý và mở rộng quy mô ứng dụng trong bộ chứa bằng Kubernetes trên AWS.
Nó cung cấp một mặt phẳng điều khiển Kubernetes có tính sẵn sàng cao và an toàn mà không cần phải quản lý cơ sở hạ tầng.
Với Amazon EKS, người dùng có thể chạy các ứng dụng Kubernetes bằng cùng công cụ và API mà họ sử dụng tại chỗ, đồng thời hưởng lợi từ khả năng mở rộng và độ tin cậy của AWS.Xử lý dữ liệu theo thời gian thực:.

