---
title: "Week 5 Worklog"
date: "2025-11-30"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Master AWS security and access management fundamentals.
* Understand Identity and Access Management (IAM) best practices.
* Learn about authentication and authorization mechanisms.
* Implement security monitoring and compliance strategies.
* Design secure multi-account AWS architectures.

---

### Tasks to be deployed this week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                    | Start Date | End Date   | Resources                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | ---------- | ----------------------------------------- |
| 1   | - Review Week 1-4 architecture and security requirements <br> - Plan security infrastructure for Week 5 <br> - Prepare IAM and security environment                                                                                                                                                                                                                      | 15/10/2025 | 15/10/2025 | Week 1-4 Notes                            |
| 2   | - Learn Shared Responsibility Model: <br>&emsp; + AWS responsibility for infrastructure security <br>&emsp; + Customer responsibility for data and applications <br>&emsp; + Shared controls <br>&emsp; + Security in different service models                                                                                                                           | 16/10/2025 | 16/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn IAM (Identity and Access Management): <br>&emsp; + IAM users, groups, roles, policies <br>&emsp; + Policy documents and permissions <br>&emsp; + Cross-account access <br>&emsp; + IAM best practices <br>&emsp; + Service control policies (SCPs)                                                                                                               | 17/10/2025 | 17/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Security & Compliance Services: <br>&emsp; + Amazon Cognito for authentication <br>&emsp; + AWS Organization for multi-account management <br>&emsp; + AWS Identity Center for centralized access <br>&emsp; + AWS Key Management Service (KMS) <br>&emsp; + AWS Security Hub for security monitoring                                                            | 18/10/2025 | 18/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Hands-on Lab 1 - IAM Configuration:** <br>&emsp; + Create IAM users with specific permissions <br>&emsp; + Create IAM groups and assign policies <br>&emsp; + Create IAM roles for services <br>&emsp; + Implement least privilege access <br>&emsp; + Test cross-account role assumption                                                                            | 19/10/2025 | 19/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Hands-on Lab 2 - Cognito & Authentication:** <br>&emsp; + Create Cognito user pool <br>&emsp; + Configure user sign-up and sign-in <br>&emsp; + Set up multi-factor authentication (MFA) <br>&emsp; + Create identity pool for federated access <br>&emsp; + Integrate Cognito with application                                                                      | 20/10/2025 | 20/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Hands-on Lab 3 - Security Monitoring & Organization:** <br>&emsp; + Set up AWS Organization with multiple accounts <br>&emsp; + Configure AWS Identity Center for SSO <br>&emsp; + Enable CloudTrail for audit logging <br>&emsp; + Set up AWS Security Hub <br>&emsp; + Configure KMS key policies and encryption <br>&emsp; + Create security alerts in CloudWatch | 21/10/2025 | 21/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

---

## Results Achieved in Week 5:

### 1. Shared Responsibility Model

* **AWS Responsibility:**
  - Infrastructure security (regions, AZs, data centers)
  - Physical security of facilities
  - Network and hypervisor security
  - Storage infrastructure security
  - Managed service security (RDS, S3, Lambda, etc.)

* **Customer Responsibility:**
  - Data encryption and management
  - Application security
  - OS and patch management
  - Firewall and network configuration
  - Access control and authentication
  - Data integrity and backup

* **Shared Controls:**
  - Patch management
  - Configuration management
  - Awareness and training
  - Compliance monitoring

### 2. Amazon Identity and Access Management (IAM)

* **IAM Core Concepts:**
  - IAM Users: individual accounts with login credentials
  - IAM Groups: collection of users with shared permissions
  - IAM Roles: temporary credentials for services/users
  - IAM Policies: JSON documents defining permissions
  - IAM Trust relationships: control who can assume roles

