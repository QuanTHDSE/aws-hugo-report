---
title: "Week 4 Worklog"
date: "2025-11-30"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Master AWS storage services and data management.
* Understand backup and disaster recovery strategies.
* Learn data migration and archival solutions.
* Implement cost optimization for storage.
* Design scalable and resilient storage architecture.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                       | Start Date | End Date   | Resources                                 |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 3 results and EC2 infrastructure <br> - Prepare storage environment for hands-on labs                                                                                                                                                                                                                                                         | 08/10/2025 | 08/10/2025 | Week 3 Notes                              |
| 2   | - Learn AWS Storage Services: <br>&emsp; + S3 storage classes (Standard, Intelligent-Tiering, Glacier) <br>&emsp; + Access points and storage class analysis <br>&emsp; + S3 performance optimization <br>&emsp; + Object lifecycle management <br>&emsp; + Storage cost analysis                                                                           | 09/10/2025 | 09/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn S3 Advanced Features: <br>&emsp; + Static website hosting & CORS <br>&emsp; + Control access (bucket policies, ACLs) <br>&emsp; + Object key naming for performance <br>&emsp; + Versioning and MFA delete <br>&emsp; + Glacier for long-term archival <br>&emsp; + Cross-region replication                                                        | 10/10/2025 | 10/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Storage Gateway and Backup: <br>&emsp; + AWS Storage Gateway concepts <br>&emsp; + Storage Gateway types (S3, EBS, Tape) <br>&emsp; + Backup strategies and snapshots <br>&emsp; + AWS Backup service <br>&emsp; + Disaster recovery planning <br>&emsp; + RTO and RPO metrics                                                                      | 11/10/2025 | 11/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Hands-on Lab 1 - S3 Storage Classes & Lifecycle:** <br>&emsp; + Create S3 bucket with versioning <br>&emsp; + Configure intelligent-tiering <br>&emsp; + Set up lifecycle policies (move to Glacier after 90 days) <br>&emsp; + Enable storage class analysis <br>&emsp; + Analyze cost savings <br>&emsp; + Test object retrieval from different tiers | 12/10/2025 | 12/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Hands-on Lab 2 - S3 Static Website & CORS:** <br>&emsp; + Create S3 bucket for static website <br>&emsp; + Upload HTML/CSS/JS files <br>&emsp; + Enable static website hosting <br>&emsp; + Configure CORS policy <br>&emsp; + Set up bucket policy for public read <br>&emsp; + Test website access and cross-origin requests                          | 13/10/2025 | 13/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Hands-on Lab 3 - Backup & Disaster Recovery:** <br>&emsp; + Create EBS snapshots manually <br>&emsp; + Set up automated snapshot schedules <br>&emsp; + Enable cross-region snapshot copy <br>&emsp; + Create AWS Backup plan <br>&emsp; + Test restore from backup <br>&emsp; + Document RTO/RPO targets <br>&emsp; + Set up backup notifications      | 14/10/2025 | 14/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

## Results Achieved in Week 4:

### 1. AWS Storage Services Fundamentals

* **S3 Storage Classes:**
  - Standard: frequently accessed data, lowest latency
  - Intelligent-Tiering: automatic cost optimization based on access patterns
  - Standard-IA (Infrequent Access): lower cost for less frequent access
  - One Zone-IA: single AZ storage for non-critical data
  - Glacier Instant Retrieval: archival with millisecond retrieval
  - Glacier Flexible Retrieval: archival with hours retrieval time
  - Deep Archive: lowest cost for long-term compliance storage

* **Access Points:**
  - Simplify access to S3 buckets for different applications
  - Each access point has its own DNS name
  - Can configure different policies per access point
  - Multi-region access points for global distribution

* **Storage Class Analysis:**
  - CloudWatch metrics for storage costs
  - Identify objects eligible for transition
  - Optimize storage class selection
  - Cost prediction and reporting

### 2. S3 Advanced Features

* **Static Website Hosting:**
  - Host static websites (HTML, CSS, JavaScript) on S3
  - Configure index and error documents
  - Enable website endpoint access
  - Serve content globally via CloudFront

* **CORS (Cross-Origin Resource Sharing):**
  - Allow browsers to request resources from different origins
  - Configure allowed origins, methods, headers
  - Required for JavaScript APIs accessing S3 from web applications
  - Security policy to prevent unauthorized cross-origin access

* **Control Access:**
  - Bucket policies: JSON-based access control
  - Object ACLs: granular per-object permissions
  - Public read access for websites
  - Block Public Access settings for security

* **Object Key Naming:**
  - Prefixes for logical organization
  - Random prefixes improve performance (sharding)
  - Avoid sequential prefixes that create hot partitions
  - Key naming strategy affects request rates

* **Versioning & MFA Delete:**
  - Maintain multiple versions of objects
  - Recover from accidental deletion
  - MFA delete protection for critical objects
  - Version ID required to delete specific versions

* **Glacier Archive:**
  - Long-term archival (7+ years compliance)
  - Significantly lower cost than standard storage
  - Retrieval times from minutes to hours
  - Suitable for compliance and historical data

* **Cross-Region Replication:**
  - Automatic replication to secondary region
  - Disaster recovery and business continuity
  - Different storage classes in replicas
  - Replication for compliance requirements

### 3. Storage Gateway & Backup Solutions

