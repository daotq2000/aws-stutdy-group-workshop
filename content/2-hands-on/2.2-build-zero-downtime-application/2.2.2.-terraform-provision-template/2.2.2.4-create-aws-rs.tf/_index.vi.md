---
title : "Tạo các tài nguyên AWS"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.2.4 </b> "
---
## Tạo thư mục tài nguyên aws
Thư mục này chứa nhiều tài nguyên như VPC, Load Balancer, Ec2...
Cấu trúc tương tự như dưới đây:
    
    ├── 00_main.tf
    ├── 01_vpc.tf
    ├── 02_subnet.tf
    ├── 03_ig.tf
    ├── 04_routeTable.tf
    ├── 05_sg.tf
    ├── 06_bastionHost.tf
    ├── 07_nlb.tf
    ├── 08_nat.tf
    ├── 09_iam-role.tf
    ├── 10_eks-cluster.tf
    ├── 11_eks-nodes.tf
    ├── 12_registry.tf
    ├── 13_database.tf
    ├── 14_caching.tf
    ├── 15_sqs..tf
    ├── 16_s3.tf
    ├── 17_alb.tf
    ├── 18_cloudfront.tf
    ├── 19_waf.tf
    ├── 20_code-build.tf
    ├── deploy-out.tf
    └── variables.tf


### Tạo file 00_main.tf

    provider "aws" {
    region = "us-east-1" # replace with your AWS region, e.g., "us-east-1"
    }

Mục đích chính của tệp này là khai báo các tài nguyên được sử dụng trong dự án
### Tạo file 01_vpc.tf
    
    resource "aws_vpc" "vpc-main" {
    vpc-main = var.vpc_cidr_block
    enable_dns_hostnames = true
    enable_dns_support = true
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }

Mục đích chính của file này là khai báo VPC, terraform sẽ tạo VPC với tên `vpc-main` và cidr_block mà chúng ta đã khai báo tại `variables.tf`.

Tại file `variables.tf` chúng ta đã xác định cidr_block

    variable "vpc_cidr_block" {
    type = string
    default = "10.0.0.0/16"
    }
Sau khi chạy terraform plan, apply , nó tạo ra vpc có tên là `vpc-main` và `vpc_cidr_block` là `10.0.0.0/16`

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![VPC created from terraform](/images/2.2/vpc1.png?featherlight=false&width=100pc)
![VPC detail created from terraform](/images/2.2/vpc2.png?featherlight=false&width=100pc)

### Tạo file 02_subnet.tf

    `resource "aws_subnet" "public-subnet-1a" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.1.0/24" # example CIDR block
    availability_zone = "us-east-1a"  # use the availability zone in your region
    
    map_public_ip_on_launch = true # for the bastion host to be accessible from the internet
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1a" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.2.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1a" # use the availability zone in your region
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1b" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.3.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1b" # use a different availability zone
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_subnet" "private-subnet-1c" {
    vpc_id     = aws_vpc.vpc-main.id
    cidr_block = "10.0.4.0/24" # example CIDR block
    map_public_ip_on_launch = false
    availability_zone = "us-east-1c" # use another different availability zone
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    #Create subnets group for rds cluster
    resource "aws_db_subnet_group" "subnet_group" {
    name       = "rds-subnet-group"
    subnet_ids = [aws_subnet.private-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id]
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    # Create subnets group for elastic cache
    resource "aws_elasticache_subnet_group" "redis_subnet_group" {
    name       = "redis-subnet-group"
    subnet_ids = [aws_subnet.private-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id]
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }
    `
Cấu hình Terraform này xác định một số mạng con AWS trong một VPC được chỉ định. Hãy chia nhỏ từng phần:

**1. Mạng con công cộng (us-east-1a):** Khối này xác định mạng con công cộng có tên "public-subnet-1a" với khối CIDR "10.0.1.0/24" trong vùng khả dụng "us-east-1a".
map_public_ip_on_launch được đặt thành true, cho biết rằng các phiên bản được khởi chạy trong mạng con này phải được chỉ định địa chỉ IP công cộng.
Nó được gắn thẻ với siêu dữ liệu mô tả.

**2. Mạng con riêng tư (us-east-1a, us-east-1b, us-east-1c):** Tương tự như mạng con công cộng, nhưng map_public_ip_on_launch được đặt thành sai, cho biết các phiên bản được khởi chạy trong các mạng con này sẽ không có địa chỉ IP công cộng .

**3. Nhóm mạng con DB:** Khối này tạo một nhóm mạng con DB có tên là "rds-subnet-group" cho cụm RDS.
Nó bao gồm các mạng con riêng được tạo trước đó trong subnet_ids của nó.

**4. Nhóm mạng con Elasticache:** Tương tự như nhóm mạng con DB, khối này tạo một nhóm mạng con Elasticache có tên là "redis-subnet-group".
Nó cũng bao gồm các mạng con riêng được tạo trước đó trong subnet_ids của nó.

