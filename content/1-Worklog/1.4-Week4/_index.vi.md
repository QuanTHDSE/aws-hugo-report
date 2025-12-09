---
title: "Worklog Tuần 4"
date: "2025-11-30"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Nắm vững AWS storage services và data management.
* Hiểu backup và disaster recovery strategies.
* Tìm hiểu data migration và archival solutions.
* Triển khai cost optimization cho storage.
* Thiết kế scalable và resilient storage architecture.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                 | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập kết quả tuần 3 và EC2 infrastructure <br> - Chuẩn bị storage environment cho hands-on labs                                                                                                                                                                                                                                                       | 08/10/2025   | 08/10/2025      | Ghi chép Tuần 3                           |
| 2   | - Tìm hiểu AWS Storage Services: <br>&emsp; + S3 storage classes (Standard, Intelligent-Tiering, Glacier) <br>&emsp; + Access points và storage class analysis <br>&emsp; + S3 performance optimization <br>&emsp; + Object lifecycle management <br>&emsp; + Storage cost analysis                                                                       | 09/10/2025   | 09/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu S3 Advanced Features: <br>&emsp; + Static website hosting & CORS <br>&emsp; + Control access (bucket policies, ACLs) <br>&emsp; + Object key naming cho performance <br>&emsp; + Versioning và MFA delete <br>&emsp; + Glacier cho long-term archival <br>&emsp; + Cross-region replication                                                    | 10/10/2025   | 10/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Storage Gateway và Backup: <br>&emsp; + AWS Storage Gateway concepts <br>&emsp; + Storage Gateway types (S3, EBS, Tape) <br>&emsp; + Backup strategies và snapshots <br>&emsp; + AWS Backup service <br>&emsp; + Disaster recovery planning <br>&emsp; + RTO và RPO metrics                                                                    | 11/10/2025   | 11/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Bài thực hành 1 - S3 Storage Classes & Lifecycle:** <br>&emsp; + Tạo S3 bucket với versioning <br>&emsp; + Cấu hình intelligent-tiering <br>&emsp; + Thiết lập lifecycle policies (move to Glacier sau 90 ngày) <br>&emsp; + Bật storage class analysis <br>&emsp; + Phân tích cost savings <br>&emsp; + Test object retrieval từ các tiers khác nhau | 12/10/2025   | 12/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Bài thực hành 2 - S3 Static Website & CORS:** <br>&emsp; + Tạo S3 bucket cho static website <br>&emsp; + Upload HTML/CSS/JS files <br>&emsp; + Bật static website hosting <br>&emsp; + Cấu hình CORS policy <br>&emsp; + Thiết lập bucket policy cho public read <br>&emsp; + Test website access và cross-origin requests                            | 13/10/2025   | 13/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Bài thực hành 3 - Backup & Disaster Recovery:** <br>&emsp; + Tạo EBS snapshots manually <br>&emsp; + Thiết lập automated snapshot schedules <br>&emsp; + Bật cross-region snapshot copy <br>&emsp; + Tạo AWS Backup plan <br>&emsp; + Test restore từ backup <br>&emsp; + Ghi chép RTO/RPO targets <br>&emsp; + Thiết lập backup notifications        | 14/10/2025   | 14/10/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

## Kết Quả Đạt Được Tuần 4:

### 1. AWS Storage Services Fundamentals

* **S3 Storage Classes:**
  - Standard: dữ liệu được truy cập thường xuyên, latency thấp nhất
  - Intelligent-Tiering: tối ưu hóa chi phí tự động dựa trên access patterns
  - Standard-IA (Infrequent Access): chi phí thấp hơn cho truy cập ít thường xuyên
  - One Zone-IA: lưu trữ single AZ cho dữ liệu không quan trọng
  - Glacier Instant Retrieval: archival với millisecond retrieval
  - Glacier Flexible Retrieval: archival với hours retrieval time
  - Deep Archive: chi phí thấp nhất cho long-term compliance storage

* **Access Points:**
  - Đơn giản hóa truy cập vào S3 buckets cho các ứng dụng khác nhau
  - Mỗi access point có tên DNS riêng
  - Có thể cấu hình different policies cho mỗi access point
  - Multi-region access points cho global distribution

