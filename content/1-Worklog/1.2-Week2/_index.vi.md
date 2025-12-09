---
title: "Worklog Tuần 2"
date: "2025-11-30"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Nâng cao hiểu biết về S3 (Simple Storage Service) và các cơ sở lưu trữ.
* Nắm vững VPC (Virtual Private Cloud) và cấu hình mạng con.
* Tìm hiểu cơ bản về RDS (Relational Database Service).
* Thực hành với S3, VPC, và các cấu hình bảo mật.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập kết quả tuần 1 <br> - Chuẩn bị không gian làm việc cho các bài thực hành tuần 2                                                                                                                                                                                                                                                                                                                                          | 18/09/2025   | 18/09/2025      | Ghi chép Tuần 1                           |
| 2   | - Tìm hiểu S3 cơ bản: <br>&emsp; + Tạo bucket & các quy ước đặt tên <br>&emsp; + Quản lý object (upload, download, delete) <br>&emsp; + Storage classes (Standard, IA, Glacier) <br>&emsp; + Versioning & lifecycle policies <br>&emsp; + Access control (ACLs, Bucket Policies) <br>&emsp; + Hosting trang web tĩnh                                                                                                              | 19/09/2025   | 19/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu VPC cơ bản: <br>&emsp; + Tạo VPC & CIDR blocks <br>&emsp; + Subnets (public & private) <br>&emsp; + Route tables & routing <br>&emsp; + Internet Gateway (IGW) <br>&emsp; + NAT Gateway cho private subnets <br>&emsp; + Network ACLs vs Security Groups                                                                                                                                                               | 20/09/2025   | 20/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu RDS cơ bản: <br>&emsp; + Các engine RDS (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server) <br>&emsp; + Tạo DB instance <br>&emsp; + Multi-AZ deployment <br>&emsp; + Read replicas <br>&emsp; + Cơ chế backup & restore <br>&emsp; + RDS security groups                                                                                                                                                                | 21/09/2025   | 21/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Bài thực hành 1 - S3 & Trang web tĩnh:** <br>&emsp; + Tạo S3 bucket với versioning <br>&emsp; + Upload file và cấu hình bucket policy cho public read <br>&emsp; + Bật static website hosting <br>&emsp; + Test truy cập website qua S3 endpoint <br>&emsp; + Thiết lập lifecycle policy để chuyển các phiên bản cũ sang Glacier                                                                                              | 22/09/2025   | 22/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Bài thực hành 2 - VPC & Networking:** <br>&emsp; + Tạo custom VPC với CIDR 10.0.0.0/16 <br>&emsp; + Tạo public subnet (10.0.1.0/24) và private subnet (10.0.2.0/24) <br>&emsp; + Tạo và attach Internet Gateway <br>&emsp; + Cấu hình route tables cho public/private subnets <br>&emsp; + Launch EC2 trong public subnet & test internet access <br>&emsp; + Launch EC2 trong private subnet & test kết nối qua bastion host | 23/09/2025   | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Bài thực hành 3 - RDS Database:** <br>&emsp; + Tạo RDS MySQL instance trong custom VPC <br>&emsp; + Cấu hình security group cho phép truy cập từ EC2 <br>&emsp; + Kết nối đến RDS từ EC2 instance <br>&emsp; + Tạo database và table mẫu <br>&emsp; + Test backup & restore functionality <br>&emsp; + Bật automated backups                                                                                                  | 24/09/2025   | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 2:

* **Nắm vững S3:**
  * Tạo được nhiều S3 bucket với các cấu hình khác nhau
  * Hiểu các quy ước đặt tên bucket và xem xét vùng địa lý
  * Nắm vững quản lý lifecycle object và versioning
  * Triển khai bucket policies và access control lists
  * Hosting trang web tĩnh sử dụng S3 endpoint
  * Thiết lập CloudFront distribution cho content delivery (tùy chọn)

* **Hiểu VPC & Networking:**
  * Tạo custom VPC với CIDR block planning hợp lý
  * Thiết kế và triển khai kiến trúc public/private subnet
  * Cấu hình Internet Gateway cho routing public subnet
  * Thiết lập NAT Gateway cho internet access từ private subnet
  * Hiểu sự khác biệt giữa Security Groups và Network ACLs
  * Triển khai bastion host pattern cho truy cập private instance

* **Cơ bản RDS Database:**
  * Khởi động RDS MySQL instance với Multi-AZ configuration
  * Cấu hình security groups cho database access
  * Thiết lập kết nối từ EC2 đến RDS
  * Tạo databases và tables qua command-line
  * Hiểu các chiến lược backup và restore procedures
  * Giám sát RDS performance metrics trong CloudWatch