Các tài nguyên này được tổ chức trong VPC và cho phép phân chia các tài nguyên dựa trên nhu cầu về khả năng tiếp cận công khai hoặc sự liên kết của chúng với các dịch vụ cụ thể như RDS hoặc Elasticache. Ngoài ra, chúng còn được gắn thẻ để quản lý và nhận dạng tốt hơn trong môi trường AWS.

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![Subnet created from terraform](/images/2.2/Subnet1.png?featherlight=false&width=100pc)
![Subnet detail created from terraform](/images/2.2/Subnet2.png?featherlight=false&width=100pc)

### Tạo file 03_ig.tf

    `
    resource "aws_internet_gateway" "gw" {
    vpc_id = aws_vpc.vpc-main.id
    tags = {
    name="terraform project"
    description = "managed by terraform provisioning"
    }
    }`

**Resource Type:** Khối này xác định tài nguyên thuộc loại aws_internet_gateway. Trong AWS, cổng internet cho phép giao tiếp giữa các phiên bản trong VPC của bạn và internet.

**Resource Name:** "gw" là tên được đặt cho khối tài nguyên cổng internet cụ thể này. Nó được sử dụng để chỉ tài nguyên này ở nơi khác trong cấu hình Terraform.

**VPC Association:** vpc_id = aws_vpc.vpc-main.id associates cổng internet này với một VPC cụ thể. aws_vpc.vpc-main.id tham chiếu ID của VPC có tên "vpc-main", ID này phải được xác định trước đó trong cấu hình.

**Tags:** Khối này xác định các thẻ cho cổng internet, cung cấp siêu dữ liệu cho mục đích nhận dạng và tổ chức. Trong trường hợp này, nó có hai thẻ: "tên" và "mô tả", cả hai đều chỉ ra rằng tài nguyên này là một phần của "dự án địa hình" và được "quản lý bằng cách cung cấp địa hình".

Nhìn chung, cấu hình này thiết lập một cổng internet trong một VPC được chỉ định và áp dụng các thẻ mô tả để quản lý và nhận dạng dễ dàng hơn trong môi trường AWS.

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![Internet Gateway created from terraform](/images/2.2/igw1.png?featherlight=false&width=100pc)

### Tạo file 04_routeTable.tf
    
    `resource "aws_route_table" "rtb-public" {
    vpc_id = aws_vpc.vpc-main.id
    route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.gw.id
    }
    
    }
    resource "aws_route_table" "rtb-internal" {
    vpc_id = aws_vpc.vpc-main.id
    route {
    cidr_block = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.nat-gw.id
    }
    
    }
    
    # Associate the route table with the public subnet
    resource "aws_route_table_association" "associate_public_1a" {
    subnet_id      = aws_subnet.public-subnet-1a.id
    route_table_id = aws_route_table.rtb-public.id
    
    }
    resource "aws_route_table_association" "associate_private_1a" {
    subnet_id = aws_subnet.private-subnet-1a.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    resource "aws_route_table_association" "associate_private_1b" {
    subnet_id = aws_subnet.private-subnet-1b.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    resource "aws_route_table_association" "associate_private_1c" {
    subnet_id = aws_subnet.private-subnet-1c.id
    route_table_id = aws_route_table.rtb-internal.id
    
    }
    `
Cấu hình Terraform này thiết lập các bảng định tuyến để quản lý lưu lượng mạng trong VPC, liên kết các tuyến khác nhau với các mạng con cụ thể. Hãy chia nhỏ từng phần:

1. Bảng lộ trình công cộng: Khối này xác định bảng lộ trình có tên "rtb-public" được liên kết với VPC có tên "vpc-main".
   Nó bao gồm một tuyến đường có khối CIDR "0.0.0.0/0", đại diện hiệu quả cho tất cả các địa chỉ IP. Tuyến đường này hướng lưu lượng truy cập đến cổng internet được chỉ định bởi `aws_internet_gateway.gw.id`
2. Bảng lộ trình nội bộ: Tương tự như bảng lộ trình công cộng, khối này xác định bảng lộ trình có tên "rtb-internal" được liên kết với VPC có tên "vpc-main".
   Nó bao gồm một tuyến có khối CIDR "0.0.0.0/0", hướng lưu lượng truy cập đến cổng NAT được chỉ định bởi aws_nat_gateway.nat-gw.id. Thiết lập này phổ biến đối với các mạng con riêng tư để cho phép truy cập internet bên ngoài trong khi vẫn giữ các phiên bản ở chế độ riêng tư.
3. Liên kết bảng định tuyến: Các khối này liên kết các bảng định tuyến được xác định trước đó với các mạng con cụ thể.

Ví dụ: "associate_public_1a" liên kết bảng lộ trình "rtb-public" với mạng con công cộng "public-subnet-1a".