* **AWS Storage Gateway:**
  - Hybrid cloud storage solution
  - Connects on-premises data to AWS

  **Storage Gateway Types:**
  - S3 File Gateway: NFS/SMB access to S3 objects
  - EBS Volume Gateway: iSCSI block storage backed by EBS
  - Tape Gateway: virtual tape library for backup

* **Backup Strategies:**
  - Snapshots: point-in-time copies of volumes
  - Incremental snapshots: only changed data
  - Cross-region snapshot copy for DR
  - Snapshot lifecycle policies

* **AWS Backup Service:**
  - Centralized backup management
  - Support for EC2, EBS, RDS, Aurora, DynamoDB, EFS, FSx
  - Backup policies and schedules
  - Cross-account and cross-region backup
  - On-demand and scheduled backups

* **Disaster Recovery Planning:**
  - **RTO (Recovery Time Objective):** maximum acceptable downtime
  - **RPO (Recovery Point Objective):** maximum acceptable data loss
  - Backup frequency based on RPO requirements
  - Automated failover mechanisms
  - Regular disaster recovery testing

### 4. Cost Optimization for Storage

* **Lifecycle Policies:**
  - Automatically transition objects to cheaper storage classes
  - Schedule deletion of old data
  - Combine with Intelligent-Tiering
  - Reduce storage costs over time

* **S3 Intelligent-Tiering:**
  - Automatic cost optimization
  - No retrieval fees in frequent access tier
  - Seamless transition between tiers
  - Monitoring and alerts

* **Cost Analysis:**
  - CloudWatch metrics tracking
  - S3 Storage Lens for comprehensive analytics
  - Cost allocation tags
  - Storage class analysis recommendations

### 5. Scalable & Resilient Storage Architecture

* **Multi-Region Architecture:**
  - Primary region for active workloads
  - Secondary region for disaster recovery
  - Cross-region replication for data durability
  - Route 53 failover for application access

* **High Availability:**
  - Multiple AZ deployment
  - Automatic snapshots across AZs
  - Cross-region backup copies
  - Redundancy at storage and application level

* **Security & Compliance:**
  - Encryption at rest and in transit
  - Versioning for data protection
  - Bucket policies and access logging
  - Compliance with regulatory requirements

---

## AWS Services Learned:

### Storage Services
* **S3 (Simple Storage Service):** Object storage with multiple tiers
* **Storage Gateway:** Hybrid cloud storage solution
* **Backup:** Centralized backup and recovery service

### Archival & Disaster Recovery
* **Glacier:** Long-term archival storage
* **EBS Snapshots:** Point-in-time volume copies
* **Cross-Region Replication:** Data replication for DR

---

## Key Commands & Resources:

**S3 Lifecycle & Tiering CLI Commands:**
```bash
# Create lifecycle policy
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration file://lifecycle.json

# Enable Intelligent-Tiering
aws s3api put-bucket-intelligent-tiering-configuration \
  --bucket my-bucket \
  --id auto-tiering \
  --intelligent-tiering-configuration file://tiering.json

# Get storage metrics
aws s3api list-bucket-metrics-configurations --bucket my-bucket

# Enable versioning
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled

# List object versions
aws s3api list-object-versions --bucket my-bucket
```

**S3 Static Website & CORS CLI Commands:**
```bash
# Enable static website hosting
aws s3api put-bucket-website \
  --bucket my-bucket \
  --website-configuration file://website.json

# Put bucket policy for public read
aws s3api put-bucket-policy \
  --bucket my-bucket \
  --policy file://policy.json

# Enable CORS
aws s3api put-bucket-cors \
  --bucket my-bucket \
  --cors-configuration file://cors.json

# Upload website files
aws s3 cp index.html s3://my-bucket/
aws s3 cp . s3://my-bucket/ --recursive
```

**Backup & Snapshot CLI Commands:**
```bash
# Create EBS snapshot
aws ec2 create-snapshot \
  --volume-id vol-xxxxx \
  --description "Database backup"

# Create snapshot lifecycle policy
aws dlm create-lifecycle-policy \
  --execution-role-arn arn:aws:iam::123456789012:role/DLMRole \
  --description "Daily snapshots" \
  --state ENABLED \
  --policy-details file://policy.json

# Copy snapshot to another region
aws ec2 copy-snapshot \
  --source-region us-east-1 \
  --source-snapshot-id snap-xxxxx \
  --destination-region us-west-2

# Create AWS Backup plan
aws backup create-backup-plan \
  --backup-plan file://backup-plan.json

# Create backup vault
aws backup create-backup-vault \
  --backup-vault-name my-vault \
  --encryption-key-arn arn:aws:kms:us-east-1:123456789012:key/xxxxx
```

---

## Example Configurations:

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

## Resources & References:

* [S3 User Guide](https://docs.aws.amazon.com/s3/)
* [S3 Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
* [Storage Gateway Documentation](https://docs.aws.amazon.com/storagegateway/)
* [AWS Backup User Guide](https://docs.aws.amazon.com/aws-backup/)
* [EBS Snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/EBSSnapshots.html)
* [Disaster Recovery on AWS](https://aws.amazon.com/disaster-recovery/)

---

### Notes:

* **Cost Optimization:** Use Intelligent-Tiering and lifecycle policies to reduce storage costs
* **Data Protection:** Enable versioning and cross-region replication for critical data
* **Compliance:** Archive data to Glacier for long-term compliance requirements
* **Performance:** Use S3 prefixes and access points for scalable access patterns
* **Testing:** Regularly test disaster recovery procedures and backup restoration