* **Storage Class Analysis:**
  - CloudWatch metrics cho storage costs
  - Xác định objects eligible cho transition
  - Tối ưu hóa lựa chọn storage class
  - Cost prediction và reporting

### 2. S3 Advanced Features

* **Static Website Hosting:**
  - Host static websites (HTML, CSS, JavaScript) trên S3
  - Cấu hình index và error documents
  - Bật website endpoint access
  - Serve content globally via CloudFront

* **CORS (Cross-Origin Resource Sharing):**
  - Cho phép browsers yêu cầu resources từ different origins
  - Cấu hình allowed origins, methods, headers
  - Cần thiết cho JavaScript APIs accessing S3 từ web applications
  - Security policy để ngăn chặn unauthorized cross-origin access

* **Control Access:**
  - Bucket policies: JSON-based access control
  - Object ACLs: granular per-object permissions
  - Public read access cho websites
  - Block Public Access settings cho security

* **Object Key Naming:**
  - Prefixes cho logical organization
  - Random prefixes cải thiện performance (sharding)
  - Tránh sequential prefixes tạo hot partitions
  - Key naming strategy ảnh hưởng đến request rates

* **Versioning & MFA Delete:**
  - Duy trì nhiều versions của objects
  - Khôi phục từ accidental deletion
  - MFA delete protection cho critical objects
  - Version ID cần thiết để xoá specific versions

* **Glacier Archive:**
  - Long-term archival (7+ năm compliance)
  - Chi phí significantly thấp hơn standard storage
  - Retrieval times từ minutes đến hours
  - Phù hợp cho compliance và historical data

* **Cross-Region Replication:**
  - Automatic replication sang secondary region
  - Disaster recovery và business continuity
  - Different storage classes trong replicas
  - Replication cho compliance requirements

### 3. Storage Gateway & Backup Solutions

* **AWS Storage Gateway:**
  - Hybrid cloud storage solution
  - Kết nối on-premises data với AWS

  **Storage Gateway Types:**
  - S3 File Gateway: NFS/SMB access đến S3 objects
  - EBS Volume Gateway: iSCSI block storage backed by EBS
  - Tape Gateway: virtual tape library cho backup

* **Backup Strategies:**
  - Snapshots: point-in-time copies của volumes
  - Incremental snapshots: chỉ changed data
  - Cross-region snapshot copy cho DR
  - Snapshot lifecycle policies

* **AWS Backup Service:**
  - Centralized backup management
  - Support cho EC2, EBS, RDS, Aurora, DynamoDB, EFS, FSx
  - Backup policies và schedules
  - Cross-account và cross-region backup
  - On-demand và scheduled backups

* **Disaster Recovery Planning:**
  - **RTO (Recovery Time Objective):** maximum acceptable downtime
  - **RPO (Recovery Point Objective):** maximum acceptable data loss
  - Backup frequency dựa trên RPO requirements
  - Automated failover mechanisms
  - Regular disaster recovery testing

### 4. Cost Optimization for Storage

* **Lifecycle Policies:**
  - Tự động transition objects sang cheaper storage classes
  - Schedule deletion của old data
  - Kết hợp với Intelligent-Tiering
  - Giảm storage costs theo thời gian

* **S3 Intelligent-Tiering:**
  - Tự động cost optimization
  - Không retrieval fees trong frequent access tier
  - Seamless transition giữa các tiers
  - Monitoring và alerts

* **Cost Analysis:**
  - CloudWatch metrics tracking
  - S3 Storage Lens cho comprehensive analytics
  - Cost allocation tags
  - Storage class analysis recommendations

### 5. Scalable & Resilient Storage Architecture

* **Multi-Region Architecture:**
  - Primary region cho active workloads
  - Secondary region cho disaster recovery
  - Cross-region replication cho data durability
  - Route 53 failover cho application access

* **High Availability:**
  - Multiple AZ deployment
  - Automatic snapshots across AZs
  - Cross-region backup copies
  - Redundancy tại storage và application level

* **Security & Compliance:**
  - Encryption at rest và in transit
  - Versioning cho data protection
  - Bucket policies và access logging
  - Compliance với regulatory requirements

---

