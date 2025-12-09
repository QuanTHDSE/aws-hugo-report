---
title: "Worklog Tuần 6"
date: "2025-11-30"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Thiết kế và triển khai kiến trúc ứng dụng 3-tier hoàn chỉnh.
* Tích hợp tất cả AWS services học trong tuần 1-5.
* Triển khai CI/CD pipeline với GitHub, CodePipeline, và CodeBuild.
* Thiết lập comprehensive monitoring và security.
* Triển khai ứng dụng trên containerized infrastructure.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập services tuần 1-5 và kiến trúc <br> - Lập kế hoạch scope và design capstone project <br> - Chuẩn bị GitHub repository và IAM roles                                                                                                                                                  | 22/10/2025   | 22/10/2025      | Ghi chép Tuần 1-5                         |
| 2   | - **Thiết lập Infrastructure:** <br>&emsp; + Tạo VPC với public/private subnets <br>&emsp; + Cấu hình security groups và NACLs <br>&emsp; + Thiết lập NAT Gateway cho private subnets <br>&emsp; + Tạo RDS Multi-AZ database <br>&emsp; + Provisioning S3 buckets với encryption             | 23/10/2025   | 23/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Frontend & CDN Setup:** <br>&emsp; + Tạo S3 bucket cho static website <br>&emsp; + Upload HTML/CSS/JavaScript files <br>&emsp; + Cấu hình CloudFront distribution <br>&emsp; + Thiết lập Route 53 DNS <br>&emsp; + Bật CORS cho API requests                                             | 24/10/2025   | 24/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **CI/CD Pipeline Setup:** <br>&emsp; + Kết nối GitHub repository tới AWS <br>&emsp; + Tạo CodePipeline cho automated deployment <br>&emsp; + Cấu hình CodeBuild cho application build <br>&emsp; + Thiết lập CodeDeploy cho instance deployment <br>&emsp; + Tạo build và deploy artifacts | 25/10/2025   | 25/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Application Layer Deployment:** <br>&emsp; + Tạo Application Load Balancer (ALB) <br>&emsp; + Launch EC2 instances với application <br>&emsp; + Cấu hình auto-scaling groups (min: 2, max: 4) <br>&emsp; + Đăng ký instances với ALB <br>&emsp; + Test application endpoints             | 26/10/2025   | 26/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Monitoring & Logging Setup:** <br>&emsp; + Cấu hình CloudWatch dashboards <br>&emsp; + Thiết lập alarms cho CPU, memory, và errors <br>&emsp; + Bật CloudTrail cho audit logging <br>&emsp; + Cấu hình VPC Flow Logs <br>&emsp; + Thiết lập SNS notifications cho alerts                 | 27/10/2025   | 27/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Testing & Validation:** <br>&emsp; + Thực hiện load testing với Apache Bench <br>&emsp; + Xác minh auto-scaling behavior <br>&emsp; + Test database connectivity và failover <br>&emsp; + Xác minh security configurations <br>&emsp; + Ghi chép lại kiến trúc và deployment procedures  | 28/10/2025   | 28/10/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

## Tổng quan Capstone Project Architecture:

### Thành phần Kiến trúc:

#### 1. **Frontend Layer (CloudFront + S3)**
   - S3 bucket hosting static website (HTML, CSS, JavaScript)
   - CloudFront distribution cho global content delivery
   - Route 53 cho DNS routing
   - CORS cấu hình cho API requests

#### 2. **Application Layer (ALB + EC2)**
   - Application Load Balancer phân phối traffic
   - Auto Scaling Group với 2-4 EC2 instances
   - Target group health checks
   - Application code được deploy qua CodeDeploy

#### 3. **Data Layer (RDS)**
   - MySQL/PostgreSQL RDS instance (Multi-AZ)
   - Automated backups được bật
   - Security group giới hạn access chỉ từ EC2
   - Read replica cho scalability

#### 4. **Storage (S3 + Media Convert)**
   - S3 bucket cho application data và media
   - Server-side encryption được bật
   - CloudFront integration cho media delivery
   - Media conversion jobs cho video processing

#### 5. **CI/CD Pipeline (GitHub + CodePipeline)**
   - GitHub repository cho source control
   - CodePipeline orchestrating deployment
   - CodeBuild cho compiling và testing code
   - CodeDeploy cho instance deployment
   - Automated triggering khi code push

#### 6. **Networking (VPC + Security)**
   - Custom VPC với CIDR 10.0.0.0/16
   - Public subnets (10.0.1.0/24, 10.0.2.0/24) cho ALB
   - Private subnets (10.0.3.0/24, 10.0.4.0/24) cho EC2 và RDS
   - NAT Gateway cho private subnet internet access
   - Security groups cho mỗi tier

#### 7. **Monitoring & Logging**
   - CloudWatch dashboards cho metrics
   - CloudWatch alarms cho auto-scaling triggers
   - CloudTrail cho API audit logging
   - VPC Flow Logs cho network analysis
   - SNS cho alert notifications

#### 8. **Authentication & Access**
   - Cognito cho user authentication (optional)
   - IAM roles cho services
   - Least privilege access policies
   - KMS encryption cho sensitive data

---