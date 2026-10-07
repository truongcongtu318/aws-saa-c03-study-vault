---
title: "Domain 1 Keyword Triggers: Direct Exam Terminology to Architecture Solution Mapping"
domain: "Domain 1: Design Secure Architectures"
task_statement: "All Tasks (1.1, 1.2, 1.3)"
exam_weight: "Very High"
tags:
  - keywords
  - triggers
  - cheat-sheet
  - rapid-recall
  - domain1
last_updated: 2026-03-30
---

# ⚡ Domain 1 Keyword Triggers: Bảng Tra Cứu Từ Khóa Phản Xạ Nhanh

> Trở về: [[00-Domain-1-MOC]]  
> **Cách sử dụng:** Khi đọc câu hỏi trong phòng thi, quét tìm các cụm từ khóa (keywords) dưới đây để loại ngay các phương án sai và nhận diện đáp án đúng trong vòng 15 giây.

---

## 1. Identity, Access & Governance (Task 1.1)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Temporary credentials for EC2 without hardcoding keys"* | **IAM Role attached to EC2 Instance Profile** |
| *"Grant temporary access across separate AWS accounts"* | **AWS STS AssumeRole (Cross-account IAM Role)** |
| *"Prevent confused deputy problem with 3rd-party vendor"* | **`sts:ExternalId` in IAM Role Trust Policy** |
| *"Centralized access management across multiple AWS accounts"* | **AWS IAM Identity Center (formerly AWS SSO)** |
| *"Integrate with on-prem Active Directory WITHOUT storing users in AWS"* | **AWS Directory Service AD Connector** |
| *"Two-way forest trust with on-premises Active Directory"* | **AWS Managed Microsoft AD** |
| *"User registration, login, MFA for mobile/web app"* | **Amazon Cognito User Pools (CUP)** |
| *"Exchange external tokens for temporary AWS credentials to access S3"* | **Amazon Cognito Identity Pools (CIP)** |
| *"Organization-wide guardrail, deny action even for Root user"* | **Service Control Policies (SCPs) in AWS Organizations** |
| *"Delegate IAM role creation to developers without privilege escalation"* | **IAM Permissions Boundary** |
| *"Share VPC subnets or Transit Gateway across accounts without duplication"*| **AWS Resource Access Manager (AWS RAM)** |
| *"Set up a multi-account environment (Landing Zone) with least operational overhead"*| **AWS Control Tower** |
| *"Prevent actions organization-wide using preventive guardrails"* | **AWS Control Tower Preventive Guardrails (SCPs)** |
| *"Detect non-compliant resources after deployment without blocking creation"*| **AWS Control Tower Detective Guardrails (AWS Config)** |
| *"Centrally aggregate CloudTrail logs into a dedicated, tamper-proof account"*| **AWS Control Tower Log Archive Account** |
| *"Self-service portal to provision new AWS accounts with pre-configured baselines"*| **AWS Control Tower Account Factory (AWS Service Catalog)** |

---

## 2. Network Isolation & Edge Defense (Task 1.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Stateful firewall at the instance/ENI level, allow rules only"* | **Security Group** |
| *"Stateless firewall at subnet level, support allow and deny"* | **Network ACL (NACL)** |
| *"Block a single malicious IP address immediately"* | **NACL Inbound DENY rule with low rule number** |
| *"Protect from SQL injection (SQLi) and Cross-Site Scripting (XSS)"* | **AWS WAF attached to ALB or CloudFront** |
| *"DDoS protection with dedicated 24/7 Response Team (DRT) & Cost Protection"*| **AWS Shield Advanced** |
| *"Inspect outbound traffic to approved domain names (FQDN) in VPC"* | **AWS Network Firewall (Stateful domain list)** |
| *"Centrally deploy WAF and Security Group rules across 100 accounts"* | **AWS Firewall Manager** |
| *"Connect EC2 to S3/DynamoDB privately with zero cost and no NAT Gateway"*| **VPC Gateway Endpoint (Route Table)** |
| *"Connect EC2 to KMS/Secrets Manager privately via private IP"* | **VPC Interface Endpoint (AWS PrivateLink / ENI)** |
| *"Secure SSH/RDP terminal to private EC2 without bastion host or open port 22"*| **AWS Systems Manager Session Manager** |

---

## 3. Cryptography & Data Protection (Task 1.3)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Rotate database credentials every 30 days automatically"* | **AWS Secrets Manager (built-in Lambda rotation)** |
| *"Store application config strings hierarchically at lowest cost"* | **AWS Systems Manager Parameter Store (Standard)** |
| *"Dedicated single-tenant hardware module certified FIPS 140-2 Level 3"* | **AWS CloudHSM** |
| *"Multi-region active-active database encryption without cross-region calls"*| **AWS KMS Multi-Region Keys** |
| *"Audit trail of who used the encryption key via CloudTrail"* | **KMS Customer Managed Key (CMK)** |
| *"Reduce KMS API charges for S3 bucket encryption by 99%"* | **Enable S3 Bucket Keys** |
| *"Write Once, Read Many (WORM) where even root cannot delete objects"*| **Amazon S3 Object Lock in Compliance Mode** |
| *"WORM storage where administrators can bypass retention if needed"* | **Amazon S3 Object Lock in Governance Mode** |
| *"Enforce HTTPS in-transit encryption for all S3 requests"* | **S3 Bucket Policy with `aws:SecureTransport: false` DENY** |
| *"Free public SSL/TLS certificate with automatic DNS renewal"* | **AWS Certificate Manager (ACM) with Route 53 validation** |

---

## 4. Threat Detection, Auditing & Compliance (Task 1.2/1.3)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Detect cryptocurrency mining or compromised instances using ML without agent"*| **Amazon GuardDuty** |
| *"Automated CVE vulnerability assessment on EC2 instances and ECR images"*| **Amazon Inspector** |
| *"Discover and protect sensitive data (PII, credit cards) stored in S3"*| **Amazon Macie** |
| *"Centralized dashboard aggregating findings and tracking CIS compliance"*| **AWS Security Hub** |
| *"Conduct root-cause analysis and visualize attack graph models"* | **Amazon Detective** |
| *"Prove S3 audit log files have not been modified or tampered with"* | **CloudTrail Log File Integrity Validation** |
| *"Download AWS official compliance reports (SOC, PCI-DSS, ISO)"* | **AWS Artifact** |
| *"Automate evidence collection for internal compliance audit readiness"* | **AWS Audit Manager** |
