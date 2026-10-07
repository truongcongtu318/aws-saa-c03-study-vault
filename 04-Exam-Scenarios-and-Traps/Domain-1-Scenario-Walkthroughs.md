---
title: "Domain 1 Scenario Walkthroughs: 10 Classic SAA-C03 Exam Scenarios with Distractor Analysis"
domain: "Domain 1: Design Secure Architectures"
task_statement: "All Tasks (1.1, 1.2, 1.3)"
exam_weight: "Very High"
tags:
  - scenarios
  - practice-questions
  - distractor-analysis
  - exam-traps
  - domain1
last_updated: 2026-03-30
---

# 🎯 Domain 1 Scenario Walkthroughs: 10 Kịch bản đề thi thực tế & Bẫy Distractor

> Trở về: [[00-Domain-1-MOC]]

---

## 📌 Kịch bản 1: EC2 truy cập S3 không dùng Access Keys (Task 1.1)

### Đề bài (Exam Prompt):
> An application running on Amazon EC2 instances in a private subnet needs to read and write transaction logs to an Amazon S3 bucket. Corporate security compliance prohibits storing long-term credentials on the server file systems. Which solution meets these requirements with the LEAST operational overhead?

- **A.** Create an IAM user with S3 read/write permissions. Store the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` in environment variables on the EC2 instances.
- **B.** Create an IAM role with an attached policy allowing S3 read/write access. Attach this IAM role to an EC2 Instance Profile and assign it to the EC2 instances.
- **C.** Store the AWS credentials in AWS Secrets Manager. Configure an application script to fetch the credentials on boot and authenticate to S3.
- **D.** Configure an Amazon Cognito Identity Pool to generate temporary credentials, and hardcode the Cognito Identity Pool ID in the EC2 application code.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Gán **IAM Role thông qua Instance Profile** cho EC2 là giải pháp chuẩn mực của AWS. AWS Instance Metadata Service (IMDS) sẽ tự động cấp phát và xoay vòng chứng chỉ bảo mật tạm thời (STS temporary credentials) cho máy chủ mà không cần lưu trữ hay quản lý bất kỳ access key dài hạn nào.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Vi phạm trực tiếp yêu cầu không lưu thông tin đăng nhập dài hạn trên file system/biến môi trường máy chủ.
  - **C SAI:** Gây phức tạp hóa không cần thiết (Overkill). Để EC2 đọc được Secrets Manager thì EC2... vẫn cần có quyền xác thực ban đầu!
  - **D SAI:** Amazon Cognito dùng cho mobile và web application hướng tới người dùng cuối, không dùng cho máy chủ backend EC2 nội bộ.

---

## 📌 Kịch bản 2: Tự động xoay mật khẩu Database định kỳ (Task 1.3)

### Đề bài (Exam Prompt):
> A solutions architect is designing an architecture for a web application connected to an Amazon Aurora MySQL database. Company policy requires that database credentials must be rotated automatically every 30 days without downtime. Which solution meets these requirements with the LEAST operational effort?

- **A.** Store database credentials in AWS Systems Manager Parameter Store as a `SecureString`. Create an Amazon EventBridge rule that triggers an AWS Lambda function every 30 days to update the parameter and database password.
- **B.** Store database credentials in AWS Secrets Manager. Enable automatic rotation and configure a built-in rotation Lambda function targeting the Aurora DB cluster with a 30-day schedule.
- **C.** Store database credentials in an S3 bucket encrypted with SSE-KMS. Use an S3 Lifecycle rule to expire the credentials every 30 days.
- **D.** Configure IAM database authentication for Aurora, and disable password-based logins entirely.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **AWS Secrets Manager** cung cấp sẵn tính năng **Automatic Rotation** với template Lambda dựng sẵn chuyên biệt cho Amazon RDS và Aurora. Secrets Manager tự động xử lý đổi pass trên DB và cập nhật secret an toàn không downtime.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Parameter Store không có tính năng auto-rotation tích hợp sẵn; việc tự viết và duy trì Lambda + EventBridge tốn nhiều công sức vận hành hơn (**Operational effort**).
  - **C SAI:** S3 Lifecycle chỉ xóa file object, không thể can thiệp vào cơ sở dữ liệu để thay đổi mật khẩu user.
  - **D SAI:** IAM Database Authentication tuy bảo mật nhưng đề bài yêu cầu cụ thể việc *"rotate database credentials"*, và IAM DB Auth có giới hạn tần suất kết nối mỗi giây (connection rate limits) không phù hợp cho mọi app production lớn.

---

## 📌 Kịch bản 3: Chặn một địa chỉ IP độc hại đang tấn công (Task 1.2)

### Đề bài (Exam Prompt):
> Security monitoring identifies that a single external IP address `203.0.113.88` is launching a brute-force attack against web servers hosted across multiple EC2 instances inside a public subnet. What is the FASTEST way to immediately block traffic from this IP address?

- **A.** Add an inbound DENY rule for `203.0.113.88/32` to the Security Group associated with the EC2 instances.
- **B.** Add an inbound DENY rule for `203.0.113.88/32` with a lower rule number than the ALLOW rule in the Network ACL associated with the subnet.
- **C.** Modify the route table of the subnet to point the route for `203.0.113.88/32` to a blackhole target.
- **D.** Submit a support request to the AWS DDoS Response Team (DRT) to blacklist the attacker's IP address.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Network ACL (NACL)** là firewall cấp Subnet, đánh giá theo số thứ tự (Rule Number) từ nhỏ đến lớn và **hỗ trợ luật DENY**. Tạo một Deny rule với số thứ tự nhỏ hơn (ví dụ: Rule 50 Deny `203.0.113.88/32`, Rule 100 Allow All) sẽ chặn đứng ngay gói tin tại cổng vào của Subnet.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** **Security Group KHÔNG CÓ luật DENY** (chỉ có luật Allow).
  - **C SAI:** Route Table chỉ hỗ trợ trỏ Next Hop tới các target hợp lệ (IGW, NAT, ENI, VGW), không hỗ trợ tính năng blackhole tuyến đường cho một IP đơn lẻ.
  - **D SAI:** DRT chỉ thuộc về AWS Shield Advanced và áp dụng cho tấn công DDoS quy mô lớn, không dùng cho việc chặn 1 IP đơn giản.

---

## 📌 Kịch bản 4: Chặn SQL Injection và XSS cho ứng dụng Web (Task 1.2)

### Đề bài (Exam Prompt):
> A retail company hosts its public website behind an Application Load Balancer (ALB). The company needs to protect the website against common web-layer vulnerabilities, specifically SQL injection (SQLi) and cross-site scripting (XSS), and block malicious web requests from specific countries. Which service should be used?

- **A.** Amazon GuardDuty
- **B.** AWS WAF
- **C.** AWS Shield Advanced
- **D.** AWS Network Firewall

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **AWS WAF** hoạt động ở **Layer 7 (Application Layer)**, gắn trực tiếp vào ALB hoặc CloudFront, cung cấp các bộ luật AWS Managed Rules chống SQLi, XSS, và hỗ trợ Geo-match conditions để chặn truy cập từ các quốc gia được chỉ định.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** GuardDuty là dịch vụ phát hiện mối đe dọa (Detection), nó chỉ cảnh báo sau khi phân tích log chứ không phải là inline reverse proxy để chặn request web thời gian thực.
  - **C SAI:** Shield Advanced bảo vệ Layer 3/4 DDoS và hỗ trợ tài chính, không chuyên xử lý payload SQL Injection/XSS.
  - **D SAI:** Network Firewall hoạt động ở cấp VPC (Layer 3-7 Stateful Packet Inspection), việc cấu hình trên ALB phức tạp hơn nhiều so với việc gắn trực tiếp AWS WAF.

---

## 📌 Kịch bản 5: Ngăn chặn tắt CloudTrail trên toàn bộ tài khoản (Task 1.1)

### Đề bài (Exam Prompt):
> An enterprise uses AWS Organizations with 50 member accounts. The security team mandates that AWS CloudTrail must be enabled in all accounts and Regions, and no individual account administrator (including the root user of any member account) should be able to disable CloudTrail or delete its trails. Which solution achieves this?

- **A.** Create an IAM policy with a `Deny` effect on `cloudtrail:StopLogging` and attach it to the `AdministratorAccess` role in each member account.
- **B.** Create a Service Control Policy (SCP) denying `cloudtrail:StopLogging` and `cloudtrail:DeleteTrail`, and attach the SCP to the Root of the Organization.
- **C.** Configure AWS Config rules in each account to automatically restart CloudTrail whenever it is stopped.
- **D.** Enable AWS Security Hub in the management account to centrally monitor CloudTrail status.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Service Control Policies (SCPs)** áp dụng rào chắn an ninh từ cấp tổ chức xuống toàn bộ Member Accounts. Điểm mấu chốt: **SCP có hiệu lực lên cả Root User của member accounts**. Gắn SCP Deny ở Root của Organization đảm bảo không một ai trong các tài khoản con có thể tắt CloudTrail.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Root User của member account có thể bypass hoặc xóa bỏ bất kỳ IAM Policy nào được cấu hình nội bộ.
  - **C SAI:** Config rule chỉ phát hiện và sửa đổi sau khi sự kiện đã xảy ra (Reactive), không ngăn chặn được hành vi tắt trail ngay từ đầu (Proactive prevention).
  - **D SAI:** Security Hub chỉ là bảng điều khiển hiển thị cảnh báo, không có tính năng cấm hành động.

---

## 📌 Kịch bản 6: Lưu trữ tuân thủ WORM tuyệt đối trong 7 năm (Task 1.3)

### Đề bài (Exam Prompt):
> A financial services company must store regulatory audit records in Amazon S3 for exactly 7 years. The records must adhere to a Write Once, Read Many (WORM) storage model. During this retention window, no user, including administrators and the AWS account root user, must be permitted to overwrite or delete any record. Which S3 feature should be configured?

- **A.** Enable S3 Object Lock in Governance Mode with a 7-year retention period.
- **B.** Enable S3 Object Lock in Compliance Mode with a 7-year retention period.
- **C.** Enable S3 Versioning and configure an S3 Lifecycle policy to transition objects to S3 Glacier with Vault Lock.
- **D.** Enable MFA Delete on the S3 bucket and distribute the MFA hardware token to the compliance officer.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **S3 Object Lock ở Compliance Mode** là tính năng duy nhất cam kết: **Tuyệt đối không một ai (kể cả Root account của AWS)** có thể xóa hoặc ghi đè object trước khi hết thời hạn retention.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Trong Governance Mode, người dùng có quyền `s3:BypassGovernanceRetention` (như Admin/Root) **vẫn có thể xóa** hoặc chỉnh sửa object.
  - **C SAI:** Phức tạp hơn và không trực tiếp ngăn chặn việc xóa object ở tầng S3 Standard trước khi lifecycle kích hoạt.
  - **D SAI:** MFA Delete chỉ yêu cầu mã MFA khi xóa version, Root user có MFA token vẫn có thể xóa được đối tượng.

---

## 📌 Kịch bản 7: Quét và phát hiện số thẻ tín dụng bị lộ trong S3 (Task 1.3)

### Đề bài (Exam Prompt):
> A company suspects that employees are inadvertently uploading unencrypted spreadsheets containing credit card numbers and government IDs (PII) to multiple Amazon S3 buckets. The company needs an automated solution to discover and alert on sensitive data exposure across all S3 buckets. Which service should be used?

- **A.** Amazon Inspector
- **B.** Amazon Macie
- **C.** Amazon GuardDuty
- **D.** AWS IAM Access Analyzer

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon Macie** là dịch vụ bảo mật dữ liệu sử dụng Machine Learning và Natural Language Processing (NLP) để tự động quét, phân loại và gắn cờ các dữ liệu nhạy cảm cá nhân (**PII: Personally Identifiable Information**, số thẻ tín dụng, CMND/hộ chiếu) lưu trữ trong **Amazon S3**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Amazon Inspector quét lỗ hổng phần mềm CVE trên EC2, ECR và Lambda, không quét nội dung file trong S3.
  - **C SAI:** Amazon GuardDuty phát hiện hành vi xâm nhập và mối đe dọa từ log (VPC Flow, CloudTrail, DNS), không đọc nội dung file bảng tính trong S3.
  - **D SAI:** IAM Access Analyzer chỉ kiểm tra quyền truy cập công cộng hoặc chia sẻ ngoài tổ chức của bucket, không phân tích nội dung dữ liệu bên trong file.

---

## 📌 Kịch bản 8: Quản trị EC2 an toàn không mở cổng SSH (Task 1.2)

### Đề bài (Exam Prompt):
> A systems administrator needs to manage and run maintenance scripts on Amazon EC2 instances residing in a private subnet. Corporate security policy dictates that no inbound ports may be opened in security groups or network ACLs, and the instances must not have public IP addresses. Which solution satisfies these security constraints?

- **A.** Deploy an EC2 Bastion host in a public subnet with an Elastic IP, and SSH into the private instances using port forwarding.
- **B.** Attach an IAM role with the `AmazonSSMManagedInstanceCore` policy to the private EC2 instances, install the SSM Agent, and use AWS Systems Manager Session Manager.
- **C.** Create an AWS Client VPN endpoint and connect administrators directly to the private subnet over port 1194.
- **D.** Attach an Internet Gateway to the VPC and configure port 22 inbound access restricted to the administrator's corporate IP.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **AWS Systems Manager Session Manager** cho phép kết nối shell/terminal vào EC2 mà **không cần mở bất kỳ cổng Inbound nào** (cổng 22 đóng), không cần Bastion Host, và không cần Public IP. Giao tiếp được thực hiện thông qua kết nối Outbound an toàn từ SSM Agent tới SSM Service Endpoint.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Vi phạm yêu cầu bảo mật vì phải mở cổng Inbound 22 trên các máy chủ và phải quản lý thêm hạ tầng Bastion Host.
  - **C SAI:** Client VPN đòi hỏi quản lý chứng chỉ, thiết lập kết nối VPN phức tạp và tốn kém chi phí hơn nhiều so với Session Manager.
  - **D SAI:** Vi phạm nghiêm trọng quy định không mở cổng Inbound và không cấp public IP cho máy chủ trong private subnet.

---

## 📌 Kịch bản 9: Tích hợp SaaS bên thứ ba chống Confused Deputy (Task 1.1)

### Đề bài (Exam Prompt):
> A company contracts a third-party SaaS vendor to analyze AWS resource utilization. The vendor will run automated analytics jobs from their own AWS account (`111122223333`) against the company's AWS account (`444455556666`). The security team requires a mechanism to prevent the "confused deputy" vulnerability. Which configuration is required in the IAM trust policy?

- **A.** Add an `IpAddress` condition to restrict access to the vendor's corporate IP range.
- **B.** Add a `StringEquals` condition checking `sts:ExternalId` with a unique passphrase shared between the company and the vendor.
- **C.** Require Multi-Factor Authentication (MFA) via `aws:MultiFactorAuthPresent`.
- **D.** Use an IAM permission boundary on the role to restrict max permissions.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Cơ chế chuẩn của AWS để chống lại tấn công **Confused Deputy Problem** khi chia sẻ IAM Role chéo tài khoản cho bên thứ ba là yêu cầu tham số **`sts:ExternalId`** trong Trust Policy. Vendor phải truyền đúng mã định danh bí mật này khi gọi `sts:AssumeRole`.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** IP của hạ tầng SaaS có thể thay đổi động và không giải quyết được bản chất lỗ hổng Confused Deputy (khi chính SaaS bị kẻ xấu lợi dụng gọi API).
  - **C SAI:** MFA chỉ áp dụng cho người dùng con người đăng nhập, không áp dụng tự động cho các tiến trình script API của vendor SaaS.
  - **D SAI:** Permission Boundary chỉ giới hạn quyền hạn tối đa, không ngăn được việc vendor bị lợi dụng để assume role của khách hàng khác.

---

## 📌 Kịch bản 10: Tiêu chuẩn FIPS 140-2 Level 3 phần cứng chuyên dụng (Task 1.3)

### Đề bài (Exam Prompt):
> A government contractor must store and manage encryption keys for a top-secret database workload. Regulatory mandates strictly require that encryption keys must be generated and managed inside dedicated, single-tenant hardware security modules certified at FIPS 140-2 Level 3. The company must have exclusive, administrator-level control over the cryptographic hardware. Which service satisfies these requirements?

- **A.** AWS Key Management Service (AWS KMS) with Customer Managed Keys
- **B.** AWS CloudHSM
- **C.** AWS Secrets Manager with KMS encryption
- **D.** Amazon S3 with SSE-C (Server-Side Encryption with Customer-Provided Keys)

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **AWS CloudHSM** là dịch vụ duy nhất của AWS cung cấp phần cứng mã hóa chuyên dụng **đơn khách hàng (Single-tenant)** đạt chuẩn **FIPS 140-2 Level 3** mà khách hàng sở hữu toàn quyền quản trị thiết bị độc lập.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** AWS KMS là giải pháp đa khách hàng (Multi-tenant), khách hàng không có quyền quản trị trực tiếp cấp phần cứng HSM.
  - **C SAI:** Secrets Manager chỉ lưu trữ chuỗi bí mật, bản thân nó không phải là thiết bị phần cứng HSM.
  - **D SAI:** SSE-C chỉ là phương thức gửi key qua header HTTP để S3 mã hóa, không cung cấp cơ chế quản lý vòng đời khóa trong module phần cứng chuyên dụng.

---

## 📌 Kịch bản 11: Thiết lập Landing Zone tự động và Phân biệt Guardrails (Task 1.1)

### Đề bài (Exam Prompt):
> An enterprise with expanding cloud operations needs to establish a secure, compliant multi-account AWS architecture. The compliance team mandates that:
> 1. All member accounts must adhere to mandatory security baselines, with policies that strictly prevent any account administrator from disabling CloudTrail or altering centralized audit logs.
> 2. The security operations team must automatically detect any non-compliant resources (such as unencrypted EBS volumes) post-deployment without blocking development teams from launching workloads.
> 3. New AWS accounts must be provisioned through a standardized self-service portal with pre-configured network baselines and governance controls.
> Which AWS service combination achieves these goals with the LEAST operational overhead?

- **A.** Use AWS Organizations manually, write custom IAM policies for each account, and configure Amazon CloudWatch alarms.
- **B.** Deploy AWS Control Tower, enforce preventive guardrails (SCPs) to protect audit logging, configure detective guardrails (AWS Config rules) to monitor encryption compliance, and use Account Factory for provisioning new accounts.
- **C.** Create an AWS CloudFormation StackSet deployed from the management account to configure VPCs and IAM roles in member accounts manually.
- **D.** Deploy AWS Systems Manager across all accounts, configure Patch Manager, and use AWS Config Conformance Packs without AWS Organizations.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** 
  - **AWS Control Tower** là giải pháp số một để thiết lập **Landing Zone** hoàn chỉnh với **ít công sức vận hành nhất (Least operational overhead)**.
  - **Preventive Guardrails (sử dụng SCPs)** đáp ứng yêu cầu 1: **CHẶN ĐỨNG** các hành động tắt CloudTrail hoặc sửa log ở cấp độ tổ chức.
  - **Detective Guardrails (sử dụng AWS Config)** đáp ứng yêu cầu 2: **CHO PHÉP TẠO** nhưng tự động phát hiện và cảnh báo các tài nguyên không tuân thủ quy định (EBS unencrypted).
  - **Account Factory (tích hợp AWS Service Catalog)** đáp ứng yêu cầu 3: cung cấp cổng tự phục vụ cấp phát tài khoản mới đã cài sẵn Guardrails và VPC baseline.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Cấu hình thủ công từng IAM policy và CloudWatch alarms tốn rất nhiều công sức vận hành (**High operational overhead**) và IAM policy không thể ngăn được root user của member account.
  - **C SAI:** CloudFormation StackSet là công cụ cấp thấp, không cung cấp sẵn kiến trúc Landing Zone hoàn chỉnh, thiếu cơ chế Guardrails và Account Factory tự phục vụ.
  - **D SAI:** Bỏ qua AWS Organizations và Control Tower sẽ làm mất khả năng quản trị đa tài khoản tập trung và không thực thi được các rào chắn phòng ngừa (Preventive SCPs).
