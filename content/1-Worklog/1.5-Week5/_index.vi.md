---
title: "Worklog Tuần 5"
date: "2025-11-30"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Nắm vững AWS security và access management fundamentals.
* Hiểu Identity and Access Management (IAM) best practices.
* Tìm hiểu về authentication và authorization mechanisms.
* Triển khai security monitoring và compliance strategies.
* Thiết kế secure multi-account AWS architectures.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 1   | - Ôn tập kiến trúc tuần 1-4 và security requirements <br> - Lập kế hoạch security infrastructure tuần 5 <br> - Chuẩn bị IAM và security environment                                                                                                                                                                                                                      | 15/10/2025   | 15/10/2025      | Ghi chép Tuần 1-4                         |
| 2   | - Tìm hiểu Shared Responsibility Model: <br>&emsp; + AWS responsibility cho infrastructure security <br>&emsp; + Customer responsibility cho data và applications <br>&emsp; + Shared controls <br>&emsp; + Security trong các service models khác nhau                                                                                                                  | 16/10/2025   | 16/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu IAM (Identity and Access Management): <br>&emsp; + IAM users, groups, roles, policies <br>&emsp; + Policy documents và permissions <br>&emsp; + Cross-account access <br>&emsp; + IAM best practices <br>&emsp; + Service control policies (SCPs)                                                                                                             | 17/10/2025   | 17/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Security & Compliance Services: <br>&emsp; + Amazon Cognito cho authentication <br>&emsp; + AWS Organization cho multi-account management <br>&emsp; + AWS Identity Center cho centralized access <br>&emsp; + AWS Key Management Service (KMS) <br>&emsp; + AWS Security Hub cho security monitoring                                                         | 18/10/2025   | 18/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Bài thực hành 1 - IAM Configuration:** <br>&emsp; + Tạo IAM users với specific permissions <br>&emsp; + Tạo IAM groups và assign policies <br>&emsp; + Tạo IAM roles cho services <br>&emsp; + Triển khai least privilege access <br>&emsp; + Test cross-account role assumption                                                                                     | 19/10/2025   | 19/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Bài thực hành 2 - Cognito & Authentication:** <br>&emsp; + Tạo Cognito user pool <br>&emsp; + Cấu hình user sign-up và sign-in <br>&emsp; + Thiết lập multi-factor authentication (MFA) <br>&emsp; + Tạo identity pool cho federated access <br>&emsp; + Tích hợp Cognito với application                                                                            | 20/10/2025   | 20/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - **Bài thực hành 3 - Security Monitoring & Organization:** <br>&emsp; + Thiết lập AWS Organization với multiple accounts <br>&emsp; + Cấu hình AWS Identity Center cho SSO <br>&emsp; + Bật CloudTrail cho audit logging <br>&emsp; + Thiết lập AWS Security Hub <br>&emsp; + Cấu hình KMS key policies và encryption <br>&emsp; + Tạo security alerts trong CloudWatch | 21/10/2025   | 21/10/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

## Kết Quả Đạt Được Tuần 5:

### 1. Shared Responsibility Model

* **AWS Responsibility:**
  - Infrastructure security (regions, AZs, data centers)
  - Physical security của facilities
  - Network và hypervisor security
  - Storage infrastructure security
  - Managed service security (RDS, S3, Lambda, v.v.)

* **Customer Responsibility:**
  - Data encryption và management
  - Application security
  - OS và patch management
  - Firewall và network configuration
  - Access control và authentication
  - Data integrity và backup

* **Shared Controls:**
  - Patch management
  - Configuration management
  - Awareness và training
  - Compliance monitoring

### 2. Amazon Identity and Access Management (IAM)

* **IAM Core Concepts:**
  - IAM Users: individual accounts với login credentials
  - IAM Groups: collection của users với shared permissions
  - IAM Roles: temporary credentials cho services/users
  - IAM Policies: JSON documents định nghĩa permissions
  - IAM Trust relationships: control ai có thể assume roles

