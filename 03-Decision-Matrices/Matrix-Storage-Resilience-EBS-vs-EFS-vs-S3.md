---
title: "Decision Matrix: Storage Resilience (Amazon EBS vs EFS vs S3 vs FSx)"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "Task 2.2: Design highly available and/or fault-tolerant architectures"
exam_weight: "High"
services:
  - Amazon EBS
  - Amazon EFS
  - Amazon S3
  - Amazon FSx
tags:
  - decision-matrix
  - storage-resilience
  - ebs
  - efs
  - s3
  - fsx
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Storage Resilience (EBS vs EFS vs S3 vs FSx)

> Deep Dive chi tiết: [[Storage-Durability-S3-EBS-EFS-FSx]] | Trở về: [[00-Domain-2-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn giải pháp lưu trữ có tính chịu lỗi cao]) --> Q1{Loại lưu trữ bạn cần là gì?}

    Q1 -- "Lưu trữ ĐỐI TƯỢNG (Object Storage / HTTP / Tệp tĩnh / Web / Data Lake)" --> A1["✅ Amazon S3<br/>(Độ bền 11 số 9, lưu trên 3 AZs, hỗ trợ CRR và S3 RTC 15 phút)"]

    Q1 -- "Lưu trữ KHỐI ĐĨA (Block Storage / Cài HĐH / Ổ đĩa gắn máy chủ EC2)" --> A2["✅ Amazon EBS<br/>(Giới hạn trong 1 AZ, sao lưu bằng Snapshots, Multi-Attach cho io1/io2 trong 1 AZ)"]

    Q1 -- "Lưu trữ HỆ THỐNG TỆP DÙNG CHUNG (File Storage / Chia sẻ cho nhiều máy chủ)" --> Q2{Hệ điều hành và khối lượng công việc là gì?}

    Q2 -- "Linux / Chuẩn POSIX / Giao thức NFS / Tự động mở rộng dung lượng" --> A3["✅ Amazon EFS<br/>(Regional Multi-AZ mặc định, cùng đọc ghi hàng trăm EC2 xuyên AZ)"]

    Q2 -- "Windows Server / Giao thức SMB / Tích hợp Active Directory" --> A4["✅ Amazon FSx for Windows File Server<br/>(Hỗ trợ tùy chọn Multi-AZ, DFS, shadow copies)"]

    Q2 -- "Tính toán hiệu năng cao HPC / Machine Learning / Tốc độ hàng trăm GB/s" --> A5["✅ Amazon FSx for Lustre<br/>(Liên kết trực tiếp với S3 Bucket, độ trễ sub-millisecond)"]
```

---

## 2. Bảng so sánh 5 công nghệ lưu trữ về Tính Sẵn Sàng & Phục Hồi

| Tiêu chí | Amazon EBS | Amazon EFS | Amazon FSx for Windows | Amazon FSx for Lustre | Amazon S3 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Mô hình dữ liệu** | **Block Storage** | **File Storage (POSIX)** | **File Storage (SMB)** | **File Storage (POSIX HPC)**| **Object Storage** |
| **Phạm vi khả dụng**| **Khóa chặt trong 1 AZ** | **Toàn Region (Multi-AZ)**| **Single-AZ hoặc Multi-AZ**| Thường là 1 AZ (Scratch/Persist)| **Toàn Region (3+ AZs)** |
| **Độ bền bỉ (Durability)**| 99.8% - 99.999% | **99.999999999% (11 9's)**| Thiết kế cho Enterprise | Theo cấu hình lưu trữ | **99.999999999% (11 9's)** |
| **Chia sẻ nhiều máy chủ**| Chỉ với **Multi-Attach (io1/io2 trong 1 AZ)** | **Hàng ngàn EC2 cùng lúc xuyên AZ** | Hàng ngàn EC2 Windows | Hàng trăm Compute instances | Truy cập qua HTTP/HTTPS API |
| **Cơ chế sao lưu phục hồi**| **EBS Snapshots** (Lưu ngầm trên S3, FSR) | **AWS Backup** tự động | **AWS Backup** / Volume shadow | Đồng bộ ngược lại vào S3 | **Versioning, CRR, S3 RTC** |
| **Tốc độ truy xuất** | IOPS cao, độ trễ sub-ms | Mili-giây | Mili-giây | **Hàng trăm GB/s, sub-ms** | Vài chục mili-giây |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ lưu trữ được chọn |
| :--- | :--- |
| *"Shared file storage accessible concurrently by hundreds of Linux EC2 instances across multiple AZs"*| **Amazon EFS** |
| *"Shared file storage for Windows workloads integrating with Active Directory and SMB protocol"* | **Amazon FSx for Windows File Server (Multi-AZ)** |
| *"High-performance compute (HPC) cluster requiring fast sub-millisecond POSIX file access linked to S3"*| **Amazon FSx for Lustre** |
| *"Attach a high-performance block storage volume to two EC2 instances in the same AZ"* | **Amazon EBS Multi-Attach (`io1` hoặc `io2`)** |
| *"Point-in-time incremental backup of an EBS volume that can be restored instantly without latency penalty"*| **EBS Snapshots kết hợp Fast Snapshot Restore (FSR)** |
| *"Replicate S3 objects to another Region with a guaranteed SLA of 15 minutes"* | **S3 Cross-Region Replication (CRR) với S3 RTC** |
