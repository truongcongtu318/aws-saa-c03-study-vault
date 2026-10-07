---
title: "Decision Matrix: SQS vs SNS vs EventBridge vs Kinesis Data Streams"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "Task 2.1: Design scalable and loosely coupled architectures"
exam_weight: "High"
services:
  - Amazon SQS
  - Amazon SNS
  - Amazon EventBridge
  - Amazon Kinesis Data Streams
tags:
  - decision-matrix
  - sqs
  - sns
  - eventbridge
  - kinesis
  - messaging
  - decoupling
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: SQS vs SNS vs EventBridge vs Kinesis Data Streams

> Deep Dive chi tiết: [[Decoupling-with-SQS-and-SNS]] | [[Event-Driven-Architectures-EventBridge-StepFunctions]] | Trở về: [[00-Domain-2-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn công nghệ xử lý thông điệp / sự kiện]) --> Q1{Cần xử lý luồng dữ liệu thời gian thực quy mô lớn (Streaming)<br/>hoặc phân tích dữ liệu lớn (Big Data / Clickstream)?}

    Q1 -- "CÓ (Xử lý theo Shards, replay trong 365 ngày)" --> A1["✅ Amazon Kinesis Data Streams<br/>(Real-time data streaming / Multi-consumer / Ordering per partition)"]
    Q1 -- "KHÔNG" --> Q2{Mô hình truyền thông điệp là gì?}

    Q2 -- "PULL (Consumer chủ động lấy tin, cần hàng đợi đệm để điều hòa tải)" --> A2["✅ Amazon SQS<br/>(Queue 1-to-1 / Buffering / Visibility Timeout / DLQ)"]
    Q2 -- "PUSH (Publisher đẩy tin tới nhiều đích cùng lúc - 1-to-Many)" --> Q3{Sự kiện đến từ đâu và có cần tích hợp SaaS bên thứ ba / Archive & Replay không?}

    Q3 -- "Cần kết nối SaaS (Zendesk/Shopify), JSON Pattern Matching, hoặc Archive/Replay" --> A3["✅ Amazon EventBridge<br/>(Serverless Event Bus / SaaS integration / Replay events)"]
    Q3 -- "Thông báo Pub/Sub tốc độ cao, gửi tới hàng triệu SQS/Email/SMS/Lambda (Fan-Out)" --> A4["✅ Amazon SNS<br/>(Pub/Sub Topic / Fan-out pattern / Message Filtering)"]
```

---

## 2. Bảng so sánh 4 giải pháp truyền thông điệp cốt lõi

| Tiêu chí so sánh | Amazon SQS | Amazon SNS | Amazon EventBridge | Amazon Kinesis Data Streams |
| :--- | :--- | :--- | :--- | :--- |
| **Mô hình truyền** | **PULL (Consumer kéo tin)** | **PUSH (Đẩy tới subscribers)**| **PUSH (Đẩy tới targets)** | **PULL (Consumer đọc theo shard)** |
| **Mối quan hệ** | **1-to-1** (1 tin chỉ 1 worker xử lý rồi xóa)| **1-to-Many** (1 tin gửi tới nhiều subs) | **1-to-Many** (Event Bus lọc sang nhiều target)| **1-to-Many** (Nhiều app cùng đọc 1 stream) |
| **Thời gian lưu trữ tin**| 1 phút đến **14 ngày** (mặc định 4 ngày) | **Không lưu trữ** (mất nếu không có sub)| Hỗ trợ **Archive vô hạn** | **24 giờ đến 365 ngày** (mặc định 24h) |
| **Phát lại tin (Replay)**| Không hỗ trợ | Không hỗ trợ | **Hỗ trợ Replay sự kiện quá khứ**| **Hỗ trợ Replay từ vị trí con trỏ shard** |
| **Bộ đệm điều hòa tải** | **CÓ** (Lý tưởng làm Buffer chống sập DB)| Không (Chuyển tiếp tức thì) | Không (Chuyển tiếp tức thì) | **CÓ** (Lưu luồng dữ liệu liên tục) |
| **Độ trễ** | Vài chục mili-giây | Mili-giây | Hàng trăm mili-giây | **Mili-giây (Real-time)** |
| **Kích thước payload** | Tối đa **256 KB** (Dùng S3 nếu >256KB) | Tối đa **256 KB** | Tối đa **256 KB** | Tối đa **1 MB** mỗi record |
| **Thứ tự (Ordering)** | FIFO Queue (300 msg/s) | SNS FIFO Topic | Không đảm bảo thứ tự | **Đảm bảo theo Partition Key** |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Buffer incoming requests to prevent overloading backend database"* | **Amazon SQS** |
| *"Fan-out messages to multiple independent worker queues"* | **Amazon SNS (kết hợp SQS)** |
| *"Filter messages based on JSON message attributes before delivering to queues"* | **SNS Message Filtering** |
| *"React to events from SaaS applications (Shopify, Zendesk) or AWS services"* | **Amazon EventBridge** |
| *"Replay historical events after a bug fix in the processing service"* | **Amazon EventBridge (Archive & Replay)** |
| *"Ingest real-time clickstream or IoT telemetry data for multi-consumer analytics"* | **Amazon Kinesis Data Streams** |