* **Policy Structure:**
  - Effect: Allow hoặc Deny
  - Action: specific API calls (ví dụ: s3:GetObject)
  - Resource: ARN của resource (ví dụ: arn:aws:s3:::bucket/*)
  - Conditions: optional conditions cho policy

* **Best Practices:**
  - Least privilege access principle
  - Sử dụng roles thay vì access keys cho applications
  - Bật MFA cho root account và privileged users
  - Rotate access keys thường xuyên
  - Sử dụng service roles cho cross-account access
  - Monitor IAM activity với CloudTrail

* **Cross-Account Access:**
  - Tạo role trong account B
  - Cho phép account A assume role
  - Users trong account A có thể temporarily assume role trong account B
  - Hữu ích cho multi-account architectures

### 3. Amazon Cognito

* **Cognito User Pools:**
  - Managed user directory và authentication service
  - User sign-up và sign-in
  - Password policies và complexity requirements
  - Multi-factor authentication (MFA) - SMS, TOTP, email
  - User attribute customization
  - Temporary JWT tokens cho API access

* **Cognito Identity Pools:**
  - Federated identity provider
  - Temporary AWS credentials cho unauthenticated/authenticated users
  - Hỗ trợ external identity providers (Facebook, Google, SAML)
  - Fine-grained access control tới AWS resources
  - Cross-account và cross-service access

* **Security Features:**
  - Account takeover protection
  - Compromised credentials handling
  - Risk-based adaptive authentication
  - Device tracking và trusted devices

### 4. AWS Organization

* **Organization Structure:**
  - Root: parent container cho tất cả accounts
  - Organizational Units (OUs): grouping của accounts
  - Policies: service control policies (SCPs) cho permission boundaries
  - Centralized billing và cost tracking

* **Service Control Policies (SCPs):**
  - Permission boundaries ở organization level
  - Whitelist/blacklist approach tới permissions
  - Applied cho accounts và OUs
  - Restrict cái gì ngay cả root user có thể làm
  - Không grant permissions, chỉ restrict

* **Multi-Account Strategy:**
  - Separate accounts cho environments (dev, prod)
  - Separate accounts cho different departments
  - Centralized logging và monitoring
  - Improved security và blast radius containment

### 5. AWS Identity Center

* **Single Sign-On (SSO):**
  - Centralized access management across accounts
  - Users sign in một lần, access tất cả authorized accounts
  - Hỗ trợ SAML 2.0 và OIDC
  - Integration với external identity providers

* **Features:**
  - User và group management
  - Permission set assignments
  - Multi-account access
  - Application access portal
  - Audit logging với CloudTrail

### 6. Amazon Key Management Service (KMS)

* **Key Management:**
  - Customer Master Keys (CMKs) cho encryption
  - AWS managed keys vs customer managed keys
  - Key policies cho access control
  - Key rotation cho compliance

* **Encryption:**
  - Data encryption at rest (S3, EBS, RDS)
  - Data encryption in transit (TLS/SSL)
  - Envelope encryption cho large data
  - Decrypt chỉ với proper key access

### 7. AWS Security Hub

* **Security Monitoring:**
  - Centralized security findings từ AWS services
  - Integration với GuardDuty, Inspector, Macie
  - Compliance standards (CIS, PCI-DSS, HIPAA)
  - Custom insights và dashboards
  - Automated remediation workflows

* **Finding Management:**
  - Critical, high, medium, low severity findings
  - Investigation và remediation tracking
  - Integration với third-party tools
  - SIEM integration

### 8. Additional Security Services

* **AWS CloudTrail:**
  - API audit logging cho tất cả AWS services
  - Track ai làm cái gì, khi nào, và từ đâu
  - Compliance và forensic analysis
  - Integration với CloudWatch và S3

* **Amazon GuardDuty:**
  - Threat detection sử dụng machine learning
  - Detection của unusual API calls, malware
  - Integration với Security Hub
  - Automated response tới findings

* **AWS Config:**
  - Track AWS resource configurations
  - Compliance auditing
  - Configuration change tracking
  - Automated compliance checking

---

## Các Dịch Vụ AWS Học Được:

### Identity & Access Management
* **IAM (Identity and Access Management):** User, group, role, và policy management
* **Cognito:** User authentication và federated identity management
* **Identity Center:** Centralized SSO across multiple accounts

### Security & Compliance
* **KMS (Key Management Service):** Encryption key management
* **Security Hub:** Centralized security findings và compliance
* **CloudTrail:** API audit logging

### Organization & Multi-Account
* **AWS Organization:** Multi-account management và governance
* **Service Control Policies:** Permission boundaries ở organization level

---

## Các Lệnh & Tài Nguyên Chính:

**IAM CLI Commands:**
```bash
# Tạo IAM user
aws iam create-user --user-name john.doe

# Tạo access key cho user
aws iam create-access-key --user-name john.doe

# Attach policy cho user
aws iam attach-user-policy \
  --user-name john.doe \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Tạo IAM role
aws iam create-role \
  --role-name lambda-execution-role \
  --assume-role-policy-document file://trust-policy.json

# Tạo IAM policy
aws iam create-policy \
  --policy-name my-custom-policy \
  --policy-document file://policy.json

# Liệt kê IAM users
aws iam list-users

# Bật MFA cho user
aws iam enable-mfa-device \
  --user-name john.doe \
  --serial-number arn:aws:iam::123456789012:mfa/john-device \
  --authentication-code1 123456 \
  --authentication-code2 654321
```

**Cognito CLI Commands:**
```bash
# Tạo user pool
aws cognito-idp create-user-pool \
  --pool-name my-pool \
  --policies file://password-policy.json

# Tạo user pool client
aws cognito-idp create-user-pool-client \
  --user-pool-id us-east-1_xxxxx \
  --client-name my-app

# Tạo Cognito user
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
# Tạo organization
aws organizations create-organization --feature-set ALL

# Tạo organizational unit
aws organizations create-organizational-unit \
  --parent-id r-xxxxx \
  --name Production

# Tạo AWS account
aws organizations create-account \
  --email production@example.com \
  --account-name Production

# Liệt kê accounts
aws organizations list-accounts

# Attach policy cho account
aws organizations attach-policy \
  --policy-id p-xxxxx \
  --target-id 123456789012
```

**Security Hub CLI Commands:**
```bash
# Tạo Security Hub
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
# Tạo KMS key
aws kms create-key \
  --description "My encryption key"

# Tạo alias cho key
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

## Ví Dụ Policies:

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

## Tài Nguyên & Tham Khảo:

* [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
* [Amazon Cognito Documentation](https://docs.aws.amazon.com/cognito/)
* [AWS Organizations Documentation](https://docs.aws.amazon.com/organizations/)
* [AWS KMS Documentation](https://docs.aws.amazon.com/kms/)
* [AWS Security Hub Documentation](https://docs.aws.amazon.com/securityhub/)
* [AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/)
* [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

---

### Ghi Chú:

* **Security First:** Luôn theo least privilege principle
* **MFA Protection:** Bật MFA cho tất cả human users
* **Audit Logging:** Bật CloudTrail cho tất cả accounts
* **Key Rotation:** Triển khai regular KMS key rotation
* **Monitoring:** Sử dụng Security Hub cho continuous compliance monitoring
* **Documentation:** Ghi chép tất cả IAM policies và access patterns