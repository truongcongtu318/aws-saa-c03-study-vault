---
title: "Decision Matrix: Active Directory Integration Modes & Directory Service"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.1: Design secure access to AWS resources"
exam_weight: "Medium"
services:
  - AWS Managed Microsoft AD
  - AD Connector
  - Simple AD
  - AWS IAM Identity Center
tags:
  - decision-matrix
  - active-directory
  - directory-service
  - sso
  - identity
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Active Directory Integration Modes

> Deep Dive chi tiết: [[Directory-Service-and-IAM-Identity-Center]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Doanh nghiệp cần tích hợp Active Directory lên AWS]) --> Q1{Có sẵn On-Premises Active Directory chưa?}

    Q1 -- "CHƯA (Cần dựng mới trên Cloud cho môi trường nhỏ dưới 5,000 user)" --> A1["✅ Simple AD<br/>(Dựa trên Samba 4, rẻ, độc lập, KHÔNG hỗ trợ Trust)"]
    Q1 -- "CÓ (Đã có hệ thống AD sẵn tại doanh nghiệp)" --> Q2{Doanh nghiệp có muốn LƯU TRỮ / ĐỒNG BỘ<br/>thông tin user lên Cloud không?}

    Q2 -- "KHÔNG (Chỉ muốn làm Proxy chuyển tiếp xác thực về On-prem)" --> A2["✅ AD Connector<br/>(Không lưu user trên AWS, chuyển tiếp xác thực qua VPN/Direct Connect)"]
    Q2 -- "CÓ (Cần Domain Controller Windows thật trên Cloud / Two-way Forest Trust)" --> A3["✅ AWS Managed Microsoft AD<br/>(Windows Server DC thật, Multi-AZ, hỗ trợ Trust 2 chiều)"]

    A2 --> Portal["Cần đăng nhập đa tài khoản AWS SSO"]
    A3 --> Portal
    Portal --> SSO["✅ Kết hợp AWS IAM Identity Center"]
```

---

## 2. Bảng so sánh 3 tùy chọn AWS Directory Service

| Tiêu chí so sánh | AWS Managed Microsoft AD | AD Connector | Simple AD |
| :--- | :--- | :--- | :--- |
| **Bản chất công nghệ** | **Windows Server Active Directory thật** do AWS quản lý Multi-AZ | **Directory Gateway Proxy** (Không phải Domain Controller) | **Samba 4 Open Source** tương thích giao thức AD |
| **Lưu trữ User trên Cloud?**| **CÓ** (Lưu trong Domain Controller trên AWS) | **KHÔNG** (Chỉ chuyển tiếp truy vấn về On-Prem AD) | **CÓ** (Lưu độc lập trên Samba) |
| **Hỗ trợ Forest Trust?** | **CÓ (One-way và Two-way Forest Trust)** | Không áp dụng (Bản thân nó chỉ là client proxy) | **KHÔNG** (Không hỗ trợ Trust) |
| **Windows Group Policy (GPO)?**| **Đầy đủ** | Lấy từ On-Premises AD | Hỗ trợ cơ bản |
| **Quy mô khuyến nghị** | Doanh nghiệp lớn (> 5,000 users) | Mọi quy mô có sẵn On-prem AD | Quy mô nhỏ (< 5,000 users) |
| **Tích hợp AWS IAM Identity Center**| **Hỗ trợ** | **Hỗ trợ** | Không hỗ trợ |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Establish a two-way forest trust relationship between on-premises AD and AWS"* | **AWS Managed Microsoft AD** |
| *"Use existing on-premises Active Directory credentials without caching or storing them in AWS"* | **AD Connector** |
| *"Low-cost standalone directory for basic LDAP/AD compatibility without trust"* | **Simple AD** |
| *"Single sign-on access to multiple AWS accounts integrated with corporate Active Directory"* | **AWS IAM Identity Center** (kết hợp AD Connector hoặc Managed AD) |
| *"Seamless domain-join for Windows EC2 workloads with full Group Policy (GPO) support"* | **AWS Managed Microsoft AD** |
