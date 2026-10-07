---
title: "Decision Matrix: Amazon S3 Storage Classes Cost and Retrieval"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "Task 4.1: Design cost-optimized storage solutions"
exam_weight: "Very High"
services:
  - Amazon S3
  - Amazon S3 Glacier
tags:
  - decision-matrix
  - s3
  - storage-classes
  - glacier
  - retrieval-cost
  - intelligent-tiering
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Amazon S3 Storage Classes (Cước Phí & Truy Xuất)

> Deep Dive chi tiết: [[S3-Lifecycle-and-Storage-Classes-Cost-Tuning]] | Trở về: [[00-Domain-4-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn lớp lưu trữ Amazon S3 tối ưu chi phí]) --> Q1{Mô hình tần suất truy cập dữ liệu có dự đoán được không?}

    Q1 -- "KHÔNG ĐOÁN ĐƯỢC hoặc THAY ĐỔI LIÊN TỤC (Tối ưu tự động, không phí phạt)" --> A1["✅ S3 Intelligent-Tiering<br/>(Tự động đổi tầng sau 30/90/180 ngày, KHÔNG THU PHÍ TRUY XUẤT)"]

    Q1 -- "DỰ ĐOÁN ĐƯỢC" --> Q2{Tần suất truy cập dữ liệu như thế nào?}

    Q2 -- "Truy cập thường xuyên (Nhiều lần mỗi tuần)" --> A2["✅ S3 Standard<br/>(0.023 USD/GB, không phí truy xuất, không giới hạn ngày lưu)"]

    Q2 -- "Ít truy cập (1-2 lần/tháng), NHƯNG KHI CẦN PHẢI LẤY NGAY (Độ trễ mili-giây)" --> Q3{Dữ liệu quan trọng cần Multi-AZ hay dữ liệu phụ có thể tạo lại?}
    Q3 -- "Dữ liệu quan trọng (Multi-AZ)" --> A3["✅ S3 Standard-IA (Infrequent Access)<br/>(0.0125 USD/GB, lưu tối thiểu 30 ngày / 128 KB)"]
    Q3 -- "Dữ liệu phụ (1 AZ duy nhất, rẻ hơn 20%)" --> A4["✅ S3 One Zone-IA<br/>(0.010 USD/GB, lưu tối thiểu 30 ngày / 128 KB)"]

    Q2 -- "SAO LƯU DÀI HẠN / LƯU TRỮ LẠNH (Archive, 1-2 lần/năm)" --> Q4{Thời gian lấy dữ liệu (Retrieval Time) chấp nhận được là bao lâu?}
    Q4 -- "Phải lấy lại được trong VÀI MILI-GIÂY (Hồ sơ y tế, ảnh CCCD)" --> A5["✅ S3 Glacier Instant Retrieval<br/>(0.004 USD/GB, lưu tối thiểu 90 ngày)"]
    Q4 -- "Chấp nhận chờ từ 1-5 phút (Expedited) hoặc 3-5 giờ (Standard)" --> A6["✅ S3 Glacier Flexible Retrieval<br/>(0.0036 USD/GB, lưu tối thiểu 90 ngày)"]
    Q4 -- "Chấp nhận chờ 12 đến 48 giờ, RẺ NHẤT TOÀN BỘ AWS (~1 USD/TB/tháng)" --> A7["✅ S3 Glacier Deep Archive<br/>(0.00099 USD/GB, lưu tối thiểu 180 ngày / Tuân thủ 7-10 năm)"]
```

---

## 2. Bảng so sánh 8 lớp lưu trữ Amazon S3

| Lớp lưu trữ S3 | Cước lưu trữ / GB | Thời gian lấy lại | Phí truy xuất dữ liệu | Ngày lưu tối thiểu | Kích thước tối thiểu |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **S3 Standard** | ~$0.0230 | Tức thì (ms) | **$0** | Không | Không |
| **S3 Intelligent-Tiering**| Tự động theo tầng | Tức thì (ms) | **$0** | Không | 128 KB |
| **S3 Standard-IA** | ~$0.0125 | Tức thì (ms) | $0.01 / GB | **30 ngày** | **128 KB** |
| **S3 One Zone-IA** | ~$0.0100 | Tức thì (ms) | $0.01 / GB | **30 ngày** | **128 KB** |
| **S3 Glacier Instant** | ~$0.0040 | Tức thì (ms) | $0.03 / GB | **90 ngày** | **128 KB** |
| **S3 Glacier Flexible**| ~$0.0036 | 1-5 phút / 3-5 giờ | $0.03 / GB | **90 ngày** | **40 KB** |
| **S3 Glacier Deep Archive**| **~$0.00099 (~$1/TB)**| **12 đến 48 giờ** | $0.02 / GB | **180 ngày** | **40 KB** |
| **S3 Express One Zone**| ~$0.1600 (Đắt hơn) | **Dưới 10ms (10x faster)**| Request rẻ hơn 80% | Không | Không |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Lớp lưu trữ được chọn |
| :--- | :--- |
| *"Unpredictable or changing access patterns, no retrieval fees, zero operational overhead"*| **S3 Intelligent-Tiering** |
| *"Archived medical or financial records retrieved rarely, but requires immediate millisecond access"*| **S3 Glacier Instant Retrieval** |
| *"Lowest cost archival storage in AWS for regulatory compliance (7-10 years), 12-48 hour retrieval"*| **S3 Glacier Deep Archive** |
| *"Secondary reproducible thumbnail images accessed infrequently at 20% lower cost"*| **S3 One Zone-IA** |
| *"Infrequently accessed data in multiple AZs with millisecond retrieval"*| **S3 Standard-IA** |
| *"Single-digit millisecond latency for machine learning training in a single AZ"*| **S3 Express One Zone** |
