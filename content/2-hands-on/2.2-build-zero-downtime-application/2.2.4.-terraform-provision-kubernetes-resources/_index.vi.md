---
title : "Quản lý tài nguyên Kubernetes với Terraform"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.4 </b> "
---

## Video
[![How to build realistic application on AWS](/images/2.2/Test.png)](https://youtu.be/XNSiWFjPslg?list=PLk36mRYn9bOHtZsDG3iA-yGzktMiBojm9 "Everything Is AWESOME")
## Overall
Bài viết này chúng ta sẽ cấu hình Helm Chart và Kubernetes Resource cho Cluster, Helm Chart là công cụ mạnh mẽ để quản lý tài nguyên trên Kubernetes. Chúng ta cần biết từng thành phần để sử dụng.

## Helm Chart là gì? Lợi ích chính của chúng
#### Helm Chart là gì
Helm Chart là gói chứa tất cả các tệp và tài nguyên cần thiết để xác định, cài đặt và nâng cấp ngay cả ứng dụng Kubernetes phức tạp nhất. Helm, thường được gọi là trình quản lý gói Kubernetes, sử dụng biểu đồ để đơn giản hóa việc triển khai và quản lý ứng dụng trên cụm Kubernetes.
#### Keys benefits:
1. Đơn giản hóa cấu hình ứng dụng Kubernetes: Biểu đồ Helm giúp kết hợp các tài nguyên Kubernetes lại với nhau thành một đơn vị, tạo ra một cách đơn giản để triển khai và quản lý ứng dụng mà không cần phải xử lý từng tài nguyên riêng biệt.

2. Templatization: Tạo khuôn mẫu: Biểu đồ Helm sử dụng khuôn mẫu, cho phép các bảng kê khai Kubernetes có thể tái sử dụng. Các tham số có thể được chuyển vào biểu đồ Helm để tùy chỉnh việc triển khai cho các môi trường khác nhau, như phát triển, dàn dựng và sản xuất.

3. Kiểm soát và sửa đổi phiên bản: Mỗi lần biểu đồ Helm được đóng gói, nó có thể được tạo phiên bản. Điều này cho phép bạn triển khai và quay lại các phiên bản cụ thể của ứng dụng, mang lại khả năng kiểm soát tốt hơn cho việc triển khai của bạn.

4. Quản lý phụ thuộc: Biểu đồ Helm có thể xác định các phụ thuộc vào các biểu đồ Helm khác hoặc các phiên bản biểu đồ cụ thể. Điều này tự động hóa việc triển khai các dịch vụ khác mà ứng dụng của bạn có thể phụ thuộc vào.

5. Cập nhật dễ dàng: Sau khi cài đặt biểu đồ Helm, việc thay đổi hoặc nâng cấp việc triển khai có thể được thực hiện bằng các lệnh rất đơn giản. Điều này giúp giảm đáng kể độ phức tạp so với các phương pháp cập nhật ứng dụng truyền thống trên Kubernetes.

6. Tài nguyên được chia sẻ: Biểu đồ Helm có thể được chia sẻ trong toàn tổ chức, do đó, các mẫu tương tự để triển khai và quản lý ứng dụng có thể được sử dụng lại, đảm bảo tính nhất quán và tiết kiệm thời gian.

7. Cộng đồng và Hệ sinh thái: Với sự phổ biến của Helm, có một cộng đồng và hệ sinh thái rộng lớn đóng góp vào biểu đồ Helm của họ. Bạn có thể sử dụng các biểu đồ hiện có do cộng đồng cung cấp để không phải viết mọi thứ từ đầu.

8. Quản lý phát hành: Hệ thống quản lý phát hành của Helm cho phép bạn theo dõi và quản lý việc cài đặt biểu đồ của mình trên toàn cụm.

9. Thân thiện với tự động hóa: Helm có thể được tích hợp vào các quy trình CI/CD tự động, giúp đơn giản hóa quá trình phát triển và triển khai liên tục.

10. Khôi phục dễ dàng: Nếu quá trình triển khai không thành công hoặc dẫn đến bản phát hành không ổn định, Helm cho phép dễ dàng khôi phục về phiên bản ổn định trước đó của ứng dụng của bạn.

## Quản lý tài nguyên Kubernetes với Terraform
![Structure Kubernetes resource provisioning by Terraform](/images/2.2/struct1.png?featherlight=false&width=100pc)

Như hình ảnh chúng ta có 2 thư mục
1. Thư mục HelmChart chứa dự án helm áp dụng tài nguyên cho kubernetes
2. Kubernetes là thư mục chứa file terraform để quản lý tài nguyên trên `kubernetes`

#### Ý nghĩa thư mục HelmChart 
HelmChart là thư mục chứa dự án biểu đồ helm mà chúng ta sẽ sử dụng trong labs này, đó là Bộ điều khiển Nginx Ingress, nếu bạn chưa biết [Bộ điều khiển Nginx Ingress là gì](https://kubernetes.io/docs/concepts/services-networking/ingress/), vui lòng theo dõi [tại đây](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- File này cung cấp Ingress Controller cho Kubernetes
  File: https://github.com/daotq2000/aws-iaac-terraform/tree/main/HelmChart/ingress-nginx

      controller:
      replicaCount: 2
      ingressClassByName: true
      ingressClassResource:
      name: "public"
      enabled: true
      default: false
      controllerValue: "k8s.io/public-ingress-nginx"
      ## Name of the ingress class to route through this controller
      ##
      ingressClass: public
      config:
      #Upload
      client-body-buffer-size: 32M
      proxy-body-size: 100G
      proxy-buffering: "off"
      proxy-read-timeout: "600"
      proxy-send-timeout: "600"
      #Logging
      log-format-upstream: '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_length $request_time [$proxy_upstream_name] [$proxy_alternative_upstream_name] $upstream_addr $upstream_response_length $upstream_response_time $upstream_status $req_id $http_x_device_id|$http_x_device_os|$http_x_device_os_version|$http_x_device_locale|$http_x_app_version|$http_x_app_id'
      # Accesslog Request IP
      compute-full-forwarded-for: "true"
      use-forwarded-headers: "true"
      service:
      type: NodePort
      nodePorts:
      http: "30080"
      https: "30443"
      tcp:
      32069: 32069
      32070: 32070
      31280: 31280
      udp: {}
      metrics:
      enabled: false
      serviceMonitor:
      enabled: true
      #tcp:
      #  32069: "external/window-rdp-external:32069"
      #  32070: "external/avnet-poc-external:3393"
      #  31280: "external/squid-external:31280"



Trong thư mục chúng ta chỉ cần một file là `public-values.yaml`, đây là file giá trị biểu đồ, kubernetes sử dụng file này để tạo triển khai cho Nginx-Ingress-Controller

#### Ý nghĩa thư mục Kubernetes
Có nội dung của thư mục, đó là dự án địa hình phụ, chúng tôi sẽ thực thi việc này sau khi tài nguyên tại `aws-resources` đã được hoàn thành.
    .
    ├── deploy.tfplan
    ├── ingress-nginx.tf
    ├── provider.tf
    ├── terraform-apply.sh
    ├── terraform-plan.sh
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    └── variables.tf
**We have complete there files below:**

**1. Tạo file variables.tf**

      variable "eks-context-name" {
      type = string
      default = "arn:aws:eks:us-east-1:846338211683:cluster/Eks-cluster"
      }
- Tệp này là các biến môi trường cấu hình cho terraform, trong tệp cụ thể chúng ta có thể sử dụng cú pháp `var.value` để lấy giá trị từ tệp môi trường

**2. Tạo file provider.tf**

    provider "kubernetes" {
    config_path    = "~/.kube/config"
    config_context = var.eks-context-name
    insecure = true
    }
    provider "helm" {
    kubernetes {
    config_path    = "~/.kube/config"
    config_context = var.eks-context-name
    insecure = true
    }
    }
- `config_path    = "~/.kube/config"` Khối mã đọc cấu hình kubernetes từ thư mục `${HOME}/.kube/config`.
- `config_context = var.eks-context-name` Đọc tên cụm Amazon Kubernetes từ `variables.tf`

**3. Tạo file ingress-nginx.tf**

        resource "kubernetes_namespace" "nginx-ingress" {
        metadata {
        annotations = {
        name:"ingress-nginx-namespace"
        }
        name = "nginx-ingress-namespace"
        }
        lifecycle {
        }
        }
        resource "helm_release" "public-nginx-ingress" {
        name       = "ingress-public"
        chart      = "ingress-nginx"
        repository = "https://kubernetes.github.io/ingress-nginx"
        version    = "4.3.0"
        namespace  = kubernetes_namespace.nginx-ingress.id
        values = [
        file("../HelmChart/ingress-nginx/public-values.yaml")
        ]
        }
**4. Tạo file terraform-plan.sh**

      terraform plan -out deploy.tfplan

Sử dụng lệnh `terraform plan -out deploy.tfplan` để lập kế hoạch thay đổi tài nguyên

**5. Tạo file terraform-apply.sh**

      terraform apply deploy.tfplan
Sử dụng lệnh `terraform apply deploy.tfplan` để áp dụng tài nguyên thay đổi

## Refer to my source code
**Github repository: ** https://github.com/daotq2000/aws-iaac-terraform 