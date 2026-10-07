---
title: "Decision Matrix: AWS Organizations vs AWS Control Tower"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.1: Design secure access to AWS resources"
exam_weight: "High"
services:
  - AWS Organizations
  - AWS Control Tower
tags:
  - decision-matrix
  - organizations
  - control-tower
  - landing-zone
  - multi-account
  - comparison
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: AWS Organizations vs AWS Control Tower

> Deep Dive chi tiết: [[AWS-Control-Tower-and-Landing-Zone]] | [[AWS-Organizations-and-SCPs]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Thiết kế quản trị môi trường Đa tài khoản Multi-Account]) --> Q1{Bạn muốn thiết lập môi trường Landing Zone chuẩn chỉnh<br/>với gom log tập trung, phân quyền SSO, Guardrails sẵn có<br/>với ÍT CÔNG SỨC VẬN HÀNH NHẤT (Least Operational Overhead)?}

    Q1 -- "CÓ (Khuyên dùng số 1 cho Doanh nghiệp)" --> A1["✅ AWS Control Tower<br/>(Dịch vụ Orchestrator cấp cao: Tự động dựng Landing Zone,<br/>Account Factory, 3 loại Guardrails, Log Archive Account)"]

    Q1 -- "KHÔNG (Chỉ cần tính năng thanh toán gộp hoặc muốn tự code 100% bằng Terraform)" --> Q2{Mục tiêu chính là gì?}

    Q2 -- "Chỉ cần thanh toán tập trung (Consolidated Billing) hoặc chia sẻ OU đơn giản" --> A2["✅ AWS Organizations (Thủ công)<br/>(Hạ tầng cốt lõi, tự tạo OU, tự viết SCPs, tự cấu hình Config)"]
    Q2 -- "Doanh nghiệp có framework tự động hóa IaC phức tạp tự phát triển riêng" --> A2
```

---

## 2. Bảng so sánh trực diện 10 tiêu chí kỹ thuật

| Tiêu chí | AWS Organizations (Thuần túy) | AWS Control Tower |
| :--- | :--- | :--- |
| **Bản chất dịch vụ** | **Dịch vụ hạ tầng nền tảng (Foundation)** | **Dịch vụ tự động hóa cấp cao (Managed Orchestrator)** |
| **Công sức thiết lập** | **Thủ công hoàn toàn (High Overhead):** Phải tự tạo từng OU, tự viết từng SCP, tự cấu hình SSO, tự tạo S3 bucket gom log | **Tự động hóa hoàn toàn (Least Overhead):** Dựng xong toàn bộ Landing Zone trong vài cú click chuột (~30 phút) |
| **Cấu trúc tài khoản mặc định** | Chỉ tạo 1 Management Account | Tự động tạo sẵn: **Management Account, Log Archive Account, Audit Account** |
| **Cấp phát tài khoản mới** | Gọi API `CreateAccount` thủ công, phải tự vào cấu hình mạng và bảo mật | **Account Factory (Self-Service):** Tự động kế thừa VPC chuẩn, Guardrails, và SSO |
| **Cơ chế rào chắn (Guardrails)**| Chỉ hỗ trợ **Service Control Policies (SCPs)** | Tích hợp sẵn cả 3 loại: **Preventive (SCPs) + Detective (Config) + Proactive (CFN Guard)** |
| **Quản lý danh tính (SSO)** | Phải tự vào cấu hình IAM Identity Center từ đầu | **Tự động tích hợp sẵn** với IAM Identity Center và cấu hình quyền truy cập mẫu |
| **Gom log tập trung** | Phải tự tạo S3 Bucket, tự viết KMS Key Policy và cấu hình CloudTrail đa tài khoản | **Tự động bật CloudTrail & Config đa tài khoản** đổ dồn vào Log Archive S3 bucket |
| **Bảng điều khiển tuân thủ (Dashboard)**| Không có bảng điều khiển trạng thái tuân thủ tập trung | **Có Dashboard trực quan:** Xem ngay bao nhiêu tài khoản và tài nguyên vi phạm Guardrails |
| **Chi phí dịch vụ** | **Miễn phí ($0)** | **Bản thân Control Tower miễn phí ($0)** (chỉ trả phí tài nguyên ngầm AWS Config, S3, CloudTrail) |
| **Mối quan hệ** | Là thành phần nền tảng bên dưới | **Bao trùm và quản lý chính AWS Organizations bên dưới** |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Set up a secure, multi-account AWS environment (Landing Zone) with the LEAST operational overhead"*| **AWS Control Tower** |
| *"Enforce organizational governance using both preventive SCPs and detective AWS Config rules automatically"*| **AWS Control Tower (Guardrails)** |
| *"Provide a self-service account provisioning portal with baseline VPCs and security guardrails pre-configured"*| **AWS Control Tower Account Factory (using AWS Service Catalog)** |
| *"Consolidated billing and basic hierarchical organizational units without pre-configured baselines"*| **AWS Organizations** |
| *"Centrally archive CloudTrail logs and AWS Config history into a dedicated, tamper-proof security account"*| **AWS Control Tower (Log Archive Account)** |
