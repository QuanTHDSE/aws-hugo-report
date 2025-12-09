---
title: "Worklog Tuần 3"
date: "2025-11-30"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Nắm vững các loại instance types của EC2 và cấu hình.
* Hiểu cách tạo AMI và chiến lược backup.
* Tìm hiểu EBS storage và quản lý snapshots.
* Nắm vững EC2 Auto Scaling và phân phối tải.
* Triển khai instance metadata và user data scripts.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập kết quả tuần 2 và VPC setup <br> - Chuẩn bị EC2 instances cho hands-on labs                                                                                                                                                                                                                             | 25/09/2025   | 25/09/2025      | Ghi chép Tuần 2                           |
| 2   | - Tìm hiểu EC2 Instance Types: <br>&emsp; + General Purpose (t3, m5, m6i) <br>&emsp; + Compute Optimized (c5, c6i) <br>&emsp; + Memory Optimized (r5, r6i, x1) <br>&emsp; + Storage Optimized (i3, i4i) <br>&emsp; + Instance sizing và naming conventions <br>&emsp; + Right-sizing cho workloads               | 26/09/2025   | 26/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu AMI, Backup & Key Pair: <br>&emsp; + Tạo custom AMI từ running instance <br>&emsp; + AMI management và sharing <br>&emsp; + SSH key pair creation và management <br>&emsp; + Best practices cho key storage <br>&emsp; + Disaster recovery với AMI                                                    | 27/09/2025   | 27/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu EBS Storage Management: <br>&emsp; + EBS volume types (gp3, io1/io2, st1, sc1) <br>&emsp; + Create, attach, và mount EBS volumes <br>&emsp; + EBS snapshots và point-in-time recovery <br>&emsp; + Volume encryption và performance tuning <br>&emsp; + Snapshot scheduling và lifecycle              | 28/09/2025   | 28/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu User Data & Instance Metadata: <br>&emsp; + Write user data scripts (Bash cho Linux) <br>&emsp; + Automate instance initialization <br>&emsp; + Query instance metadata (curl 169.254.169.254) <br>&emsp; + Sử dụng metadata cho dynamic configuration <br>&emsp; + CloudWatch integration            | 29/09/2025   | 29/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tìm hiểu EC2 Auto Scaling: <br>&emsp; + Tạo launch templates <br>&emsp; + Tạo Auto Scaling groups <br>&emsp; + Cấu hình scaling policies (target tracking, step, simple) <br>&emsp; + Set min/max/desired capacity <br>&emsp; + Tích hợp với load balancers <br>&emsp; + Lifecycle hooks cho graceful shutdown | 30/09/2025   | 30/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Tìm hiểu EFS, FSx, Lightsail & MGN: <br>&emsp; + Elastic File System (EFS) setup <br>&emsp; + FSx cho Windows/Lustre basics <br>&emsp; + Lightsail như simplified cloud alternative <br>&emsp; + Application Migration Service (MGN) concepts <br>&emsp; + Migration planning và execution                     | 31/09/2025   | 31/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

## Kết Quả Đạt Được Tuần 3:

### 1. EC2 Instance Types

* **Các Loại Instance Family:**
  - General Purpose (t3, m5, m6i) - balanced compute, memory, networking
  - Compute Optimized (c5, c6i) - high-performance processors
  - Memory Optimized (r5, r6i, x1) - large memory workloads
  - Storage Optimized (i3, i4i, h1) - high sequential I/O access
  - Accelerated Computing (p3, g4, f1) - GPU/FPGA workloads

* **Instance Naming Convention:**
  - t3.micro, m5.large, c5.xlarge, r6i.2xlarge
  - Phần đầu = family, phần giữa = generation, phần cuối = size
  - Kích thước lớn hơn = vCPUs, memory, và network performance nhiều hơn

* **Các Cân Nhắc Chính:**
  - Right-sizing instances dựa trên workload requirements
  - Cost optimization thông qua appropriate instance selection
  - Performance vs. cost trade-offs
  - Burstable vs. steady-state performance

