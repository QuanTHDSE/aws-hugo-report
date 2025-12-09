---
title: "Week 3 Worklog"
date: "2025-11-30"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Master EC2 instance types and configurations.
* Understand AMI creation and backup strategies.
* Learn EBS storage and snapshots management.
* Master EC2 Auto Scaling and load distribution.
* Implement instance metadata and user data scripts.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                  | Start Date | End Date   | Resources                                 |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 2 results and VPC setup <br> - Prepare EC2 instances for hands-on labs                                                                                                                                                                                                                                   | 25/09/2025 | 25/09/2025 | Week 2 Notes                              |
| 2   | - Learn EC2 Instance Types: <br>&emsp; + General Purpose (t3, m5, m6i) <br>&emsp; + Compute Optimized (c5, c6i) <br>&emsp; + Memory Optimized (r5, r6i, x1) <br>&emsp; + Storage Optimized (i3, i4i) <br>&emsp; + Instance sizing and naming conventions <br>&emsp; + Right-sizing for workloads                       | 26/09/2025 | 26/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn AMI, Backup & Key Pair: <br>&emsp; + Create custom AMI from running instance <br>&emsp; + AMI management and sharing <br>&emsp; + SSH key pair creation and management <br>&emsp; + Best practices for key storage <br>&emsp; + Disaster recovery with AMI                                                     | 27/09/2025 | 27/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn EBS Storage Management: <br>&emsp; + EBS volume types (gp3, io1/io2, st1, sc1) <br>&emsp; + Create, attach, and mount EBS volumes <br>&emsp; + EBS snapshots and point-in-time recovery <br>&emsp; + Volume encryption and performance tuning <br>&emsp; + Snapshot scheduling and lifecycle                   | 28/09/2025 | 28/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn User Data & Instance Metadata: <br>&emsp; + Write user data scripts (Bash for Linux) <br>&emsp; + Automate instance initialization <br>&emsp; + Query instance metadata (curl 169.254.169.254) <br>&emsp; + Use metadata for dynamic configuration <br>&emsp; + CloudWatch integration                         | 29/09/2025 | 29/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Learn EC2 Auto Scaling: <br>&emsp; + Create launch templates <br>&emsp; + Create Auto Scaling groups <br>&emsp; + Configure scaling policies (target tracking, step, simple) <br>&emsp; + Set min/max/desired capacity <br>&emsp; + Integrate with load balancers <br>&emsp; + Lifecycle hooks for graceful shutdown | 30/09/2025 | 30/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Learn EFS, FSx, Lightsail & MGN: <br>&emsp; + Elastic File System (EFS) setup <br>&emsp; + FSx for Windows/Lustre basics <br>&emsp; + Lightsail as simplified cloud alternative <br>&emsp; + Application Migration Service (MGN) concepts <br>&emsp; + Migration planning and execution                              | 31/09/2025 | 31/09/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

## Results Achieved in Week 3:

### 1. EC2 Instance Types

* **Instance Family Categories:**
  - General Purpose (t3, m5, m6i) - balanced compute, memory, networking
  - Compute Optimized (c5, c6i) - high-performance processors
  - Memory Optimized (r5, r6i, x1) - large memory workloads
  - Storage Optimized (i3, i4i, h1) - high sequential I/O access
  - Accelerated Computing (p3, g4, f1) - GPU/FPGA workloads

* **Instance Naming Convention:**
  - t3.micro, m5.large, c5.xlarge, r6i.2xlarge
  - First part = family, second part = generation, third part = size
  - Larger size = more vCPUs, memory, and network performance

* **Key Considerations:**
  - Right-sizing instances based on workload requirements
  - Cost optimization through appropriate instance selection
  - Performance vs. cost trade-offs
  - Burstable vs. steady-state performance

### 2. AMI / Backup / Key Pair

* **Amazon Machine Image (AMI):**
  - Template containing OS, applications, and configurations
  - Public AMIs (AWS-provided, community, marketplace)
  - Custom AMIs created from running instances
  - AMI includes OS, root volume configuration, and block device mappings

* **Key Pair Management:**
  - SSH key pairs for Linux instance access
  - RDP password for Windows instance access
  - Key pairs stored in AWS (download once)
  - Different key pair per region/use-case recommended
  - Never share private keys

* **Instance Backup & Recovery:**
  - Create AMI from running instance (includes all volumes)
  - Snapshots for point-in-time recovery
  - Launch new instances from custom AMI
  - Useful for disaster recovery and instance replication

### 3. Elastic Block Store (EBS)

* **EBS Volume Types:**
  - gp3 (General Purpose) - balanced price/performance for most workloads
  - io1/io2 (Provisioned IOPS) - high-performance databases, NoSQL
  - st1 (Throughput Optimized) - big data, data warehouses
  - sc1 (Cold HDD) - infrequent access, low-cost storage
  - Standard (previous generation) - legacy workloads

* **EBS Features:**
  - Persistent block-level storage for EC2 instances
  - Independent from instance lifecycle
  - Automatic replication within AZ
  - Snapshots for backup and recovery
  - Volume encryption for data security
  - Can resize volumes without stopping instance (gp3, io2)

* **EBS Snapshots:**
  - Point-in-time copy of EBS volume
  - Stored in S3 (incremental snapshots)
  - Used for backup, disaster recovery, volume copy
  - Can create AMI from snapshot
  - Encryption maintained for encrypted volumes

