---
title: "Week 2 Worklog"
date: "2025-11-30"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Deepen understanding of VPC (Virtual Private Cloud) and network configuration.
* Master VPC security features and Multi-VPC capabilities.
* Learn about VPN, DirectConnect and Load Balancer.
* Practice with VPC networking and security configuration.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | End Date   | Resources                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 1 results <br> - Prepare workspace for Week 2 hands-on labs                                                                                                                                                                                                                                                                                                                                                              | 18/09/2025 | 18/09/2025 | Week 1 Notes                              |
| 2   | - Learn basic VPC: <br>&emsp; + Create VPC & CIDR blocks <br>&emsp; + Subnets (public & private) <br>&emsp; + Route tables & routing <br>&emsp; + Internet Gateway (IGW) <br>&emsp; + NAT Gateway for private subnets <br>&emsp; + Network ACLs vs Security Groups                                                                                                                                                                     | 19/09/2025 | 19/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn VPC Security & Multi-VPC: <br>&emsp; + Security Groups configuration <br>&emsp; + Network ACLs setup <br>&emsp; + VPC Peering concepts <br>&emsp; + VPC Endpoints (Gateway & Interface) <br>&emsp; + Flow Logs setup <br>&emsp; + Multi-VPC architecture patterns                                                                                                                                                              | 20/09/2025 | 20/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn VPN, DirectConnect & Load Balancer: <br>&emsp; + Site-to-Site VPN concepts <br>&emsp; + Client VPN setup <br>&emsp; + AWS DirectConnect basics <br>&emsp; + Application Load Balancer (ALB) <br>&emsp; + Network Load Balancer (NLB) <br>&emsp; + Health checks & Auto Scaling                                                                                                                                                 | 21/09/2025 | 21/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Hands-on Lab 1 - VPC & Networking:** <br>&emsp; + Create custom VPC with CIDR 10.0.0.0/16 <br>&emsp; + Create public subnet (10.0.1.0/24) and private subnet (10.0.2.0/24) <br>&emsp; + Create and attach Internet Gateway <br>&emsp; + Configure route tables for public/private subnets <br>&emsp; + Launch EC2 in public subnet & test internet access <br>&emsp; + Launch EC2 in private subnet & test bastion host connection | 22/09/2025 | 22/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Hands-on Lab 2 - VPC Security & Peering:** <br>&emsp; + Configure Security Groups with inbound/outbound rules <br>&emsp; + Setup Network ACLs for subnet-level filtering <br>&emsp; + Create VPC Peering between two VPCs <br>&emsp; + Test instance communication across peered VPCs <br>&emsp; + Enable VPC Flow Logs for traffic analysis <br>&emsp; + Monitor traffic in CloudWatch                                            | 23/09/2025 | 23/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Hands-on Lab 3 - Load Balancer & Auto Scaling:** <br>&emsp; + Create Application Load Balancer (ALB) <br>&emsp; + Configure target groups and health checks <br>&emsp; + Launch EC2 instances with web application <br>&emsp; + Register instances with ALB <br>&emsp; + Create Auto Scaling Group with launch template <br>&emsp; + Test scaling policies and verify load distribution                                            | 24/09/2025 | 24/09/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

## Results Achieved in Week 2:

### 1. AWS Virtual Private Cloud (VPC)

* **VPC Concept:**
  - VPC is an isolated virtual network within AWS
  - Full control over network configuration
  - Can create multiple VPCs within a Region

* **VPC Architecture:**
  - Understand CIDR blocks and IP addressing
  - Subnets (Public and Private)
  - Route tables and routing
  - Internet Gateway (IGW) for internet connectivity
  - NAT Gateway for private subnets

* **Network Connectivity:**
  - Public subnets: direct path to Internet Gateway
  - Private subnets: no direct internet access, require NAT Gateway
  - Elastic IPs for instances requiring static IP