* **Tích hợp & Kiến trúc:**
  * Thiết kế kiến trúc 3-tầng: web tier (public EC2) → database tier (RDS)
  * Triển khai security best practices trên tất cả các tầng
  * Sử dụng AWS CLI để quản lý S3, VPC, và RDS tài nguyên
  * Giám sát chi phí và tối ưu hóa sử dụng tài nguyên

### Các lệnh & tài nguyên chính:

**Lệnh S3 CLI:**
```bash
# Tạo bucket
aws s3 mb s3://my-bucket-name

# Upload files
aws s3 cp file.txt s3://my-bucket-name/
aws s3 cp . s3://my-bucket-name/ --recursive

# Download files
aws s3 cp s3://my-bucket-name/file.txt ./
aws s3 sync s3://my-bucket-name/ ./local-folder/

# Liệt kê buckets & contents
aws s3 ls
aws s3 ls s3://my-bucket-name/

# Bật bucket versioning
aws s3api put-bucket-versioning --bucket my-bucket-name --versioning-configuration Status=Enabled

# Đặt bucket policy
aws s3api put-bucket-policy --bucket my-bucket-name --policy file://policy.json
```

**Lệnh VPC CLI:**
```bash
# Tạo VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Tạo subnets
aws ec2 create-subnet --vpc-id vpc-xxxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a

# Tạo Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxxx --vpc-id vpc-xxxxx

# Tạo route table
aws ec2 create-route-table --vpc-id vpc-xxxxx
aws ec2 create-route --route-table-id rtb-xxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxxx

# Liên kết subnet với route table
aws ec2 associate-route-table --subnet-id subnet-xxxxx --route-table-id rtb-xxxxx
```

**Lệnh RDS CLI:**
```bash
# Tạo RDS instance
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --master-username admin \
  --master-user-password MyPassword123! \
  --allocated-storage 20

# Xem chi tiết RDS instances
aws rds describe-db-instances

# Tạo database snapshot
aws rds create-db-snapshot \
  --db-instance-identifier mydb \
  --db-snapshot-identifier mydb-snapshot

# Restore từ snapshot
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier mydb-restored \
  --db-snapshot-identifier mydb-snapshot
```

**Kết nối Database (từ EC2):**
```bash
# Kết nối RDS MySQL từ EC2
mysql -h mydb.xxxxx.us-east-1.rds.amazonaws.com -u admin -p

# Tạo database
CREATE DATABASE my_app_db;
USE my_app_db;

# Tạo table mẫu
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Thêm dữ liệu mẫu
INSERT INTO users (name, email) VALUES ('Nguyễn Văn A', 'nguyenvana@example.com');
INSERT INTO users (name, email) VALUES ('Trần Thị B', 'tranthib@example.com');

# Truy vấn dữ liệu
SELECT * FROM users;
```

### Ví dụ S3 Bucket Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket-name/*"
    }
  ]
}
```

### Dọn dẹp tuần 2:

* Ghi chép lại tất cả VPC configurations (subnet mappings, route tables)
* Export S3 bucket contents để backup
* Tạo RDS snapshots trước khi dọn dẹp
* Xoá test resources nhưng giữ một instance của mỗi service cho tuần 3
* Kiểm tra chi phí trong AWS Billing Dashboard
* Gắn tag cho tất cả tài nguyên còn lại với Week identifier

### Tài nguyên & Tham khảo:

* [S3 User Guide](https://docs.aws.amazon.com/s3/)
* [VPC User Guide](https://docs.aws.amazon.com/vpc/)
* [RDS User Guide](https://docs.aws.amazon.com/rds/)
* [AWS CLI S3 Commands](https://docs.aws.amazon.com/cli/latest/reference/s3/)
* [AWS CLI VPC Commands](https://docs.aws.amazon.com/cli/latest/reference/ec2/)
* [AWS CLI RDS Commands](https://docs.aws.amazon.com/cli/latest/reference/rds/)

---

### Ghi chú:

* **Kiểm soát chi phí:** S3 storage và RDS instances có thể phát sinh chi phí. Giám sát Free Tier usage cẩn thận.
* **Bảo mật:** Luôn sử dụng security groups để hạn chế database access. Không bao giờ mở cho 0.0.0.0/0 trong production.
* **Kiến trúc:** Các bài thực hành tuần này là nền tảng cho multi-tier applications trong tuần 3.