### 4. Instance Store

* **Instance Store Characteristics:**
  - Ephemeral (temporary) block-level storage
  - Physically attached to host computer
  - High IOPS performance, low latency
  - No additional cost (included in instance price)
  - Data lost when instance stops/terminates

* **Use Cases:**
  - Temporary files, caches
  - Session data (replicated elsewhere for HA)
  - High-speed temporary processing
  - Data that can be regenerated

* **Best Practices:**
  - Never use for critical/persistent data
  - Combine with EBS for persistent storage
  - Use for specific performance requirements only

### 5. User Data

* **User Data Scripts:**
  - Run at instance launch (root/admin privileges)
  - Bash script for Linux, PowerShell for Windows
  - Automate software installation and configuration
  - Common uses: web server setup, agent installation, monitoring tools

* **Limitations:**
  - Runs only at first launch
  - Output logged to /var/log/cloud-init-output.log
  - Must be idempotent (safe to run multiple times)
  - Should complete quickly to avoid timeout

### 6. Metadata

* **EC2 Instance Metadata:**
  - Information about running instance accessible from within instance
  - Retrieved via HTTP call to 169.254.169.254
  - Includes: instance ID, instance type, VPC, security groups, IAM role, etc.

* **Common Metadata Uses:**
  - Determine instance configuration
  - Retrieve IAM role credentials
  - Get public/private IP addresses
  - Configure applications based on instance type
  - Service discovery

* **Example Metadata Query:**
  ```bash
  # Get instance ID
  curl http://169.254.169.254/latest/meta-data/instance-id
  
  # Get instance type
  curl http://169.254.169.254/latest/meta-data/instance-type
  
  # Get IAM role credentials
  curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
  ```

### 7. EC2 Auto Scaling

* **Auto Scaling Concepts:**
  - Launch templates define instance configuration
  - Auto Scaling groups manage instances based on policies
  - Scaling policies (target tracking, step scaling, simple scaling)
  - Minimum, maximum, and desired capacity settings

* **Key Features:**
  - Automatic instance launch when demand increases
  - Automatic termination when demand decreases
  - Health checks remove unhealthy instances
  - Integration with load balancers
  - Cost optimization through right-sizing

* **Scaling Policies:**
  - Target Tracking: maintain target metric (e.g., 70% CPU)
  - Step Scaling: scale based on metric thresholds
  - Simple Scaling: single scaling action per alarm

### 8. EC2 Autoscaling - EFS/FSx - Lightsail - MGN

* **EC2 Autoscaling Advanced:**
  - Lifecycle hooks for graceful instance termination
  - Termination policies (oldest instance, default policies)
  - Scheduled scaling (scale at specific times)
  - Capacity reservations for reserved capacity

* **Elastic File System (EFS):**
  - Managed NFS file system
  - Shared storage across EC2 instances in multiple AZs
  - Auto-scaling storage capacity
  - Use cases: shared application files, web servers, CMS

* **FSx:**
  - Windows File Server (SMB protocol) or Lustre (HPC)
  - Managed file storage service
  - Full Windows compatibility
  - High performance for specific workloads

* **Lightsail:**
  - Simplified, low-cost cloud platform
  - Includes compute, storage, networking
  - Fixed monthly pricing
  - Good for small applications, websites, development

* **Application Migration Service (MGN):**
  - Migrate applications from on-premise to AWS
  - Minimal downtime during cutover
  - Test migration before cutover
  - Replication agent installed on source servers

---

## AWS Services Learned:

### Compute Services
* **EC2 (Elastic Compute Cloud):** Virtual machines with flexible configurations
* **Auto Scaling:** Automatically scale EC2 instances based on demand
* **Lightsail:** Simplified cloud platform for small workloads

### Storage Services
* **EBS (Elastic Block Store):** Persistent block-level storage for EC2
* **EFS (Elastic File System):** Managed shared network file system
* **FSx:** Managed file storage for Windows and HPC

### Migration Services
* **Application Migration Service (MGN):** Migrate applications to AWS

---

## Key Commands & Resources:

**EC2 CLI Commands:**
```bash
# Describe instances
aws ec2 describe-instances

# Create AMI from instance
aws ec2 create-image --instance-id i-xxxxx --name my-ami

# Create EBS snapshot
aws ec2 create-snapshot --volume-id vol-xxxxx --description "Backup snapshot"

# Create Auto Scaling group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template,Version='$Latest' \
  --min-size 2 --max-size 6 --desired-capacity 3 \
  --vpc-zone-identifier "subnet-xxxxx,subnet-yyyyy"

# Get instance metadata
curl http://169.254.169.254/latest/meta-data/
```

---

## Resources & References:

* [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
* [EC2 User Guide](https://docs.aws.amazon.com/ec2/)
* [EBS Documentation](https://docs.aws.amazon.com/ebs/)
* [EFS Documentation](https://docs.aws.amazon.com/efs/)
* [Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/)
* [Application Migration Service](https://docs.aws.amazon.com/application-migration-service/)

---

### Notes:

* **Cost Optimization:** Choose appropriate instance types for your workload
* **Performance:** Use EBS for persistent storage, instance store for temporary
* **Automation:** User data and metadata enable instance automation
* **Scalability:** Auto Scaling ensures availability and cost efficiency
* **Testing:** Always test user data scripts before production deployment