### 2. AMI / Backup / Key Pair

* **Amazon Machine Image (AMI):**
  - Template chứa OS, applications, và configurations
  - Public AMIs (AWS-provided, community, marketplace)
  - Custom AMIs được tạo từ running instances
  - AMI bao gồm OS, root volume configuration, và block device mappings

* **Key Pair Management:**
  - SSH key pairs cho Linux instance access
  - RDP password cho Windows instance access
  - Key pairs được lưu trong AWS (download một lần)
  - Khác key pair cho mỗi region/use-case được khuyến nghị
  - Không bao giờ chia sẻ private keys

* **Instance Backup & Recovery:**
  - Tạo AMI từ running instance (bao gồm tất cả volumes)
  - Snapshots cho point-in-time recovery
  - Launch instances mới từ custom AMI
  - Hữu ích cho disaster recovery và instance replication

### 3. Elastic Block Store (EBS)

* **EBS Volume Types:**
  - gp3 (General Purpose) - balanced price/performance cho most workloads
  - io1/io2 (Provisioned IOPS) - high-performance databases, NoSQL
  - st1 (Throughput Optimized) - big data, data warehouses
  - sc1 (Cold HDD) - infrequent access, low-cost storage
  - Standard (previous generation) - legacy workloads

* **EBS Features:**
  - Persistent block-level storage cho EC2 instances
  - Independent từ instance lifecycle
  - Automatic replication trong AZ
  - Snapshots cho backup và recovery
  - Volume encryption cho data security
  - Có thể resize volumes mà không cần stop instance (gp3, io2)

* **EBS Snapshots:**
  - Point-in-time copy của EBS volume
  - Được lưu trong S3 (incremental snapshots)
  - Sử dụng cho backup, disaster recovery, volume copy
  - Có thể tạo AMI từ snapshot
  - Encryption được duy trì cho encrypted volumes

### 4. Instance Store

* **Instance Store Characteristics:**
  - Ephemeral (temporary) block-level storage
  - Được gắn vật lý vào host computer
  - High IOPS performance, low latency
  - Không có chi phí bổ sung (bao gồm trong instance price)
  - Dữ liệu bị mất khi instance stops/terminates

* **Use Cases:**
  - Temporary files, caches
  - Session data (replicated ở nơi khác cho HA)
  - High-speed temporary processing
  - Data có thể được regenerated

* **Best Practices:**
  - Không bao giờ sử dụng cho critical/persistent data
  - Kết hợp với EBS cho persistent storage
  - Sử dụng cho specific performance requirements chỉ

### 5. User Data

* **User Data Scripts:**
  - Chạy khi instance launch (root/admin privileges)
  - Bash script cho Linux, PowerShell cho Windows
  - Automate software installation và configuration
  - Common uses: web server setup, agent installation, monitoring tools

* **Limitations:**
  - Chỉ chạy ở first launch
  - Output được ghi vào /var/log/cloud-init-output.log
  - Phải là idempotent (safe để chạy multiple times)
  - Nên hoàn thành nhanh để tránh timeout

### 6. Metadata

* **EC2 Instance Metadata:**
  - Thông tin về running instance accessible từ trong instance
  - Retrieved qua HTTP call tới 169.254.169.254
  - Bao gồm: instance ID, instance type, VPC, security groups, IAM role, v.v.

* **Common Metadata Uses:**
  - Xác định instance configuration
  - Retrieve IAM role credentials
  - Lấy public/private IP addresses
  - Configure applications dựa trên instance type
  - Service discovery

* **Example Metadata Query:**
  ```bash
  # Lấy instance ID
  curl http://169.254.169.254/latest/meta-data/instance-id
  
  # Lấy instance type
  curl http://169.254.169.254/latest/meta-data/instance-type
  
  # Lấy IAM role credentials
  curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
  ```

### 7. EC2 Auto Scaling

