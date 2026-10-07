---
title: "Decision Matrix: AWS Secrets Manager vs SSM Parameter Store"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.3: Determine appropriate data security controls"
exam_weight: "High"
services:
  - AWS Secrets Manager
  - AWS Systems Manager Parameter Store
tags:
  - decision-matrix
  - secrets-manager
  - parameter-store
  - comparison
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Secrets Manager vs SSM Parameter Store

> Deep Dive chi tiết: [[Secrets-Manager-and-Parameter-Store]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định (Decision Tree)

```mermaid
flowchart TD
    Start([Lựa chọn giải pháp lưu trữ dữ liệu nhạy cảm]) --> Q1{Cần tự động xoay mật khẩu<br/>(Automatic Rotation) cho DB/API Key?}

    Q1 -- "CÓ" --> A1["✅ AWS Secrets Manager<br/>(Tích hợp sẵn Lambda Rotation cho RDS/Aurora)"]
    Q1 -- "KHÔNG" --> Q2{Cần sao chép bí mật sang nhiều Region<br/>(Multi-Region Replication)?}

    Q2 -- "CÓ" --> A1
    Q2 -- "KHÔNG" --> Q3{Cần lưu cấu hình ứng dụng thông thường<br/>với chi phí MIỄN PHÍ?}

    Q3 -- "CÓ (Tối đa 4KB)" --> A2["✅ SSM Parameter Store (Standard)<br/>(Miễn phí 100%, hỗ trợ SecureString với KMS)"]
    Q3 -- "KHÔNG (Cần lớn hơn 4KB hoặc TTL Policies)" --> A3["✅ SSM Parameter Store (Advanced)<br/>(0.05 USD/tháng, tối đa 8KB)"]
```

---

## 2. Bảng so sánh tính năng trực diện

| Đặc tính kỹ thuật            | AWS Secrets Manager                         | SSM Parameter Store (Standard)                  | SSM Parameter Store (Advanced)                   |
| :--------------------------- | :------------------------------------------ | :---------------------------------------------- | :----------------------------------------------- |
| **Mục đích chính**           | Mật khẩu database, API keys, credentials    | Cấu hình ứng dụng, chuỗi kết nối tĩnh           | Cấu hình ứng dụng dung lượng lớn, chính sách TTL |
| **Tự động xoay mật khẩu**    | **Có sẵn (Built-in Lambda)** cho RDS/Aurora | Không có sẵn (Phải tự code)                     | Không có sẵn (Phải tự code)                      |
| **Chi phí cơ bản**           | **$0.40 / secret / tháng**                  | **MIỄN PHÍ ($0)**                               | **$0.05 / param / tháng**                        |
| **Phí API Calls**            | $0.05 mỗi 10,000 API calls                  | Miễn phí (Standard throughput)                  | $0.05 mỗi 10,000 API calls (High throughput)     |
| **Dung lượng tối đa**        | **64 KB**                                   | **4 KB**                                        | **8 KB**                                         |
| **Sao chép đa vùng**         | **Có (1-click Cross-region replica)**       | Không hỗ trợ tự động                            | Không hỗ trợ tự động                             |
| **Chính sách hết hạn (TTL)** | Không có chính sách hết hạn                 | Không có                                        | **Có (Parameter Policies / TTL)**                |
| **Mã hóa dữ liệu**           | Bắt buộc (qua AWS KMS)                      | Tùy chọn (`String` hoặc `SecureString` qua KMS) | Tùy chọn (`String` hoặc `SecureString` qua KMS)  |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Automatically rotate credentials every 30 days"* | **AWS Secrets Manager** |
| *"Database credentials for Amazon RDS with built-in rotation"* | **AWS Secrets Manager** |
| *"Cross-region secret replication for disaster recovery"* | **AWS Secrets Manager** |
| *"Store application configuration settings at lowest cost"* | **SSM Parameter Store (Standard)** |
| *"Hierarchical parameter structure e.g. /prod/db/name"* | **SSM Parameter Store** |
| *"Set an expiration date or notification policy for parameters"* | **SSM Parameter Store (Advanced)** |
