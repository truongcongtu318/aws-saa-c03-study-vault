---
title: "Decision Matrix: Threat Detection & Security Intelligence Tools"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.2: Design secure workloads and applications"
exam_weight: "High"
services:
  - Amazon GuardDuty
  - Amazon Inspector
  - Amazon Macie
  - AWS Security Hub
  - Amazon Detective
tags:
  - decision-matrix
  - guardduty
  - inspector
  - macie
  - security-hub
  - detective
  - threat-detection
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: GuardDuty vs Inspector vs Macie vs Security Hub vs Detective

> Deep Dive chi tiết: [[Threat-Detection-and-Auditing]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Cần giải pháp giám sát an ninh / phát hiện rủi ro]) --> Q1{Mục tiêu bạn muốn tìm kiếm là gì?}

    Q1 -- "Hành vi bất thường, xâm nhập trái phép, đào coin (Threats/Malware)" --> A1["✅ Amazon GuardDuty<br/>(ML phân tích ngầm VPC Flow, DNS, CloudTrail - KHÔNG CẦN AGENT)"]

    Q1 -- "Lỗ hổng phần mềm đã biết (CVE) & hở mạng trên EC2/Container/Lambda" --> A2["✅ Amazon Inspector<br/>(Quét tự động lỗ hổng CVE - Cần SSM Agent trên EC2)"]

    Q1 -- "Dữ liệu nhạy cảm cá nhân PII (Thẻ tín dụng, CCCD) trong S3" --> A3["✅ Amazon Macie<br/>(Quét & phân loại dữ liệu trong S3 bằng ML)"]

    Q1 -- "Bảng điều khiển tập trung xem toàn bộ cảnh báo & Chấm điểm tuân thủ CIS/PCI" --> A4["✅ AWS Security Hub<br/>(Single Pane of Glass gom kết quả từ GuardDuty, Inspector, Macie)"]

    Q1 -- "Vẽ đồ thị tương tác để điều tra nguyên nhân gốc rễ (Root Cause Analysis)" --> A5["✅ Amazon Detective<br/>(Xây dựng mô hình đồ thị quan hệ để điều tra vụ xâm nhập)"]
```

---

## 2. Bảng so sánh ma trận 5 dịch vụ an ninh thông minh

| Tiêu chí | Amazon GuardDuty | Amazon Inspector | Amazon Macie | AWS Security Hub | Amazon Detective |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Phân loại** | **Phát hiện Mối đe dọa (Threat Detection)** | **Quản lý Lỗ hổng (Vulnerability Scanning)** | **Bảo vệ Dữ liệu (Data Privacy / DLP)** | **Quản lý Tư thế An ninh (CSPM & Compliance)** | **Điều tra Sự cố (Incident Investigation)** |
| **Đối tượng phân tích** | - VPC Flow Logs<br/>- DNS Query Logs<br/>- CloudTrail Events<br/>- S3 Data Events<br/>- EKS Audit Logs | - EC2 Instances (Hệ điều hành & packages)<br/>- ECR Container Images<br/>- AWS Lambda Code | Các tệp tin lưu trữ trong **Amazon S3** | Nhận findings từ các dịch vụ AWS khác & Partner | Dữ liệu Graph dựng từ CloudTrail, VPC Flow Logs, GuardDuty |
| **Yêu cầu Agent?** | **KHÔNG** (Agentless hoàn toàn) | **CÓ trên EC2** (SSM Agent); ECR & Lambda không cần | **KHÔNG** | **KHÔNG** | **KHÔNG** |
| **Thời điểm kích hoạt** | Liên tục theo thời gian thực (Real-time) | Định kỳ hoặc khi có image mới push lên ECR | Định kỳ quét (Scheduled) hoặc theo lệnh | Liên tục tổng hợp | Theo yêu cầu điều tra sự cố |
| **Đầu ra chính** | **Threat Findings** (ví dụ: EC2 bị chiếm quyền, gọi ra IP C2) | **CVE Findings** (ví dụ: CVE-2023-38545 cURL vulnerability) | **Data Findings** (ví dụ: phát hiện 10,000 số thẻ tín dụng) | **Compliance Standards** (CIS Benchmark %, PCI-DSS %) | **Graph Visualizations** (Ai đã làm gì, lúc nào, từ đâu) |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Detect anomalous API activity or compromised EC2 instances running crypto mining"* | **Amazon GuardDuty** |
| *"Continuous vulnerability assessment for CVEs on EC2 instances and ECR images"* | **Amazon Inspector** |
| *"Identify and classify personally identifiable information (PII) stored in S3"* | **Amazon Macie** |
| *"Centralized dashboard to aggregate findings and track compliance against CIS benchmarks"*| **AWS Security Hub** |
| *"Investigate the root cause of security findings and visualize interaction graphs"* | **Amazon Detective** |
| *"Agentless threat detection analyzing VPC Flow Logs and DNS query logs"* | **Amazon GuardDuty** |