## Các Dịch Vụ AWS Học Được:

### Storage Services
* **S3 (Simple Storage Service):** Object storage với multiple tiers
* **Storage Gateway:** Hybrid cloud storage solution
* **Backup:** Centralized backup và recovery service

### Archival & Disaster Recovery
* **Glacier:** Long-term archival storage
* **EBS Snapshots:** Point-in-time volume copies
* **Cross-Region Replication:** Data replication cho DR

---

## Các Lệnh & Tài Nguyên Chính:

**S3 Lifecycle & Tiering CLI Commands:**
```bash
# Tạo lifecycle policy
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration file://lifecycle.json

# Bật Intelligent-Tiering
aws s3api put-bucket-intelligent-tiering-configuration \
  --bucket my-bucket \
  --id auto-tiering \
  --intelligent-tiering-configuration file://tiering.json

# Get storage metrics
aws s3api list-bucket-metrics-configurations --bucket my-bucket

# Bật versioning
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled

# List object versions
aws s3api list-object-versions --bucket my-bucket
```

**S3 Static Website & CORS CLI Commands:**
```bash
# Bật static website hosting
aws s3api put-bucket-website \
  --bucket my-bucket \
  --website-configuration file://website.json

# Put bucket policy cho public read
aws s3api put-bucket-policy \
  --bucket my-bucket \
  --policy file://policy.json

# Bật CORS
aws s3api put-bucket-cors \
  --bucket my-bucket \
  --cors-configuration file://cors.json

# Upload website files
aws s3 cp index.html s3://my-bucket/
aws s3 cp . s3://my-bucket/ --recursive
```

**Backup & Snapshot CLI Commands:**
```bash
# Tạo EBS snapshot
aws ec2 create-snapshot \
  --volume-id vol-xxxxx \
  --description "Database backup"

# Tạo snapshot lifecycle policy
aws dlm create-lifecycle-policy \
  --execution-role-arn arn:aws:iam::123456789012:role/DLMRole \
  --description "Daily snapshots" \
  --state ENABLED \
  --policy-details file://policy.json

# Copy snapshot sang region khác
aws ec2 copy-snapshot \
  --source-region us-east-1 \
  --source-snapshot-id snap-xxxxx \
  --destination-region us-west-2

# Tạo AWS Backup plan
aws backup create-backup-plan \
  --backup-plan file://backup-plan.json

# Tạo backup vault
aws backup create-backup-vault \
  --backup-vault-name my-vault \
  --encryption-key-arn arn:aws:kms:us-east-1:123456789012:key/xxxxx
```

---

## Ví Dụ Cấu Hình:

**Lifecycle Policy (lifecycle.json):**
```json
{
  "Rules": [
    {
      "Id": "Archive old objects",
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        },
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

**CORS Configuration (cors.json):**
```json
{
  "CORSRules": [
    {
      "AllowedOrigins": ["https://example.com"],
      "AllowedMethods": ["GET", "PUT", "POST"],
      "AllowedHeaders": ["*"],
      "MaxAgeSeconds": 3000
    }
  ]
}
```

**Website Configuration (website.json):**
```json
{
  "IndexDocument": {
    "Suffix": "index.html"
  },
  "ErrorDocument": {
    "Key": "error.html"
  }
}
```

---

## Tài Nguyên & Tham Khảo:

* [S3 User Guide](https://docs.aws.amazon.com/s3/)
* [S3 Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
* [Storage Gateway Documentation](https://docs.aws.amazon.com/storagegateway/)
* [AWS Backup User Guide](https://docs.aws.amazon.com/aws-backup/)
* [EBS Snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/EBSSnapshots.html)
* [Disaster Recovery on AWS](https://aws.amazon.com/disaster-recovery/)

---

### Ghi Chú:

* **Tối ưu hóa chi phí:** Sử dụng Intelligent-Tiering và lifecycle policies để giảm storage costs
* **Data Protection:** Bật versioning và cross-region replication cho critical data
* **Compliance:** Archive data sang Glacier cho long-term compliance requirements
* **Performance:** Sử dụng S3 prefixes và access points cho scalable access patterns
* **Testing:** Thường xuyên test disaster recovery procedures và backup restoration