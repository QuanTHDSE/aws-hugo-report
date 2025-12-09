---
title: "Worklog Tuần 1 "
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.
* Thiết lập tài khoản AWS Free Tier và môi trường phát triển cục bộ.
* Thực hành với EC2, S3, và IAM cơ bản.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                             | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập                                                                                                                                                                                                                                                                                       | 11/09/2025   | 11/09/2025      | Hướng dẫn FCJ                             |
| 3   | - Tìm hiểu AWS và các loại dịch vụ <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC, Security Groups) <br>&emsp; + Database (RDS, DynamoDB) <br>&emsp; + Nhận dạng & Truy cập (IAM)                                                                                                                                                     | 12/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account <br> - Bật MFA cho tài khoản root <br> - Tìm hiểu AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp; + Cài AWS CLI v2 & cấu hình <br>&emsp; + Kiểm tra kết nối: `aws sts get-caller-identity`                                                                                                                            | 13/09/2025   | 13/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu IAM cơ bản: <br>&emsp; + Tạo IAM user với quyền hạn tối thiểu <br>&emsp; + Tạo Access Keys <br>&emsp; + Hiểu vai trò vs chính sách quyền <br> - Tìm hiểu EC2 cơ bản: <br>&emsp; + Instance types (t3.micro, t4g.micro) <br>&emsp; + AMI (Amazon Machine Images) <br>&emsp; + EBS volumes & snapshots <br>&emsp; + Security Groups & Key Pairs <br>&emsp; + Elastic IPs     | 14/09/2025   | 15/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Bài thực hành 1 - EC2:** <br>&emsp; + Tạo SSH key pair trong AWS Console <br>&emsp; + Khởi động EC2 instance t3.micro (Linux) <br>&emsp; + Cấu hình Security Group (cho phép port 22 từ IP của bạn) <br>&emsp; + Kết nối SSH từ Windows Terminal hoặc PuTTY <br>&emsp; + Tạo EBS volume, gắn & mount vào EC2 <br>&emsp; + Kiểm tra tính bền dữ liệu bằng cách stop/start instance | 15/09/2025   | 15/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

## Kết Quả Đạt Được Tuần 1:

### 1. Bắt Đầu Hành Trình Lên Mây

* Hiểu được AWS là gì và tại sao nó quan trọng
* Nắm các mô hình triển khai cloud (Public, Private, Hybrid)
* Xác định các lợi thế của cloud computing so với on-premise
* Hiểu được các trường hợp sử dụng thực tế của AWS

### 2. Hạ Tầng Toàn Cầu Của AWS

* Hiểu cấu trúc AWS Global Infrastructure (Regions, Availability Zones, Edge Locations)
* Nắm các tiêu chí chọn Region phù hợp:
  - Độ trễ (Latency)
  - Tuân thủ pháp lý (Compliance)
  - Giá cả
  - Sẵn có dịch vụ
* Hiểu lợi ích của Multi-AZ deployment cho độ sẵn sàng cao
* Khái niệm về Edge Locations và CloudFront

### 3. Công Cụ Quản Lý AWS Services

* **AWS Management Console:**
  - Làm quen giao diện web console
  - Điều hướng và tìm kiếm dịch vụ
  - Hiểu cấu trúc menu chính

* **AWS CLI (Command Line Interface):**
  - Cài đặt AWS CLI v2 trên Windows
  - Cấu hình credentials (`aws configure`)
  - Thực hiện các lệnh cơ bản
  - Hiểu output formats (JSON, text, table)

* **AWS SDKs:**
  - Khái niệm về SDK cho lập trình
  - Các ngôn ngữ được hỗ trợ (Python, Java, Node.js, etc.)

* **Infrastructure as Code (IaC):**
  - Giới thiệu CloudFormation và Terraform
  - Lợi ích của IaC

### 4. Tối Ưu Hóa Chi Phí Trên AWS

* **Mô Hình Định Giá:**
  - Pay-as-you-go
  - Reserved Instances
  - Spot Instances
  - Savings Plans

* **AWS Free Tier:**
  - Các dịch vụ miễn phí trong 12 tháng đầu
  - Always free services
  - Giới hạn free tier

* **Cost Management Tools:**
  - AWS Pricing Calculator
  - AWS Cost Explorer
  - Billing Alerts & CloudWatch

* **Làm Việc Với AWS Support:**
  - Các mức hỗ trợ (Basic, Developer, Business, Enterprise)
  - Cách liên hệ support
  - AWS documentation và resources

### 5. Thực Hành và Nghiên Cứu Bổ Sung

* Tạo AWS Free Tier account
* Bật MFA cho tài khoản root
* Cấu hình AWS CLI và kiểm tra kết nối
* Tạo IAM user với quyền hạn tối thiểu
* Hiểu về Access Keys và Secret Keys
* Làm quen với AWS documentation

---

## Các Dịch Vụ AWS Cơ Bản Học Được:

### Compute (Tính Toán)
* **EC2 (Elastic Compute Cloud):** Virtual machines on-demand
* **Lambda:** Serverless computing

### Storage (Lưu Trữ)
* **S3 (Simple Storage Service):** Object storage
* **EBS (Elastic Block Store):** Block-level storage for EC2

### Networking (Mạng)
* **VPC (Virtual Private Cloud):** Isolated network environment
* **Security Groups:** Virtual firewall

### Database (Cơ Sở Dữ Liệu)
* **RDS (Relational Database Service):** Managed relational databases
* **DynamoDB:** NoSQL database

### Identity & Access (Quản Lý Truy Cập)
* **IAM (Identity and Access Management):** User, role, permission management

---

## Tài Nguyên & Tham Khảo:

* [AWS Getting Started Guide](https://aws.amazon.com/getting-started/)
* [AWS Free Tier Documentation](https://aws.amazon.com/free/)
* [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
* [AWS Pricing](https://aws.amazon.com/pricing/)
* [AWS CLI User Guide](https://docs.aws.amazon.com/cli/)
* [AWS Support](https://aws.amazon.com/support/)

---

### Ghi Chú:

* **Bảo mật:** Không bao giờ commit AWS credentials vào version control
* **Giám sát chi phí:** Kiểm tra billing dashboard thường xuyên
* **Tìm hiểu thêm:** Sử dụng AWS documentation và các tài nguyên miễn phí