Tương tự, "associate_private_1a", "associate_private_1b" và "associate_private_1c" liên kết bảng tuyến đường "rtb-internal" với các mạng con riêng tư "private-subnet-1a", "private-subnet-1b" và "private-subnet-1c ", tương ứng.

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![Subnet created from terraform](/images/2.2/rtb1.png?featherlight=false&width=100pc)
![Subnet detail created from terraform](/images/2.2/rtb2.png?featherlight=false&width=100pc)
### Tạo file 05_sg.tf
    
    `resource "aws_security_group" "bastion_sg" {
    name        = "bastion_security_group"
    description = "Allow SSH inbound traffic bastion_sg"
    vpc_id      = aws_vpc.vpc-main.id
    
    ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [
    "0.0.0.0/0", aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block
    ]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    
    
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_security_group" "sg_private_eks_node" {
    name = "Security group private eks node"
    vpc_id = aws_vpc.vpc-main.id
    ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [aws_vpc.vpc-main.cidr_block,aws_subnet.private-subnet-1a.cidr_block,aws_subnet.private-subnet-1b.cidr_block,aws_subnet.private-subnet-1c.cidr_block,]
    }
    egress {
    from_port   = 6379
    to_port     = 6379
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 5432
    to_port     = 5432
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    
    }
    resource "aws_security_group" "postgres_aurora_sg" {
    name   = "security_rds_cluster"
    vpc_id = aws_vpc.vpc-main.id
    
    ingress {
    from_port   = 6739
    to_port     = 6739
    protocol    = "tcp"
    cidr_blocks = [
    aws_vpc.vpc-main.cidr_block, aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block
    ]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    resource "aws_security_group" "redis_sg" {
    name   = "redis_sg"
    vpc_id = aws_vpc.vpc-main.id
    ingress {
    from_port = 6379
    to_port = 6379
    protocol = "tcp"
    security_groups = [aws_security_group.bastion_sg.id]
    cidr_blocks = [aws_vpc.vpc-main.cidr_block,aws_subnet.public-subnet-1a.cidr_block,
    aws_subnet.private-subnet-1a.cidr_block,aws_subnet.private-subnet-1b.cidr_block,aws_subnet.private-subnet-1c.cidr_block,]
    }
    egress {
    from_port = 0
    to_port = 0
    protocol = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    }
    #Create SG for Application Load Banlancer
    resource "aws_security_group" "alb_sg" {
    name   = "application_loadbalancer_sg"
    vpc_id = aws_vpc.vpc-main.id
    tags = {
    name        = "terraform project"
    description = "managed by terraform provisioning"
    }
    ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    }
    egress {
    from_port   = 0
    to_port     = 0
    protocol    = "tcp"
    cidr_blocks = [aws_subnet.private-subnet-1a.cidr_block, aws_subnet.private-subnet-1b.cidr_block,
    aws_subnet.private-subnet-1c.cidr_block, aws_subnet.public-subnet-1a.cidr_block]
    }
    }
    `
Cấu hình Terraform này tạo ra một số nhóm bảo mật AWS, mỗi nhóm có quy tắc gửi vào và gửi đi cụ thể để kiểm soát lưu lượng trong VPC. Hãy chia nhỏ từng phần:
**Bastion Security Group:**
Được đặt tên là "bastion_sg", nó cho phép lưu lượng truy cập vào SSH (cổng 22) từ mọi nơi (0.0.0.0/0) cũng như từ các khối CIDR của mạng con riêng tư và công cộng.

Nó cho phép tất cả lưu lượng truy cập đi (0.0.0.0/0) và cũng cho phép lưu lượng truy cập đi trên các cổng 6379 (Redis) và 5432 (PostgreSQL) tới các khối CIDR của mạng con riêng tư và công cộng.

**Private EKS Node Security Group:**
Được đặt tên là "sg_private_eks_node", nó cho phép lưu lượng truy cập vào SSH (cổng 22) từ các khối CIDR của VPC và các mạng con riêng tư.
Nó cho phép lưu lượng truy cập đi trên các cổng 6379 (Redis) và 5432 (PostgreSQL) tới các khối CIDR của mạng con riêng tư và công cộng cũng như đến bất kỳ đích nào.
Nhóm bảo mật cụm RDS:

Được đặt tên là "postgres_aurora_sg", nó cho phép lưu lượng truy cập vào trên cổng 6739 (dành cho RDS hoặc Aurora) từ các khối CIDR của VPC và mạng con.
Nó cho phép lưu lượng truy cập đi tới các khối CIDR của mạng con riêng tư và công cộng.
Nhóm bảo mật Redis:

Được đặt tên là "redis_sg", nó chỉ cho phép lưu lượng truy cập vào trên cổng 6379 (Redis) từ nhóm bảo mật "bastion_sg" và từ các khối CIDR của VPC và mạng con.
Nó cho phép tất cả lưu lượng truy cập đi (0.0.0.0/0).

**Application Load Balancer (ALB) Security Group:**

Được đặt tên là "alb_sg", nó cho phép tất cả lưu lượng truy cập vào và ra từ và đến mọi nơi (0.0.0.0/0) trên tất cả các cổng (-1 cho tất cả các giao thức).
Nó cho phép lưu lượng truy cập đi trên tất cả các cổng tới khối CIDR của mạng con riêng tư và công cộng.

