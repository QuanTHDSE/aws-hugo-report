---
title: "Week 6 Worklog"
date: "2025-11-30"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Design and deploy a complete 3-tier web application architecture.
* Integrate all AWS services learned in Week 1-5.
* Implement CI/CD pipeline with GitHub, CodePipeline, and CodeBuild.
* Set up comprehensive monitoring and security.
* Deploy application on containerized infrastructure.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                            | Start Date | End Date   | Resources                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 1-5 services and architecture <br> - Plan capstone project scope and design <br> - Prepare GitHub repository and IAM roles                                                                                                                                                         | 22/10/2025 | 22/10/2025 | Week 1-5 Notes                            |
| 2   | - **Infrastructure Setup:** <br>&emsp; + Create VPC with public/private subnets <br>&emsp; + Configure security groups and NACLs <br>&emsp; + Set up NAT Gateway for private subnets <br>&emsp; + Create RDS Multi-AZ database <br>&emsp; + Provision S3 buckets with encryption                 | 23/10/2025 | 23/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Frontend & CDN Setup:** <br>&emsp; + Create S3 bucket for static website <br>&emsp; + Upload HTML/CSS/JavaScript files <br>&emsp; + Configure CloudFront distribution <br>&emsp; + Set up Route 53 DNS <br>&emsp; + Enable CORS for API requests                                             | 24/10/2025 | 24/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **CI/CD Pipeline Setup:** <br>&emsp; + Connect GitHub repository to AWS <br>&emsp; + Create CodePipeline for automated deployment <br>&emsp; + Configure CodeBuild for application build <br>&emsp; + Set up CodeDeploy for instance deployment <br>&emsp; + Create build and deploy artifacts | 25/10/2025 | 25/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Application Layer Deployment:** <br>&emsp; + Create Application Load Balancer (ALB) <br>&emsp; + Launch EC2 instances with application <br>&emsp; + Configure auto-scaling groups (min: 2, max: 4) <br>&emsp; + Register instances with ALB <br>&emsp; + Test application endpoints          | 26/10/2025 | 26/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Monitoring & Logging Setup:** <br>&emsp; + Configure CloudWatch dashboards <br>&emsp; + Set up alarms for CPU, memory, and errors <br>&emsp; + Enable CloudTrail for audit logging <br>&emsp; + Configure VPC Flow Logs <br>&emsp; + Set up SNS notifications for alerts                     | 27/10/2025 | 27/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Testing & Validation:** <br>&emsp; + Perform load testing with Apache Bench <br>&emsp; + Verify auto-scaling behavior <br>&emsp; + Test database connectivity and failover <br>&emsp; + Validate security configurations <br>&emsp; + Document architecture and deployment procedures        | 28/10/2025 | 28/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

## Capstone Project Architecture Overview:

### Architecture Components:

#### 1. **Frontend Layer (CloudFront + S3)**
   - S3 bucket hosting static website (HTML, CSS, JavaScript)
   - CloudFront distribution for global content delivery
   - Route 53 for DNS routing
   - CORS configured for API requests

#### 2. **Application Layer (ALB + EC2)**
   - Application Load Balancer distributing traffic
   - Auto Scaling Group with 2-4 EC2 instances
   - Target group health checks
   - Application code deployed via CodeDeploy

#### 3. **Data Layer (RDS)**
   - MySQL/PostgreSQL RDS instance (Multi-AZ)
   - Automated backups enabled
   - Security group restricting access to EC2 only
   - Read replica for scalability

#### 4. **Storage (S3 + Media Convert)**
   - S3 bucket for application data and media
   - Server-side encryption enabled
   - CloudFront integration for media delivery
   - Media conversion jobs for video processing

#### 5. **CI/CD Pipeline (GitHub + CodePipeline)**
   - GitHub repository for source control
   - CodePipeline orchestrating deployment
   - CodeBuild for compiling and testing code
   - CodeDeploy for instance deployment
   - Automated triggering on code push

#### 6. **Networking (VPC + Security)**
   - Custom VPC with CIDR 10.0.0.0/16
   - Public subnets (10.0.1.0/24, 10.0.2.0/24) for ALB
   - Private subnets (10.0.3.0/24, 10.0.4.0/24) for EC2 and RDS
   - NAT Gateway for private subnet internet access
   - Security groups for each tier

#### 7. **Monitoring & Logging**
   - CloudWatch dashboards for metrics
   - CloudWatch alarms for auto-scaling triggers
   - CloudTrail for API audit logging
   - VPC Flow Logs for network analysis
   - SNS for alert notifications

#### 8. **Authentication & Access**
   - Cognito for user authentication (optional)
   - IAM roles for services
   - Least privilege access policies
   - KMS encryption for sensitive data

---
