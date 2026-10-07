---
title: "Decision Matrix: Amazon S3 Performance Options and Storage Classes"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "Task 3.1: Determine high-performing and/or scalable storage solutions"
exam_weight: "Very High"
services:
  - Amazon S3
  - Amazon S3 Express One Zone
tags:
  - decision-matrix
  - s3
  - s3-express-one-zone
  - transfer-acceleration
  - s3-select
  - multipart-upload
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Amazon S3 Performance Options & Tăng Tốc Dữ Liệu

> Deep Dive chi tiết: [[S3-Express-One-Zone-and-Storage-Performance]] | Trở về: [[00-Domain-3-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Tối ưu hóa hiệu năng Amazon S3]) --> Q1{Mục tiêu tối ưu hóa chính là gì?}

    Q1 -- "ĐỘ TRỄ CỰC THẤP (dưới 10ms) & Hàng triệu request/s cho Machine Learning / AI trong 1 AZ" --> A1["✅ Amazon S3 Express One Zone<br/>(Directory Buckets, nhanh gấp 10x, phí request rẻ hơn 80%)"]

    Q1 -- "TĂNG TỐC TẢI LÊN (Upload) từ các chi nhánh/người dùng toàn cầu xuyên lục địa" --> A2["✅ S3 Transfer Acceleration<br/>(Dẫn qua CloudFront Edge Locations vào mạng cáp quang AWS)"]

    Q1 -- "TẢI FILE KHỔNG LỒ (lớn hơn 100 MB hoặc 5 GB) nhanh hơn và có khả năng thử lại" --> A3["✅ S3 Multipart Upload<br/>(Bắt buộc cho file lớn hơn 5 GB, tải song song nhiều phần)"]

    Q1 -- "TẢI VỀ (Download) file lớn nhanh hơn hoặc xem video tua nhanh (Streaming seek)" --> A4["✅ S3 Byte-Range Fetches<br/>(Gửi header Range tải song song các đoạn byte nhỏ)"]

    Q1 -- "LỌC DỮ LIỆU SQL trực tiếp trên S3 để giảm dung lượng mạng truyền về máy chủ" --> A5["✅ S3 Select<br/>(Lọc dữ liệu trực tiếp tại storage, giảm 80% data transfer)"]

    Q1 -- "GIẢM NGHẼN CỔ CHAI (503 Slow Down) khi lưu hàng chục nghìn file/giây vào S3 Standard" --> A6["✅ Phân chia cấu trúc nhiều Prefixes<br/>(Mỗi prefix độc lập 3,500 PUTs và 5,500 GETs mỗi giây)"]
```

---

## 2. Bảng so sánh các giải pháp tăng tốc S3

| Giải pháp | Cơ chế kỹ thuật | Lợi ích chính | Kịch bản áp dụng chuẩn |
| :--- | :--- | :--- | :--- |
| **S3 Express One Zone** | Lưu trữ trong **Directory Buckets tại 1 AZ** | **Độ trễ < 10ms (10x faster)**, request rẻ hơn 80% | Training AI/ML, High-Frequency Trading, Media Transcoding |
| **S3 Standard (Prefixes)**| Phân tán dữ liệu theo thư mục prefix | Đạt 3,500 PUTs và 5,500 GETs/s **trên mỗi prefix** | Lưu trữ dữ liệu lớn tải cao, Data Lake |
| **S3 Transfer Acceleration**| Định tuyến qua mạng Edge Locations | Tăng tốc upload xuyên quốc gia 50 - 500% | Người dùng/chi nhánh toàn cầu tải file lớn lên S3 |
| **Multipart Upload** | Chia file thành nhiều parts tải song song | Upload nhanh hơn, tự thử lại từng part lỗi | Bắt buộc cho file **> 5 GB**, khuyên dùng cho file **> 100 MB** |
| **Byte-Range Fetches** | Dùng HTTP header `Range: bytes=x-y` | Tải song song nhiều đoạn byte, tải có chọn lọc | Xem video tua nhanh, tải phần đầu của file zip lớn |
| **S3 Select** | Thực thi câu lệnh SQL trực tiếp tại S3 | Giảm tới **80% băng thông truyền tải** | Đọc dữ liệu phân tích từ file CSV/JSON/Parquet lớn |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Giải pháp được chọn |
| :--- | :--- |
| *"Single-digit millisecond latency for frequently accessed data in a single AZ with 80% lower request costs"*| **Amazon S3 Express One Zone** |
| *"Accelerate upload of large files from global users across continents to an S3 bucket"* | **S3 Transfer Acceleration (kết hợp Multipart Upload)** |
| *"Mandatory upload mechanism for objects larger than 5 GB"* | **S3 Multipart Upload** |
| *"Download specific portions of a multi-gigabyte video file to support streaming playback"* | **S3 Byte-Range Fetches** |
| *"Filter large CSV files stored in S3 using SQL without retrieving the full dataset"* | **S3 Select** |
| *"Overcome S3 request rate limits (503 Slow Down) for an analytics ingestion application"* | **Distribute objects across multiple S3 Prefixes** |