##### Tất cả các nhóm bảo mật đều được gắn thẻ cho mục đích nhận dạng và quản lý. Họ kiểm soát luồng lưu lượng giữa các loại tài nguyên khác nhau trong VPC, đảm bảo tính bảo mật và tuân thủ các chính sách mạng.

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![Security Group created from terraform](/images/2.2/sg.png?featherlight=false&width=100pc)
![Security Group created from terraform](/images/2.2/sg-detail.png?featherlight=false&width=100pc)


### Tạo file 06_bastionHost.tf

      `resource "aws_instance" "bastion_host" {
      ami           = "ami-0c101f26f147fa7fd" # example AMI, update to the latest
      instance_type = "t2.micro" # example instance type, choose as per need
      subnet_id     = aws_subnet.public-subnet-1a.id
      key_name      = var.ssh_access_key # specify your key pair name
      security_groups = [aws_security_group.bastion_sg.id]
      user_data = <<EOF
      sudo yum update –y
      sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
      sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
      sudo yum upgrade
      sudo dnf install java-17-amazon-corretto -y
      sudo yum install jenkins -y
      sudo systemctl enable jenkins
      sudo systemctl start jenkins
      sudo systemctl status jenkins
      EOF
      
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }`

**Resource Type and Name:** Nó xác định tài nguyên phiên bản AWS có tên là "bastion_host". Loại tài nguyên này đại diện cho một máy chủ ảo trong đám mây Amazon Web Services (AWS).

**AMI and Instance Type:** Tham số ami chỉ định ID hình ảnh máy Amazon (AMI) sẽ sử dụng để khởi chạy phiên bản. Trong trường hợp này, nó sử dụng giá trị giữ chỗ (ami-0c101f26f147fa7fd). Tham số instance_type chỉ định loại phiên bản sẽ khởi chạy, trong trường hợp này là phiên bản "t2.micro".

**Subnet and Security:** Tham số subnet_id chỉ định mạng con mà phiên bản sẽ được khởi chạy. Nó sử dụng một biến (aws_subnet.public-subnet-1a.id), biến này phải chứa ID mạng con thực tế. Tham số security_groups xác định (các) nhóm bảo mật được liên kết với phiên bản, đảm bảo an ninh mạng.

**Key Pair:** Tham số key_name chỉ định tên của cặp khóa SSH sẽ được sử dụng để kết nối với phiên bản. Nó sử dụng một biến (var.ssh_access_key), biến này phải chứa tên cặp khóa thực tế.

**User Data:** Tham số user_data chứa tập lệnh sẽ được thực thi khi phiên bản được khởi chạy. Tập lệnh này cập nhật các gói hệ thống, cài đặt Jenkins (một máy chủ tự động hóa phổ biến) và khởi động dịch vụ Jenkins. Nó cũng cài đặt Java 17 bằng bản phân phối Amazon Corretto.


Nhìn chung, khối mã này thiết lập một phiên bản AWS EC2 (máy chủ pháo đài) với các cấu hình và cài đặt phần mềm cần thiết bằng Terraform. Đó là một phần của quy trình cung cấp cơ sở hạ tầng do Terraform quản lý.

Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:

