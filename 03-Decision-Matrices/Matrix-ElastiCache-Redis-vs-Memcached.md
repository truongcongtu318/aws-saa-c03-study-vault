---
title: "Decision Matrix: Amazon ElastiCache for Redis vs Memcached"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "Task 3.3: Determine high-performing database solutions"
exam_weight: "High"
services:
  - Amazon ElastiCache
  - Amazon ElastiCache Serverless
tags:
  - decision-matrix
  - elasticache
  - redis
  - memcached
  - caching
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: ElastiCache for Redis vs Memcached

> Deep Dive chi tiết: [[Caching-Deep-Dive-ElastiCache-and-DAX]] | Trở về: [[00-Domain-3-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn công nghệ Caching cho ElastiCache]) --> Q1{Có yêu cầu tự động scale bộ nhớ và CPU tức thì<br/>mà KHÔNG MUỐN quản lý cụm node không?}

    Q1 -- "CÓ" --> A1["✅ Amazon ElastiCache Serverless<br/>(Hỗ trợ cả Redis và Memcached / Tự động scale trong vài giây)"]
    Q1 -- "KHÔNG" --> Q2{Ứng dụng cần cấu trúc dữ liệu phức tạp (Sorted Sets, Hashes)<br/>hoặc Tính sẵn sàng cao (Multi-AZ, Tự động Failover, Lưu bền vững)?}

    Q2 -- "CÓ" --> A2["✅ Amazon ElastiCache for Redis<br/>(Sorted sets, Pub/Sub, Multi-AZ failover, Read Replicas, AOF/Snapshot)"]
    Q2 -- "KHÔNG (Chỉ cần cache chuỗi Key-Value đơn thuần / Cần đa luồng Multi-threaded)" --> A3["✅ Amazon ElastiCache for Memcached<br/>(Thuần in-memory, đa luồng, mở rộng ngang đơn giản, không lưu bền vững)"]
```

---

## 2. Bảng so sánh trực diện 10 tiêu chí kỹ thuật

| Tiêu chí so sánh | ElastiCache for Redis | ElastiCache for Memcached |
| :--- | :--- | :--- |
| **Kiến trúc luồng xử lý** | Đơn luồng (Single-threaded core) | **Đa luồng (Multi-threaded)** tận dụng tối đa đa nhân CPU |
| **Cấu trúc dữ liệu** | **Rất phong phú:** Strings, Hashes, Lists, Sets, **Sorted Sets**, Bitmaps, Geospatial | **Đơn giản:** Chỉ hỗ trợ chuỗi String và Object nhị phân |
| **Khả dụng cao (High Availability)**| **CÓ:** Hỗ trợ Multi-AZ với **Tự động Failover** | **KHÔNG:** Không có Standby node, không có tự động failover |
| **Bản sao đọc (Read Replicas)**| **Hỗ trợ tối đa 5 Read Replicas** trên mỗi shard | Không hỗ trợ (Chỉ mở rộng bằng cách thêm node mới) |
| **Lưu trữ bền vững (Persistence)**| **CÓ:** Hỗ trợ ghi file AOF (Append Only File) và Snapshot RDB | **KHÔNG:** Dữ liệu trong RAM mất hoàn toàn khi node restart |
| **Tính năng Pub/Sub** | **CÓ** (Kênh giao tiếp thời gian thực Publish/Subscribe) | Không hỗ trợ |
| **Mở rộng Sharding** | **Redis Cluster:** Hỗ trợ tối đa 500 shards | Sharding do Client-side hashing phân tán |
| **Sao lưu & Khôi phục (Backup)**| **Hỗ trợ đầy đủ** Backup lên Amazon S3 | Không hỗ trợ |
| **Bảng xếp hạng (Leaderboards)**| **Rất tối ưu** (Dùng kiểu dữ liệu `Sorted Sets`) | Không hỗ trợ cấu trúc xếp hạng |
| **Tùy chọn Serverless** | **Có (ElastiCache Serverless for Redis)** | **Có (ElastiCache Serverless for Memcached)** |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Công nghệ Cache được chọn |
| :--- | :--- |
| *"Maintain a real-time gaming leaderboard with ranked player scores"* | **ElastiCache for Redis (Sorted Sets)** |
| *"In-memory caching layer with Multi-AZ replication and automatic failover"* | **ElastiCache for Redis** |
| *"Simple multi-threaded key-value caching layer with no persistence requirements"* | **ElastiCache for Memcached** |
| *"Cache session state for web application with unpredictable traffic surges and zero node management"*| **Amazon ElastiCache Serverless** |
| *"Point-in-time backup and restore capability for cached in-memory data"* | **ElastiCache for Redis** |
| *"Scale cache horizontally across multiple independent nodes using consistent hashing"* | **ElastiCache for Memcached** |
