---
title : "Testing Connection"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 2.2.2.5 </b> "
---
## Xác minh kết nối
Tiếp theo, chúng ta cần xác minh kết nối để đảm bảo rằng container (Ứng dụng này được phát triển bởi bất kỳ ngôn ngữ lập trình nào như Java, Python, NodeJS ...) đang chạy hay không,
nó có thể kết nối với cơ sở dữ liệu rds và bộ đệm redis.

Có những danh mục chúng tôi cần xác minh để đảm bảo kết nối luôn khả dụng:

1. Kết nối SSH từ Internet tới máy chủ Bastion
2. **Bastion Host** có thể kết nối với **Cơ sở dữ liệu RDS**, **Redis Cache**, **Cụm Kubernetes đàn hồi** từ VPC
3. **Máy chủ pháo đài** có thể SSH tới Nút công nhân từ VPC
4. Worker Node có thể kết nối với  **RDS Database**, **Redis Cache**, **Elastic Kubernetes Cluster**, **Elastic Container Registry** from VPC
## Hands-on
### 1. SSH đến máy chủ Bastion từ Internet

Bước 1: Download cặp key vừa tạo tại `aws-resources/variables.tf`. đây là [ssh_access_key](https://github.com/daotq2000/aws-iaac-terraform/blob/main/aws-resources/variables.tf) variable

Bước 2: Sử dụng câu lệnh sau để ssh tới server

    $ chmod 400 public-bastion-host.pem
    $ ssh -i public-bastion-host.pem ec2-user@{Your Bastion Host IP}
Nếu bạn nhận được kết quả như ảnh bên dưới, bạn đã thành công SSH tới server.
![AWS SSH](/images/2.2/ssh-bastion-host.png?featherlight=false&width=100pc)

### 2. Ping connection từ Bastion Host tới  Rds Database, Redis Cache, EKS cluster
Sau khi ssh thành công SSH tới bastion host, chúng ta cần cài đặt tool [telnet](https://www.ionos.com/digitalguide/server/tools/telnet-commands/) để ping connect tới 3rd service. 
Cài đặt telnet tool tại Bastion Host sử dụng lệnh bên dưới: 

    sudo yum install telnet
Nếu bạn nhận được kết quả như ảnh bên dưới, bạn đã thành công cài đặt telnet tools
![AWS SSH](/images/2.2/installed_telnet.png?featherlight=false&width=100pc)
Nếu bạn không nhận được kết quả như ảnh trên, hãy kiểm tra NAT Gateway, Internet Gateway có thể cấu hình sai khiến EC2 không thể kết nối tới Internet để cài đặt tool.
#### 2.1 Ping connection từ Bastion Host tới RDS Database
Sử dụng lệnh bên dưới

    telnet {host-aurora-postgres} {port-running}

- {host-aurora-postgres} and {port-running} bạn có thể lấy được thông tin này từ Amazon Console giống ảnh bên dưới:
  ![AWS SSH](/images/2.2/auroraping.png?featherlight=false&width=100pc)

If you received result below , you have connected to RDS Database from Bastion host
![AWS SSH](/images/2.2/telnet_ok_aurora.png?featherlight=false&width=100pc)

Or if you received result below, you haven't connected to RDS Database, need to check Security Group, Inbound traffic from Bastion Host to RDS datbase
![AWS SSH](/images/2.2/telnet_failed_aurora.png?featherlight=false&width=100pc)

#### 2.2 Ping connection from Bastion Host to Redis Cache Cluster
Using command below to ping

    telnet {host-redis-cluster} {port-running}

If you received result below , you have connected to Redis Cache from Bastion host
![AWS SSH](/images/2.2/telnet_ok_redis.png?featherlight=false&width=100pc)

#### 2.3 Ping connection from Bastion Host to Elastic Kubernetes Cluster(EKS)
Using command below to ping

    telnet {host-eks-cluster} {port-running}

If you received result below , you have connected to Redis Cache from Bastion host
![AWS SSH](/images/2.2/telnet_ok_eks.png?featherlight=false&width=100pc)

### 3. Bastion Host can SSH to Worker Node from VPC
Now, we need to ssh to Worker Node to monitoring, troubleshoot issue

Step 1: Copy your pem file generate at `1. SSH to Bastion Host from Internet` to Bastion Host , using command below

    vi public-basion-host.pem
Copy pem content file into this and using `Ecs + wq` to save file

Step 2: we need to permission this file for only system can access, using command below

    chmod 400 public-basion-host.pem
Step 3: Test ssh to Worker node using command below

    ssh -i public-basion-host.pem ec2-user@{your-private-worker-node-ip}

- {your-private-worker-node-ip} you can get this value at Amazon Console, why we need get private IP? Because of when provisioning resources with terraform.
  We have configured worker node inside private subnet, mean which can't access by internet. It's internal communication,
  it help connection is safely and more effective because of save money charge for outbound traffic

#### 4. Worker Node can connect to  **RDS Database**, **Redis Cache**, **Elastic Kubernetes Cluster**, **Elastic Container Registry** from VPC
As mentions, all resources need to avoid access from Internet, now we need ping connection from Worker Node to that resources,
It's ensure when new deployment have been deploy to kubernetes, which work fine, no any element loss connection to require resources.