![Ec2 Bastion Host](/images/2.2/ec2_basion_host.png?featherlight=false&width=100pc)
![Ec2 Bastion Host Details](/images/2.2/ec2_basion_host2.png?featherlight=false&width=100pc)
### Tạo file 07_nat.tf
Tiếp theo, chúng ta cần tạo NAT Gateway để instance ec2 có thể kết nối qua internet.

      resource "aws_eip" "nat-eip" {
      vpc = true
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      resource "aws_nat_gateway" "nat-gw" {
      allocation_id = aws_eip.nat-eip.id
      subnet_id     = aws_subnet.public-subnet-1a.id
      depends_on = [aws_internet_gateway.gw]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

Elastic IP Resource:

      resource "aws_eip" "nat-eip" {
      vpc = true
      tags = {
      name        = "terraform project"
      description = "managed by terraform provisioning"
      }
      }

Khối này xác định tài nguyên IP đàn hồi (EIP) của AWS có tên là "nat-eip". Dòng vpc = true chỉ định rằng EIP này sẽ được liên kết với VPC (Đám mây riêng ảo). Thẻ cũng được gán cho EIP nhằm mục đích nhận dạng và quản lý.

NAT Gateway Resource:
      
      resource "aws_nat_gateway" "nat-gw" {
      allocation_id = aws_eip.nat-eip.id
      subnet_id     = aws_subnet.public-subnet-1a.id
      depends_on    = [aws_internet_gateway.gw]
      tags = {
      name        = "terraform project"
      description = "managed by terraform provisioning"
      }
      }

Khối này xác định tài nguyên Cổng NAT AWS có tên là "nat-gw". Tham số phân bổ_id chỉ định ID phân bổ IP đàn hồi được liên kết với Cổng NAT, được lấy từ tài nguyên IP đàn hồi được xác định trước đó (aws_eip.nat-eip.id). Tham số subnet_id chỉ định mạng con trong đó Cổng NAT sẽ được triển khai. Tham số phụ thuộc_on đảm bảo rằng Cổng NAT chỉ được tạo sau khi cổng internet liên kết (aws_internet_gateway.gw) được tạo. Thẻ cũng được áp dụng cho NAT Gateway nhằm mục đích nhận dạng và quản lý.

Nhìn chung, khối mã này cung cấp IP đàn hồi và Cổng NAT trong môi trường AWS sử dụng Terraform. Cổng NAT tạo điều kiện truy cập internet bên ngoài cho các tài nguyên trong mạng con riêng tư trong VPC.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:

![Elastic IP Address ](/images/2.2/eip1.png?featherlight=false&width=100pc)
![Nat Gateway  ](/images/2.2/eip2.png?featherlight=false&width=100pc)
## Create 08_iam-role.tf

      ### Khối này xác định vai trò IAM có tên "eks_role" cho cụm EKS. Nó chỉ định chính sách cho phép người đứng đầu dịch vụ EKS (eks.amazonaws.com) đảm nhận vai trò này.      
      resource "aws_iam_role" "role-eks" {
      name = "eks_role"
      
      assume_role_policy = jsonencode({
      "Version": "2012-10-17",
      "Statement": [
      {
      "Effect": "Allow",
      "Principal": {
      "Service": [
      "eks.amazonaws.com"
      ]
      },
      "Action": "sts:AssumeRole"
      }
      ]
      })
      }
      # Khối này gắn chính sách IAM "AmazonEKSClusterPolicy" vào vai trò IAM được tạo cho cụm EKS. Chính sách này cấp các quyền cần thiết để quản lý tài nguyên cụm EKS.      
      resource "aws_iam_role_policy_attachment" "eks_attach" {
      role       = aws_iam_role.role-eks.name
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
      
      }
      # Khối này xác định vai trò IAM có tên là "eks-node-group-nodes" cho các nút công nhân EKS. Nó chỉ định chính sách cho phép các phiên bản EC2 (ec2.amazonaws.com) đảm nhận vai trò này.      
      resource "aws_iam_role" "nodes" {
      name = "eks-node-group-nodes"
      
      assume_role_policy = jsonencode({
      Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
      Service = "ec2.amazonaws.com"
      }
      }]
      Version = "2012-10-17"
      })
      
      }

      #Các khối này đính kèm các chính sách (AmazonEKSWorkerNodePolicy, AmazonEKS_CNI_Policy, AmazonEC2ContainerRegistryReadOnly) vào vai trò IAM được tạo cho nút nhân viên EKS. Các chính sách này cấp các quyền cần thiết cho hoạt động của nút EKS, kết nối mạng và truy cập Amazon ECR (Elastic Container Register) để lưu trữ hình ảnh Docker.      
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEKSWorkerNodePolicy" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
      role       = aws_iam_role.nodes.name
      }
      
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEKS_CNI_Policy" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
      role       = aws_iam_role.nodes.name
      
      }
      
      resource "aws_iam_role_policy_attachment" "nodes-AmazonEC2ContainerRegistryReadOnly" {
      policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
      role       = aws_iam_role.nodes.name
      }
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:

![IAM Dashboard](/images/2.2/dashboard-iam.png?featherlight=false&width=100pc)
![Roles List Dashboard](/images/2.2/role-lists.png?featherlight=false&width=100pc)
![Policy  Dashboard](/images/2.2/role-lists2.png?featherlight=false&width=100pc)
![Policy Dashboard](/images/2.2/policy.png?featherlight=false&width=100pc)
## Create 09_eks-cluster.tf
      #Khối này xác định tài nguyên cụm AWS EKS có tên "eks_cluster".      
      resource "aws_eks_cluster" "eks_cluster" {
      #Chỉ định tên của cụm EKS, trong trường hợp này là "Cụm Eks".
      name     = "Eks-cluster"
      #Chỉ định ARN (Tên tài nguyên Amazon) của vai trò IAM được cụm EKS sử dụng. Nó tham chiếu đến vai trò IAM được xác định trước đó (aws_iam_role.role-eks) và truy cập ARN của nó.
      role_arn = aws_iam_role.role-eks.arn
      #Chỉ định cấu hình VPC cho cụm EKS. Nó xác định các mạng con trong đó tài nguyên của cụm EKS sẽ được khởi chạy. Tham số subnet_ids chứa danh sách ID mạng con riêng nơi các nút công nhân của cụm EKS sẽ cư trú.
      vpc_config {
      subnet_ids = [
      aws_subnet.private-subnet-1a.id,
      aws_subnet.private-subnet-1b.id,
      aws_subnet.private-subnet-1c.id
      ]
      }
      #Chỉ định rằng việc tạo tài nguyên cụm EKS phụ thuộc vào việc đính kèm thành công chính sách IAM được xác định trước đó (aws_iam_role_policy_attachment.eks_attach). Điều này đảm bảo có sẵn các quyền cần thiết trước khi thử tạo cụm EKS.
      depends_on = [aws_iam_role_policy_attachment.eks_attach]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![EKS Cluster](/images/2.2/eks.png?featherlight=false&width=100pc)
![EKS Cluster Detail](/images/2.2/Eks-detail.png?featherlight=false&width=100pc)
## Create 10_eks-nodes.tf
      ## Khối này xác định tài nguyên nhóm nút AWS EKS với tên "các nút riêng tư".
      resource "aws_eks_node_group" "private-nodes" {
      ## Chỉ định tên của cụm EKS mà nhóm nút này thuộc về. Nó tham chiếu đến tên của cụm EKS được tạo trước đó (aws_eks_cluster.eks_cluster.name).
      cluster_name    = aws_eks_cluster.eks_cluster.name
      ## Chỉ định tên của nhóm nút EKS.
      node_group_name = "private-nodes"
      ## Chỉ định ARN (Tên tài nguyên Amazon) của vai trò IAM được nhóm nút EKS sử dụng. Nó tham chiếu vai trò IAM được xác định trước đó (aws_iam_role.nodes.arn). Chỉ định ARN (Tên tài nguyên Amazon) của vai trò IAM được nhóm nút EKS sử dụng. Nó tham chiếu vai trò IAM được xác định trước đó (aws_iam_role.nodes.arn).
      node_role_arn   = aws_iam_role.nodes.arn
      ## Chỉ định loại Hình ảnh máy Amazon (AMI) cho các nút. Trong trường hợp này, nó được đặt thành "AL2_x86_64" cho biết Amazon Linux 2.
      ami_type = "AL2_x86_64"
      ## Chỉ định ID của các mạng con trong đó các phiên bản EC2 (nút) của nhóm nút này sẽ được lưu trữ
      subnet_ids = [
      aws_subnet.private-subnet-1a.id,
      aws_subnet.private-subnet-1b.id,
      aws_subnet.private-subnet-1c.id,
      ]
      
      capacity_type  = "SPOT"
      ## Xác định loại price cho phiên bản (trong trường hợp này là phiên bản "SPOT"), cũng như loại phiên bản sẽ được sử dụng (ở đây là "t3.medium").
      instance_types = ["t3.medium"]
      ##Chỉ định cấu hình chia tỷ lệ cho nhóm nút, bao gồm số lượng nút mong muốn, tối đa và tối thiểu.
      scaling_config {
      desired_size = 1
      max_size     = 5
      min_size     = 0
      }
      ## Định cấu hình quyền truy cập từ xa vào các nút bằng khóa SSH (ec2_ssh_key) và chỉ định nhóm bảo mật để áp dụng cho các nút.
      remote_access {
      ec2_ssh_key = var.ssh_access_key
      source_security_group_ids = [aws_security_group.sg_private_eks_node.id]
      }
      update_config {
      max_unavailable = 1
      }
      
      labels = {
      role = "Eks-cluster"
      }
      ## Chỉ định rằng việc tạo tài nguyên nhóm nút EKS phụ thuộc vào việc đính kèm thành công các chính sách IAM được xác định trước đó.
      depends_on = [
      aws_iam_role_policy_attachment.nodes-AmazonEKSWorkerNodePolicy,
      aws_iam_role_policy_attachment.nodes-AmazonEKS_CNI_Policy,
      aws_iam_role_policy_attachment.nodes-AmazonEC2ContainerRegistryReadOnly,
      ]
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![EKS Private Node ](/images/2.2/eks-private-node.png?featherlight=false&width=100pc)
## Create 11_registry.tf
      ## khối mã này cung cấp một kho lưu trữ Amazon ECR có tên là "eks-project" với các cấu hình được chỉ định như tên và thẻ bằng cách sử dụng Terraform. Kho lưu trữ này có thể được sử dụng để lưu trữ hình ảnh Docker và quản lý các ứng dụng được đóng gói trong cơ sở hạ tầng AWS của bạn.
      resource "aws_ecr_repository" "ecr_repository" {
      name = "eks-project"
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/ecr.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/ecr2.png?featherlight=false&width=100pc)
## Create 12_database.tf
      resource "aws_rds_cluster" "aurora-postgresql-cluster" {
      cluster_identifier = "aurora-postgresql-cluster"
      engine             = "aurora-postgresql"
      engine_mode        = "provisioned"
      engine_version     = "13.7"
      database_name      = "test"
      master_username    = "test"
      master_password    = "sadalkdakl232"
      vpc_security_group_ids = [aws_security_group.rds_cluster_sg.id] # Replace with your security group IDs
      db_subnet_group_name   = aws_db_subnet_group.subnet_group.name
      storage_encrypted  = true
      skip_final_snapshot = true
      serverlessv2_scaling_configuration {
      max_capacity = 1.0
      min_capacity = 0.5
      }
      }
      
      resource "aws_rds_cluster_instance" "aurora-postgresql-cluster" {
      for_each = var.az
      identifier = "instance-${each.value}"
      cluster_identifier = aws_rds_cluster.aurora-postgresql-cluster.id
      instance_class     = "db.serverless"
      engine             = aws_rds_cluster.aurora-postgresql-cluster.engine
      engine_version     = aws_rds_cluster.aurora-postgresql-cluster.engine_version
      availability_zone = each.value
      }

Khối mã này cung cấp một cụm cơ sở dữ liệu Amazon Aurora PostgreSQL với cấu hình mở rộng quy mô v2 serverless, bao gồm cấu hình cụm và cấu hình phiên bản, sử dụng Terraform.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/sqs1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/ecr2.png?featherlight=false&width=100pc)
## Create 13_caching.tf
      resource "aws_elasticache_replication_group" "redis" {
      replication_group_id          = "redis-cluster"
      description = "redis replication"
      node_type                     = "cache.t3.micro"
      num_cache_clusters         = 1
      engine                        = "redis"
      engine_version                = "6.0"
      automatic_failover_enabled = false
      parameter_group_name          = "default.redis6.x"
      subnet_group_name             = aws_elasticache_subnet_group.redis_subnet_group.name
      security_group_ids            = [aws_security_group.redis_sg.id]
      auto_minor_version_upgrade    = true
      #   automatic_failover_enabled    = true
      at_rest_encryption_enabled    = false
      transit_encryption_enabled    = false
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      depends_on = [
      aws_eks_cluster.eks_cluster,
      aws_eks_node_group.private-nodes
      ]
      }
Khối mã này cung cấp nhóm sao chép Amazon ElastiCache Redis với các cấu hình được chỉ định bằng cách sử dụng Terraform, đảm bảo đáp ứng các phần phụ thuộc với cụm nút và nhóm nút EKS.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/redis1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/redis2.png?featherlight=false&width=100pc)
## Create 14_sqs..tf
      resource "aws_sqs_queue" "sqs_iaac" {
      name = "sqs_queue"
      delay_seconds = 9
      max_message_size = 2048
      message_retention_seconds = 86400
      receive_wait_time_seconds = 10
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

Khối mã này cung cấp hàng đợi Amazon SQS với các cấu hình được chỉ định như tên, số giây trễ, kích thước tin nhắn tối đa, số giây lưu giữ tin nhắn, số giây thời gian chờ nhận và các thẻ sử dụng Terraform. Hàng đợi này có thể được sử dụng để tách các thành phần của ứng dụng phân tán bằng cách cho phép giao tiếp không đồng bộ giữa chúng.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/sqs1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/sqs2.png?featherlight=false&width=100pc)
##  Create 15_s3.tf

      resource "aws_s3_bucket" "eks-project-front-end-source" {
      bucket = "eks-project-front-end-source"
      }
      resource "aws_s3_bucket" "eks-project-bucket-data" {
      bucket = "eks-project-bucket-data"
      }