* **Policy Structure:**
  - Effect: Allow or Deny
  - Action: specific API calls (e.g., s3:GetObject)
  - Resource: ARN of resource (e.g., arn:aws:s3:::bucket/*)
  - Conditions: optional conditions for policy

* **Best Practices:**
  - Least privilege access principle
  - Use roles instead of access keys for applications
  - Enable MFA for root account and privileged users
  - Rotate access keys regularly
  - Use service roles for cross-account access
  - Monitor IAM activity with CloudTrail

* **Cross-Account Access:**
  - Create role in account B
  - Allow account A to assume role
  - Users in account A can temporarily assume role in account B
  - Useful for multi-account architectures

### 3. Amazon Cognito

* **Cognito User Pools:**
  - Managed user directory and authentication service
  - User sign-up and sign-in
  - Password policies and complexity requirements
  - Multi-factor authentication (MFA) - SMS, TOTP, email
  - User attribute customization
  - Temporary JWT tokens for API access

* **Cognito Identity Pools:**
  - Federated identity provider
  - Temporary AWS credentials for unauthenticated/authenticated users
  - Supports external identity providers (Facebook, Google, SAML)
  - Fine-grained access control to AWS resources
  - Cross-account and cross-service access

* **Security Features:**
  - Account takeover protection
  - Compromised credentials handling
  - Risk-based adaptive authentication
  - Device tracking and trusted devices

### 4. AWS Organization

* **Organization Structure:**
  - Root: parent container for all accounts
  - Organizational Units (OUs): grouping of accounts
  - Policies: service control policies (SCPs) for permission boundaries
  - Centralized billing and cost tracking

* **Service Control Policies (SCPs):**
  - Permission boundaries at organization level
  - Whitelist/blacklist approach to permissions
  - Applied to accounts and OUs
  - Restrict what even root user can do
  - Does not grant permissions, only restricts

* **Multi-Account Strategy:**
  - Separate accounts for environments (dev, prod)
  - Separate accounts for different departments
  - Centralized logging and monitoring
  - Improved security and blast radius containment

### 5. AWS Identity Center

* **Single Sign-On (SSO):**
  - Centralized access management across accounts
  - Users sign in once, access all authorized accounts
  - Support for SAML 2.0 and OIDC
  - Integration with external identity providers

* **Features:**
  - User and group management
  - Permission set assignments
  - Multi-account access
  - Application access portal
  - Audit logging with CloudTrail

### 6. Amazon Key Management Service (KMS)

* **Key Management:**
  - Customer Master Keys (CMKs) for encryption
  - AWS managed keys vs customer managed keys
  - Key policies for access control
  - Key rotation for compliance

* **Encryption:**
  - Data encryption at rest (S3, EBS, RDS)
  - Data encryption in transit (TLS/SSL)
  - Envelope encryption for large data
  - Decrypt only with proper key access

### 7. AWS Security Hub

* **Security Monitoring:**
  - Centralized security findings from AWS services
  - Integration with GuardDuty, Inspector, Macie
  - Compliance standards (CIS, PCI-DSS, HIPAA)
  - Custom insights and dashboards
  - Automated remediation workflows

* **Finding Management:**
  - Critical, high, medium, low severity findings
  - Investigation and remediation tracking
  - Integration with third-party tools
  - SIEM integration

### 8. Additional Security Services

* **AWS CloudTrail:**
  - API audit logging for all AWS services
  - Track who did what, when, and from where
  - Compliance and forensic analysis
  - Integration with CloudWatch and S3

* **Amazon GuardDuty:**
  - Threat detection using machine learning
  - Detection of unusual API calls, malware
  - Integration with Security Hub
  - Automated response to findings

* **AWS Config:**
  - Track AWS resource configurations
  - Compliance auditing
  - Configuration change tracking
  - Automated compliance checking

---

## AWS Services Learned:

### Identity & Access Management
* **IAM (Identity and Access Management):** User, group, role, and policy management
* **Cognito:** User authentication and federated identity management
* **Identity Center:** Centralized SSO across multiple accounts

### Security & Compliance
* **KMS (Key Management Service):** Encryption key management
* **Security Hub:** Centralized security findings and compliance
* **CloudTrail:** API audit logging

### Organization & Multi-Account
* **AWS Organization:** Multi-account management and governance
* **Service Control Policies:** Permission boundaries at organization level

---

## Key Commands & Resources:

**IAM CLI Commands:**
```bash
# Create IAM user
aws iam create-user --user-name john.doe

# Create access key for user
aws iam create-access-key --user-name john.doe

# Attach policy to user
aws iam attach-user-policy \
  --user-name john.doe \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Create IAM role
aws iam create-role \
  --role-name lambda-execution-role \
  --assume-role-policy-document file://trust-policy.json

# Create IAM policy
aws iam create-policy \
  --policy-name my-custom-policy \
  --policy-document file://policy.json

# List IAM users
aws iam list-users

# Enable MFA for user
aws iam enable-mfa-device \
  --user-name john.doe \
  --serial-number arn:aws:iam::123456789012:mfa/john-device \
  --authentication-code1 123456 \
  --authentication-code2 654321
```

**Cognito CLI Commands:**
```bash
# Create user pool
aws cognito-idp create-user-pool \
  --pool-name my-pool \
  --policies file://password-policy.json

# Create user pool client
aws cognito-idp create-user-pool-client \
  --user-pool-id us-east-1_xxxxx \
  --client-name my-app

# Create Cognito user
aws cognito-idp admin-create-user \
  --user-pool-id us-east-1_xxxxx \
  --username john.doe \
  --temporary-password TempPassword123!

# Sign in user (get tokens)
aws cognito-idp admin-initiate-auth \
  --user-pool-id us-east-1_xxxxx \
  --client-id xxxxx \
  --auth-flow ADMIN_NO_SRP_AUTH \
  --auth-parameters USERNAME=john.doe,PASSWORD=NewPassword123!
```

**Organization CLI Commands:**
```bash
# Create organization
aws organizations create-organization --feature-set ALL

# Create organizational unit
aws organizations create-organizational-unit \
  --parent-id r-xxxxx \
  --name Production

# Create AWS account
aws organizations create-account \
  --email production@example.com \
  --account-name Production

# List accounts
aws organizations list-accounts

# Attach policy to account
aws organizations attach-policy \
  --policy-id p-xxxxx \
  --target-id 123456789012
```

**Security Hub CLI Commands:**
```bash
# Create Security Hub
aws securityhub enable-security-hub

# Get findings
aws securityhub get-findings \
  --filters '{"RecordState":[{"Value":"ACTIVE","Comparison":"EQUALS"}]}'

# Update finding workflow
aws securityhub update-findings \
  --finding-identifiers file://finding-ids.json \
  --workflow Status=RESOLVED
```

**KMS CLI Commands:**
```bash
# Create KMS key
aws kms create-key \
  --description "My encryption key"

# Create alias for key
aws kms create-alias \
  --alias-name alias/my-key \
  --target-key-id key-id

# Encrypt data
aws kms encrypt \
  --key-id alias/my-key \
  --plaintext fileb://data.txt \
  --output text --query CiphertextBlob

# Decrypt data
aws kms decrypt \
  --ciphertext-blob fileb://encrypted-data \
  --output text --query Plaintext
```

---

## Example Policies:

**S3 Least Privilege Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    }
  ]
}
```

**Cross-Account Trust Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111111111111:role/SourceRole"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

## Resources & References:

* [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
* [Amazon Cognito Documentation](https://docs.aws.amazon.com/cognito/)
* [AWS Organizations Documentation](https://docs.aws.amazon.com/organizations/)
* [AWS KMS Documentation](https://docs.aws.amazon.com/kms/)
* [AWS Security Hub Documentation](https://docs.aws.amazon.com/securityhub/)
* [AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/)
* [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

---

### Notes:

* **Security First:** Always follow least privilege principle
* **MFA Protection:** Enable MFA for all human users
* **Audit Logging:** Enable CloudTrail for all accounts
* **Key Rotation:** Implement regular KMS key rotation
* **Monitoring:** Use Security Hub for continuous compliance monitoring
* **Documentation:** Document all IAM policies and access patterns