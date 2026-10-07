---
title: "Decision Matrix: Amazon EBS Volume Types and Performance Profiles"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "Task 3.1: Determine high-performing and/or scalable storage solutions"
exam_weight: "Very High"
services:
  - Amazon EBS
  - Amazon EC2 Instance Store
tags:
  - decision-matrix
  - ebs
  - gp3
  - io2-block-express
  - instance-store
  - iops
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Amazon EBS Volume Types & Performance Profiles

> Deep Dive chi tiết: [[EBS-Volume-Types-gp3-and-io2-Block-Express]] | Trở về: [[00-Domain-3-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn loại ổ đĩa lưu trữ Block Storage]) --> Q1{Cần hàng triệu IOPS, độ trễ micro-giây<br/>và CHẤP NHẬN MẤT DỮ LIỆU KHI DỪNG MÁY (Cache/Temp)?}

    Q1 -- "CÓ" --> A1["✅ EC2 Instance Store (NVMe SSD)<br/>(Ổ đĩa gắn vật lý trực tiếp, cực nhanh, Ephemeral data)"]
    Q1 -- "KHÔNG" --> Q2{Mục đích sử dụng làm gì?}

    Q2 -- "Ổ cứng BOOT hoặc Ứng dụng đa dụng, Web, Dev/Test (Tiết kiệm nhất)" --> A2["✅ EBS gp3 (General Purpose SSD)<br/>(3,000 IOPS & 125 MB/s miễn phí, nâng cấp độc lập dung lượng)"]
    Q2 -- "Database lớn (Oracle, SAP HANA), Cần trên 16,000 IOPS, Độ trễ dưới 1ms" --> A3["✅ EBS io2 Block Express<br/>(Lên tới 256,000 IOPS & 4,000 MB/s, độ bền 99.999%)"]
    Q2 -- "Xử lý Big Data, Hadoop HDFS, Kafka, Log tuần tự (KHÔNG LÀM BOOT)" --> A4["✅ EBS st1 (Throughput Optimized HDD)<br/>(Thông lượng 500 MB/s, chi phí thấp cho I/O tuần tự)"]
    Q2 -- "Lưu trữ lạnh, dữ liệu ít truy cập, chi phí rẻ nhất của EBS" --> A5["✅ EBS sc1 (Cold HDD)<br/>(250 MB/s, lưu trữ file nén/archive)"]
```

---

## 2. Bảng so sánh 5 loại ổ đĩa Block Storage

| Tiêu chí | `gp3` (SSD) | `io2 Block Express` (SSD) | `st1` (HDD) | `sc1` (HDD) | EC2 Instance Store |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IOPS tối đa** | **16,000 IOPS** | **256,000 IOPS** | 500 IOPS | 250 IOPS | **Hàng triệu IOPS** |
| **Throughput tối đa** | **1,000 MB/s** | **4,000 MB/s** | **500 MB/s** | 250 MB/s | **Hàng chục GB/s** |
| **Độ trễ** | Vài mili-giây | **Dưới 1 mili-giây (sub-ms)**| Hàng chục mili-giây | Hàng chục mili-giây | **Micro-giây** |
| **Làm Boot Volume?** | **CÓ** | **CÓ** | **KHÔNG** | **KHÔNG** | Tùy chọn OS image |
| **Tính bền bỉ** | 99.8% - 99.9% | **99.999% (5 số 9)** | 99.8% - 99.9% | 99.8% - 99.9% | **Mất dữ liệu khi Stop/Fail** |
| **Cơ chế tính giá** | GB + IOPS/Throughput thêm| GB + Provisioned IOPS | Chỉ tính theo GB | Chỉ tính theo GB | **Bao gồm sẵn trong giá EC2** |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Loại ổ đĩa được chọn |
| :--- | :--- |
| *"Cost-effective boot volume with baseline 3,000 IOPS independent of storage size"* | **Amazon EBS `gp3`** |
| *"Mission-critical database workload requiring sub-millisecond latency and over 64,000 IOPS"* | **Amazon EBS `io2 Block Express`** |
| *"Sequential large block I/O throughput for big data analytics / Kafka at lowest cost"* | **Amazon EBS `st1`** |
| *"Ultra-low latency temporary scratchpad or caching storage with millions of IOPS"* | **EC2 Instance Store (NVMe)** |
| *"EBS volume type that CANNOT be used as an EC2 root/boot volume"* | **`st1` hoặc `sc1`** |