* **Network Design:**
  - Design VPC before creating resources
  - Consider future scalability
  - How to partition subnets for different layers

### 2. VPC Security and Multi-VPC Features

* **Security Groups:**
  - Virtual firewall for EC2 instances
  - Control inbound and outbound traffic
  - Stateful - only define inbound rules
  - Can reference other Security Groups

* **Network ACLs (Access Control Lists):**
  - Firewall at subnet level
  - Stateless - must define both inbound and outbound
  - Applied to all instances in subnet
  - Order of rules is important

* **VPC Peering:**
  - Connect two VPCs together
  - Instances can communicate via VPC peering
  - Can connect VPCs across different Regions
  - No need for internet gateway or VPN

* **VPC Endpoints:**
  - Gateway endpoints: S3, DynamoDB
  - Interface endpoints: for other AWS services
  - Reduce data transfer costs
  - Increase security by avoiding internet traffic

* **Flow Logs:**
  - Record traffic information in VPC
  - Useful for troubleshooting
  - Can send to CloudWatch Logs or S3

### 3. VPN - DirectConnect - Load Balancer

* **VPN (Virtual Private Network):**
  - Secure connection between on-premise and VPC
  - Site-to-Site VPN: connect office to AWS
  - Client VPN: allow individuals to connect to VPC
  - Encrypt traffic to AWS

* **AWS DirectConnect:**
  - Dedicated connection from on-premise to AWS
  - High bandwidth, low latency
  - Better stability than VPN
  - Higher cost than VPN but suitable for enterprise

* **Load Balancer:**
  - Distribute traffic to multiple instances
  - Improve reliability and availability

  **Load Balancer Types:**
  - **Application Load Balancer (ALB):** Layer 7 (Application) - suitable for web applications
  - **Network Load Balancer (NLB):** Layer 4 (Transport) - extremely fast, for gaming/IoT
  - **Classic Load Balancer (CLB):** Legacy - Layer 4 & 7

  **Health Checks:**
  - Load balancer periodically checks if instance is healthy
  - Automatically removes unhealthy instances
  - Reduces traffic to failed instances

  **Auto Scaling:**
  - Automatically create/delete instances based on demand
  - Combine with Load Balancer to distribute traffic
  - Reduce costs by using only necessary resources

---

## AWS Services Learned:

### Networking Services
* **VPC (Virtual Private Cloud):** Custom network environment
* **Subnets:** Partition VPC into smaller networks
* **Internet Gateway (IGW):** Connect VPC to internet
* **NAT Gateway:** Allow private instances to access internet
* **Route Tables:** Route traffic
* **Security Groups:** Instance-level firewall
* **Network ACLs:** Subnet-level firewall

### VPC Connection Services
* **VPC Peering:** Connect two VPCs
* **VPC Endpoints:** Connect to AWS services without internet
* **VPN (Site-to-Site, Client VPN):** Secure on-premise connection
* **AWS DirectConnect:** Dedicated connection

### Load Balancing & Auto Scaling
* **Elastic Load Balancing (ELB):** Distribute traffic
* **Application Load Balancer (ALB):** Layer 7 balancing
* **Network Load Balancer (NLB):** Layer 4 balancing
* **Auto Scaling:** Automatically scale resources

---

## Resources & References:

* [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/)
* [VPC Security Best Practices](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)
* [AWS VPN Documentation](https://docs.aws.amazon.com/vpn/)
* [AWS DirectConnect Documentation](https://docs.aws.amazon.com/directconnect/)
* [Elastic Load Balancing Documentation](https://docs.aws.amazon.com/elasticloadbalancing/)
* [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/)

---

### Notes:

* **Security:** Always use Security Groups and NACLs following least privilege principle
* **Cost:** DirectConnect is expensive, evaluate carefully before using
* **Design:** Plan VPC thoroughly before creating resources
* **Monitoring:** Use VPC Flow Logs to troubleshoot networking issues