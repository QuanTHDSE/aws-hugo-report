---
title: " Week 1 Worklog"
date: "2025-11-30"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect and get to know members in First Cloud Journey.
* Understand basic AWS services, how to use console & CLI.
* Set up AWS Free Tier account and local development environment.
* Practice with EC2, S3, and basic IAM.

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                            | Start Date | End Date   | Resources                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 2   | - Get to know FCJ members <br> - Read and note regulations and policies at the internship unit                                                                                                                                                                                                                                                                                   | 11/09/2025 | 11/09/2025 | FCJ Guide                                 |
| 3   | - Explore AWS and its service types <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC, Security Groups) <br>&emsp; + Database (RDS, DynamoDB) <br>&emsp; + Identity & Access (IAM)                                                                                                                                                  | 12/09/2025 | 12/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account <br> - Enable MFA for root account <br> - Explore AWS Console & AWS CLI <br> - **Practice:** <br>&emsp; + Create AWS account <br>&emsp; + Install AWS CLI v2 & configure <br>&emsp; + Check connection: `aws sts get-caller-identity`                                                                                                             | 13/08/2025 | 13/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic IAM: <br>&emsp; + Create IAM user with least privilege <br>&emsp; + Create Access Keys <br>&emsp; + Understand roles vs policies <br> - Learn basic EC2: <br>&emsp; + Instance types (t3.micro, t4g.micro) <br>&emsp; + AMI (Amazon Machine Images) <br>&emsp; + EBS volumes & snapshots <br>&emsp; + Security Groups & Key Pairs <br>&emsp; + Elastic IPs         | 14/08/2025 | 15/08/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Hands-on Lab 1 - EC2:** <br>&emsp; + Create SSH key pair in AWS Console <br>&emsp; + Launch EC2 instance t3.micro (Linux) <br>&emsp; + Configure Security Group (allow port 22 from your IP) <br>&emsp; + SSH connect from Windows Terminal or PuTTY <br>&emsp; + Create EBS volume, attach & mount to EC2 <br>&emsp; + Check data persistence by stopping/starting instance | 15/08/2025 | 15/08/2025 | <https://cloudjourney.awsstudygroup.com/> |

## Results Achieved in Week 1:

### 1. Starting the Cloud Journey

* Understand what AWS is and why it matters
* Grasp cloud deployment models (Public, Private, Hybrid)
* Identify advantages of cloud computing over on-premise
* Understand real-world use cases of AWS

### 2. AWS Global Infrastructure

* Understand AWS Global Infrastructure structure (Regions, Availability Zones, Edge Locations)
* Know the criteria for choosing the right Region:
  - Latency
  - Compliance
  - Pricing
  - Service availability
* Understand benefits of Multi-AZ deployment for high availability
* Concept of Edge Locations and CloudFront

### 3. AWS Services Management Tools

* **AWS Management Console:**
  - Familiarize with web console interface
  - Navigate and search for services
  - Understand main menu structure

* **AWS CLI (Command Line Interface):**
  - Install AWS CLI v2 on Windows
  - Configure credentials (`aws configure`)
  - Perform basic commands
  - Understand output formats (JSON, text, table)

* **AWS SDKs:**
  - Concept of SDKs for programming
  - Supported languages (Python, Java, Node.js, etc.)

* **Infrastructure as Code (IaC):**
  - Introduction to CloudFormation and Terraform
  - Benefits of IaC

### 4. Cost Optimization on AWS

* **Pricing Models:**
  - Pay-as-you-go
  - Reserved Instances
  - Spot Instances
  - Savings Plans

* **AWS Free Tier:**
  - Free services for first 12 months
  - Always free services
  - Free tier limits

* **Cost Management Tools:**
  - AWS Pricing Calculator
  - AWS Cost Explorer
  - Billing Alerts & CloudWatch

* **Working With AWS Support:**
  - Support levels (Basic, Developer, Business, Enterprise)
  - How to contact support
  - AWS documentation and resources

### 5. Practice and Further Research

* Create AWS Free Tier account
* Enable MFA for root account
* Configure AWS CLI and check connection
* Create IAM user with least privilege
* Understand Access Keys and Secret Keys
* Familiarize with AWS documentation

---

## Basic AWS Services Learned:

### Compute
* **EC2 (Elastic Compute Cloud):** Virtual machines on-demand
* **Lambda:** Serverless computing

### Storage
* **S3 (Simple Storage Service):** Object storage
* **EBS (Elastic Block Store):** Block-level storage for EC2

### Networking
* **VPC (Virtual Private Cloud):** Isolated network environment
* **Security Groups:** Virtual firewall

### Database
* **RDS (Relational Database Service):** Managed relational databases
* **DynamoDB:** NoSQL database

### Identity & Access
* **IAM (Identity and Access Management):** User, role, permission management

---

## Resources & References:

* [AWS Getting Started Guide](https://aws.amazon.com/getting-started/)
* [AWS Free Tier Documentation](https://aws.amazon.com/free/)
* [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
* [AWS Pricing](https://aws.amazon.com/pricing/)
* [AWS CLI User Guide](https://docs.aws.amazon.com/cli/)
* [AWS Support](https://aws.amazon.com/support/)

---

### Notes:

* **Security:** Never commit AWS credentials to version control
* **Monitor costs:** Check billing dashboard regularly
* **Learn more:** Use AWS documentation and free resources