Có các khối mã cung cấp hai nhóm Amazon S3 riêng biệt với các tên cụ thể bằng cách sử dụng Terraform. Bộ chứa S3 được sử dụng để lưu trữ và quản lý các đối tượng (chẳng hạn như tệp và dữ liệu) trên đám mây AWS. Mỗi nhóm có thể được xác định duy nhất bằng tên của nó và có thể được định cấu hình với nhiều cài đặt và quyền khác nhau nếu cần cho các trường hợp sử dụng cụ thể.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/s3.png?featherlight=false&width=100pc)
## Create 16_alb.tf
      
      resource "aws_lb" "alb" {
      name               = "application-loadbalancer"
      internal           = true
      load_balancer_type = "application"
      security_groups    = [aws_security_group.alb_sg.id]
      subnets            = [aws_subnet.public-subnet-1a.id,aws_subnet.private-subnet-1b.id,aws_subnet.private-subnet-1c.id,]
      enable_deletion_protection = false
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
Khối mã này cung cấp Cân bằng tải ứng dụng AWS với các cấu hình được chỉ định như tên, cài đặt nội bộ, loại, nhóm bảo mật, mạng con, bảo vệ chống xóa và thẻ sử dụng Terraform. ALB phân phối lưu lượng ứng dụng đến trên nhiều mục tiêu, chẳng hạn như phiên bản EC2, trong nhiều vùng sẵn sàng để đảm bảo tính sẵn sàng cao và khả năng chịu lỗi cho ứng dụng của bạn.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![ECR ](/images/2.2/Alb1.png?featherlight=false&width=100pc)
![ECR ](/images/2.2/Alb2.png?featherlight=false&width=100pc)
## Create 17_cloudfront.tf
      resource "aws_cloudfront_origin_access_identity" "cloudfront_s3" {
      comment = "Cloudfront distribution"
      }
      resource "aws_cloudfront_distribution" "cloudfront_s3_distribution" {
      origin {
      domain_name = aws_s3_bucket.eks-project-front-end-source.bucket_domain_name
      origin_id   = aws_s3_bucket.eks-project-front-end-source.id
      s3_origin_config {
      origin_access_identity = aws_cloudfront_origin_access_identity.cloudfront_s3.cloudfront_access_identity_path
      }
      }
      origin {
      domain_name = aws_lb.alb.dns_name
      origin_id   = aws_lb.alb.id
      custom_origin_config {
      http_port              = 80
      https_port             = 443
      origin_protocol_policy = "https-only"
      origin_ssl_protocols   = ["TLSv1.2"]
      }
      }
      enabled             = true
      default_root_object = "index.html"
      default_cache_behavior {
      allowed_methods  = ["GET", "HEAD"]
      cached_methods   = ["GET", "HEAD"]
      target_origin_id = aws_s3_bucket.eks-project-front-end-source.id
      forwarded_values {
      query_string = true
      cookies {
      forward = "none"
      }
      }
      viewer_protocol_policy = "https-only"
      min_ttl                = 0
      default_ttl            = 3600
      max_ttl                = 86400
      }
      price_class = "PriceClass_100"
      restrictions {
      geo_restriction {
      restriction_type = "none"
      }
      }
      
      viewer_certificate {
      cloudfront_default_certificate = true
      }
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }

Khối mã này cung cấp bản phân phối AWS CloudFront với các cấu hình được chỉ định như nguồn gốc, hành vi bộ đệm, loại giá, hạn chế và thẻ sử dụng Terraform. Phân phối này có thể được sử dụng để lưu trữ và phân phối nội dung trên toàn cầu cho người dùng, cải thiện hiệu suất và tính khả dụng của các ứng dụng và nội dung web.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![CloudFront ](/images/2.2/CloudFront.png?featherlight=false&width=100pc)
![CloudFront ](/images/2.2/CloudFront2.png?featherlight=false&width=100pc)
## Create 18_waf.tf
      resource "aws_wafv2_web_acl" "WafWebAcl" {
      name  = "wafv2-web-acl"
      scope = "REGIONAL"
      
      default_action {
      allow {
      }
      }
      
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "WAF_Common_Protections"
      sampled_requests_enabled   = true
      }
      
      rule {
      name     = "AWS-AWSManagedRulesCommonRuleSet"
      priority = 0
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesCommonRuleSet"
      vendor_name = "AWS"
      
            }
          }
          visibility_config {
            cloudwatch_metrics_enabled = true
            metric_name                = "AWS-AWSManagedRulesCommonRuleSet"
            sampled_requests_enabled   = true
          }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesLinuxRuleSet"
      priority = 1
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesLinuxRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesLinuxRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesAmazonIpReputationList"
      priority = 2
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesAmazonIpReputationList"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesAmazonIpReputationList"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesAnonymousIpList"
      priority = 3
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesAnonymousIpList"
      vendor_name = "AWS"
      
            }
          }
          visibility_config {
            cloudwatch_metrics_enabled = true
            metric_name                = "AWS-AWSManagedRulesAnonymousIpList"
            sampled_requests_enabled   = true
          }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesKnownBadInputsRuleSet"
      priority = 4
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesKnownBadInputsRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesKnownBadInputsRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesUnixRuleSet"
      priority = 5
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesUnixRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesUnixRuleSet"
      sampled_requests_enabled   = true
      }
      }
      
      rule {
      name     = "AWS-AWSManagedRulesWindowsRuleSet"
      priority = 6
      override_action {
      none {
      }
      }
      statement {
      managed_rule_group_statement {
      name        = "AWSManagedRulesWindowsRuleSet"
      vendor_name = "AWS"
      }
      }
      visibility_config {
      cloudwatch_metrics_enabled = true
      metric_name                = "AWS-AWSManagedRulesWindowsRuleSet"
      sampled_requests_enabled   = true
      }
      }
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      
      resource "aws_cloudwatch_log_group" "WafWebAclLoggroup" {
      name              = "aws-waf-logs-wafv2-web-acl"
      retention_in_days = 30
      tags = {
      name="terraform project"
      description = "managed by terraform provisioning"
      }
      }
      
      resource "aws_wafv2_web_acl_logging_configuration" "WafWebAclLogging" {
      log_destination_configs = [aws_cloudwatch_log_group.WafWebAclLoggroup.arn]
      resource_arn            = aws_wafv2_web_acl.WafWebAcl.arn
      depends_on = [
      aws_wafv2_web_acl.WafWebAcl,
      aws_cloudwatch_log_group.WafWebAclLoggroup
      ]
      
      }
      
      resource "aws_wafv2_web_acl_association" "WafWebAclAssociation" {
      resource_arn = aws_lb.alb.arn
      web_acl_arn  = aws_wafv2_web_acl.WafWebAcl.arn
      depends_on = [
      aws_wafv2_web_acl.WafWebAcl,
      aws_cloudwatch_log_group.WafWebAclLoggroup
      ]
      }
Khối mã này cung cấp tài nguyên AWS WAFv2 bao gồm ACL web, cấu hình ghi nhật ký và liên kết với tính năng ghi nhật ký CloudWatch liên quan để giám sát và ghi nhật ký hoạt động ACL web, từ đó nâng cao trạng thái bảo mật của các ứng dụng web được triển khai phía sau Cân bằng tải ứng dụng.
Sau khi tài nguyên được tạo bởi terraform , chúng ta sẽ có kết quả như bên dưới:
![CloudFront ](/images/2.2/Waf1.png?featherlight=false&width=100pc)
![CloudFront ](/images/2.2/Waf2.png?featherlight=false&width=100pc)