* **Auto Scaling Concepts:**
  - Launch templates định nghĩa instance configuration
  - Auto Scaling groups quản lý instances dựa trên policies
  - Scaling policies (target tracking, step scaling, simple scaling)
  - Minimum, maximum, và desired capacity settings

* **Key Features:**
  - Automatic instance launch khi demand tăng
  - Automatic termination khi demand giảm
  - Health checks xóa unhealthy instances
  - Integration với load balancers
  - Cost optimization thông qua right-sizing

* **Scaling Policies:**
  - Target Tracking: duy trì target metric (ví dụ: 70% CPU)
  - Step Scaling: scale dựa trên metric thresholds
  - Simple Scaling: single scaling action mỗi alarm

### 8. EC2 Autoscaling - EFS/FSx - Lightsail - MGN

* **EC2 Autoscaling Advanced:**
  - Lifecycle hooks cho graceful instance termination
  - Termination policies (oldest instance, default policies)
  - Scheduled scaling (scale ở specific times)
  - Capacity reservations cho reserved capacity

* **Elastic File System (EFS):**
  - Managed NFS file system
  - Shared storage across EC2 instances trong multiple AZs
  - Auto-scaling storage capacity
  - Use cases: shared application files, web servers, CMS

* **FSx:**
  - Windows File Server (SMB protocol) hoặc Lustre (HPC)
  - Managed file storage service
  - Full Windows compatibility
  - High performance cho specific workloads

* **Lightsail:**
  - Simplified, low-cost cloud platform
  - Bao gồm compute, storage, networking
  - Fixed monthly pricing
  - Phù hợp cho small applications, websites, development

* **Application Migration Service (MGN):**
  - Migrate applications từ on-premise sang AWS
  - Minimal downtime trong cutover
  - Test migration trước cutover
  - Replication agent được cài trên source servers

---

## Các Dịch Vụ AWS Học Được:

### Compute Services
* **EC2 (Elastic Compute Cloud):** Virtual machines với flexible configurations
* **Auto Scaling:** Tự động scale EC2 instances dựa trên demand
* **Lightsail:** Simplified cloud platform cho small workloads

### Storage Services
* **EBS (Elastic Block Store):** Persistent block-level storage cho EC2
* **EFS (Elastic File System):** Managed shared network file system
* **FSx:** Managed file storage cho Windows và HPC

### Migration Services
* **Application Migration Service (MGN):** Migrate applications sang AWS

---

## Các Lệnh & Tài Nguyên Chính:

**EC2 CLI Commands:**
```bash
# Describe instances
aws ec2 describe-instances

# Tạo AMI từ instance
aws ec2 create-image --instance-id i-xxxxx --name my-ami

# Tạo EBS snapshot
aws ec2 create-snapshot --volume-id vol-xxxxx --description "Backup snapshot"

# Tạo Auto Scaling group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template,Version='$Latest' \
  --min-size 2 --max-size 6 --desired-capacity 3 \
  --vpc-zone-identifier "subnet-xxxxx,subnet-yyyyy"

# Lấy instance metadata
curl http://169.254.169.254/latest/meta-data/
```

---

## Tài Nguyên & Tham Khảo:

* [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
* [EC2 User Guide](https://docs.aws.amazon.com/ec2/)
* [EBS Documentation](https://docs.aws.amazon.com/ebs/)
* [EFS Documentation](https://docs.aws.amazon.com/efs/)
* [Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/)
* [Application Migration Service](https://docs.aws.amazon.com/application-migration-service/)

---

### Ghi Chú:

* **Tối ưu hóa chi phí:** Chọn appropriate instance types cho workload của bạn
* **Hiệu suất:** Sử dụng EBS cho persistent storage, instance store cho temporary
* **Tự động hóa:** User data và metadata kích hoạt instance automation
* **Khả năng mở rộng:** Auto Scaling đảm bảo availability và cost efficiency
* **Testing:** Luôn test user data scripts trước production deployment