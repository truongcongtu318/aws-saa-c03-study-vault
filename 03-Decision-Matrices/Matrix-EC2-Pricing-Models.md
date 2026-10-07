---
title: "Decision Matrix: Amazon EC2 Pricing Models"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "Task 4.2: Design cost-optimized compute solutions"
exam_weight: "Very High"
services:
  - Amazon EC2
  - AWS Savings Plans
  - Spot Instances
  - Reserved Instances
tags:
  - decision-matrix
  - ec2-pricing
  - spot
  - savings-plans
  - reserved-instances
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Amazon EC2 Pricing Models

> Deep Dive chi tiết: [[EC2-Pricing-Models-Spot-Savings-Plans-and-RIs]] | Trở về: [[00-Domain-4-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn mô hình định giá máy chủ EC2]) --> Q1{Khối lượng công việc có CHỊU ĐƯỢC GIÁN ĐOẠN<br/>(Fault-tolerant / Batch / Render / CI-CD) không?}

    Q1 -- "CÓ" --> A1["✅ Spot Instances<br/>(Giảm tới 90% / Bị thu hồi với 2-minute notice / Rẻ nhất)"]
    Q1 -- "KHÔNG" --> Q2{Thời gian chạy ứng dụng là ngắn hạn hay dài hạn ổn định?}

    Q2 -- "Ngắn hạn / Không đoán trước được / Mới triển khai / Dev-Test" --> A2["✅ On-Demand Instances<br/>(Linh hoạt tuyệt đối, không cam kết, thanh toán theo giây)"]
    Q2 -- "Dài hạn ổn định (Steady-State 24/7) trong 1 hoặc 3 năm" --> Q3{Mức độ linh hoạt chuyển đổi công nghệ mong muốn?}

    Q3 -- "Linh hoạt tối đa xuyên suốt EC2 (bất kể Family/Region), Fargate & Lambda" --> A3["✅ Compute Savings Plans<br/>(Giảm tới 66% / Khuyên dùng tốt nhất hiện nay)"]
    Q3 -- "Cố định 1 họ máy trong 1 Region cụ thể (ví dụ c6g tại us-east-1)" --> A4["✅ EC2 Instance Savings Plans<br/>(Giảm tới 72% / Rẻ hơn Compute Savings Plans)"]
    Q3 -- "Cơ sở dữ liệu quan hệ RDS hoặc ứng dụng cũ cố định" --> A5["✅ Reserved Instances (Standard 72% / Convertible 54%)"]
```

---

## 2. Bảng so sánh toàn diện 5 mô hình định giá

| Tiêu chí | On-Demand | Spot Instances | Compute Savings Plans | EC2 Instance Savings Plans | Standard RIs |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Mức giảm giá** | **0% (Giá gốc)** | **Lên tới 90%** | **Lên tới 66%** | **Lên tới 72%** | **Lên tới 72%** |
| **Thời hạn cam kết** | Không | Không | **1 hoặc 3 năm** | **1 hoặc 3 năm** | **1 hoặc 3 năm** |
| **Rủi ro thu hồi** | Không bao giờ | **Bị thu hồi sau 2 phút** | Không bao giờ | Không bao giờ | Không bao giờ |
| **Đổi họ máy (Family)**| Tự do | Tự do | **Tự động áp dụng** | Không (Chỉ đổi kích cỡ size) | Không |
| **Đổi Region** | Tự do | Tự do | **Tự động áp dụng** | Không | Không |
| **Đổi sang Fargate/Lambda**| Không | Không | **Tự động áp dụng** | Không | Không |
| **Thanh toán** | Trả sau theo giây | Trả sau theo giây | All / Partial / No Upfront | All / Partial / No Upfront | All / Partial / No Upfront |
| **Kịch bản phù hợp** | Ngắn hạn, thử nghiệm | Xử lý batch, Big Data, CI/CD | Tải ổn định hỗn hợp của công ty | Cụm máy chủ cố định 1 dòng chip | Cơ sở dữ liệu RDS cố định |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Mô hình định giá được chọn |
| :--- | :--- |
| *"Fault-tolerant batch processing or stateless web fleet requiring maximum cost savings up to 90%"*| **Amazon EC2 Spot Instances** |
| *"Workload runs continuously 24/7 for 3 years, company plans to adopt Fargate in the future"*| **Compute Savings Plans** |
| *"Unpredictable short-term workload with no upfront commitment or interruption tolerance"*| **On-Demand Instances** |
| *"Steady-state EC2 workload committed to a specific instance family in a single Region for 3 years"*| **EC2 Instance Savings Plans** |
| *"Reduce risk of Spot interruptions across multiple instance types and Availability Zones"*| **Spot Fleet with `capacity-optimized` strategy** |
