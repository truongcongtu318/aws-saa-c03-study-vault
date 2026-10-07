---
title: "Decision Matrix: AWS Cost Management Tools (Cost Explorer vs Budgets vs Anomaly Detection vs CUR)"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "Task 4.4: Design cost-optimized network architectures"
exam_weight: "High"
services:
  - AWS Cost Explorer
  - AWS Budgets
  - AWS Cost Anomaly Detection
  - AWS Cost and Usage Report (CUR)
  - AWS Trusted Advisor
tags:
  - decision-matrix
  - finops
  - cost-explorer
  - budgets
  - anomaly-detection
  - cur
  - trusted-advisor
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Bộ Công Cụ Quản Lý Chi Phí AWS

> Deep Dive chi tiết: [[AWS-Cost-Governance-Budgets-Anomaly-and-Tags]] | Trở về: [[00-Domain-4-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn công cụ quản lý chi phí AWS FinOps]) --> Q1{Mục tiêu bạn muốn thực hiện là gì?}

    Q1 -- "ĐẶT NGƯỠNG NGÂN SÁCH CỐ ĐỊNH, Gửi cảnh báo & TỰ ĐỘNG HÀNH ĐỘNG (Áp SCP / Tắt máy)" --> A1["✅ AWS Budgets<br/>(Ngân sách chi phí/sử dụng, hỗ trợ Budget Actions tự động)"]

    Q1 -- "PHÁT HIỆN SỰ CỐ BẤT THƯỜNG BẰNG AI (Tự tìm đợt tăng vọt chi phí lạ không cần đặt ngưỡng)" --> A2["✅ AWS Cost Anomaly Detection<br/>(Machine Learning tự động phát hiện sốc chi phí & tìm nguyên nhân gốc)"]

    Q1 -- "XEM BIỂU ĐỒ TRỰC QUAN XU HƯỚNG, Dự báo 12 tháng tới & Nhận khuyến nghị Savings Plans/RIs" --> A3["✅ AWS Cost Explorer<br/>(Trực quan hóa chi tiêu, lọc theo Tags, phân tích xu hướng)"]

    Q1 -- "XUẤT TOÀN BỘ DỮ LIỆU CHI TIẾT NHẤT RA S3 ĐỂ PHÂN TÍCH BẰNG ATHENA / QUICKSIGHT" --> A4["✅ AWS Cost and Usage Report (CUR)<br/>(Dữ liệu chi tiết từng dòng tài nguyên theo giờ, lưu file Parquet trên S3)"]

    Q1 -- "KIỂM TRA TÀI NGUYÊN LÃNG PHÍ NHÀN RỖI (EBS mồ côi, IP không dùng, ELB rỗng)" --> A5["✅ AWS Trusted Advisor<br/>(Kiểm tra 5 trụ cột, nhận diện tài nguyên nhàn rỗi để xóa bỏ)"]
```

---

## 2. Bảng so sánh trực diện 5 công cụ tài chính đám mây

| Tiêu chí | AWS Cost Explorer | AWS Budgets | AWS Cost Anomaly Detection | AWS Cost and Usage Report (CUR) | AWS Trusted Advisor |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bản chất chính** | **Phân tích & Dự báo trực quan** | **Kiểm soát Ngân sách & Hành động** | **Phát hiện Bất thường bằng AI/ML** | **Báo cáo Dữ liệu thô chuyên sâu** | **Khuyến nghị Kiểm toán hạ tầng** |
| **Hình thức giao diện**| Biểu đồ đường, cột trên Console | Thiết lập ngưỡng và cảnh báo | Bảng cảnh báo bất thường | File CSV / Parquet lưu trên S3 | Danh sách Checklists 5 trụ cột |
| **Hành động tự động?**| Không (Chỉ xem) | **CÓ (Budget Actions: SCP, Stop EC2)**| CÓ (Gửi thông báo qua SNS) | Không (Dữ liệu tĩnh) | Không |
| **Dự báo tương lai?** | **CÓ (Lên tới 12 tháng tới)** | **CÓ (Cảnh báo khi dự báo vượt ngưỡng)**| Không (Phát hiện quá khứ/hiện tại)| Không | Không |
| **Gợi ý Savings Plans?**| **CÓ (Khuyến nghị mua chi tiết)**| Không | Không | Không | CÓ (Kiểm tra RI utilization) |
| **Tìm tài nguyên rác?** | Không trực tiếp | Không | Không | Không | **CÓ (EBS unattached, Idle ELB)** |
| **Chi phí sử dụng** | **Miễn phí** (API calls có phí nhỏ)| Miễn phí 2 budgets đầu tiên ($0.02/ngày sau đó)| **Miễn phí** | Miễn phí (Chỉ trả tiền lưu trữ S3)| Miễn phí bản Core / Trả phí gói Support |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Công cụ tài chính được chọn |
| :--- | :--- |
| *"Set custom spending limits and automatically apply a Service Control Policy (SCP) when exceeded"*| **AWS Budgets (with Budget Actions)** |
| *"Machine learning powered tool to detect sudden unexpected cost spikes and determine root cause"*| **AWS Cost Anomaly Detection** |
| *"Visualize historical spending trends over the past 12 months and forecast next year's bill"*| **AWS Cost Explorer** |
| *"Deliver the most comprehensive, granular cost data to Amazon S3 for Athena SQL analytics"*| **AWS Cost and Usage Report (CUR)** |
| *"Identify idle resources such as unattached EBS volumes and underutilized load balancers to reduce costs"*| **AWS Trusted Advisor (Cost pillar)** |
| *"Generate purchase recommendations for Compute Savings Plans and Reserved Instances"*| **AWS Cost Explorer** |
