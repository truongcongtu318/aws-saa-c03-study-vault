---
title: "Decision Matrix: Amazon CloudFront vs AWS Global Accelerator"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "Task 3.4: Determine high-performing and/or scalable network architectures"
exam_weight: "Very High"
services:
  - Amazon CloudFront
  - AWS Global Accelerator
tags:
  - decision-matrix
  - cloudfront
  - global-accelerator
  - edge-networking
  - anycast-ip
  - cdn
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Amazon CloudFront vs AWS Global Accelerator

> Deep Dive chi tiết: [[CloudFront-vs-Global-Accelerator-Edge-Performance]] | Trở về: [[00-Domain-3-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Tối ưu hóa hiệu năng mạng cho người dùng toàn cầu]) --> Q1{Loại lưu lượng và yêu cầu Caching là gì?}

    Q1 -- "Lưu lượng HTTP/HTTPS/WebSocket, CẦN LƯU BỘ NHỚ ĐỆM (Caching) nội dung tĩnh/video" --> A1["✅ Amazon CloudFront<br/>(Global CDN / Edge Caching / Tối ưu hoá cho Web & Video)"]

    Q1 -- "Giao thức TCP/UDP (Gaming, VoIP, IoT) hoặc HTTP KHÔNG ĐƯỢC CACHE" --> Q2{Khách hàng có yêu cầu 2 ĐỊA CHỈ IP TĨNH CỐ ĐỊNH (Static Anycast IP)<br/>để cấu hình Firewall / Whitelist không?}

    Q2 -- "CÓ (Hoặc cần chuyển đổi dự phòng Failover tức thì dưới 1 phút)" --> A2["✅ AWS Global Accelerator<br/>(2 Static Anycast IPs / Mạng trục cáp quang riêng AWS / Không Cache)"]
    Q2 -- "KHÔNG (Vẫn là HTTP web động)" --> A1
```

---

## 2. Bảng so sánh toàn diện 10 tiêu chí kỹ thuật

| Tiêu chí | Amazon CloudFront | AWS Global Accelerator |
| :--- | :--- | :--- |
| **Bản chất công nghệ** | **Mạng phân phối nội dung (CDN)** | **Dịch vụ định tuyến mạng Anycast (Network Layer)** |
| **Cơ chế hoạt động** | **CÓ LƯU TRỮ BỘ NHỚ ĐỆM (Caching)** tại 400+ PoPs | **KHÔNG CÓ BỘ NHỚ ĐỆM (No Caching)** |
| **Giao thức mạng hỗ trợ**| **Chỉ hỗ trợ HTTP, HTTPS, WebSocket** | **Tất cả các giao thức TCP và UDP** (HTTP, VoIP, Gaming...) |
| **Địa chỉ IP cung cấp** | Tên miền DNS động (IP thay đổi liên tục) | **2 Địa chỉ IP tĩnh Anycast IPv4 duy nhất trên toàn cầu** |
| **Tốc độ Failover đa vùng**| Phụ thuộc vào thời gian **DNS TTL** (vài phút đến giờ) | **Gần như tức thì (< 1 phút)**, không bị ảnh hưởng bởi DNS |
| **Bảo vệ máy chủ gốc** | Giảm tới **90% request** nhờ bộ nhớ đệm và **Origin Shield** | Không giảm request (toàn bộ gói tin đều được gửi tới đích) |
| **Tích hợp bảo mật** | Tích hợp sâu với **AWS WAF** và **AWS Shield** | Tích hợp với **AWS Shield** (Không tích hợp WAF trực tiếp) |
| **Tính toán tại biên** | Hỗ trợ **CloudFront Functions** và **Lambda@Edge** | Không hỗ trợ tính toán tại biên |
| **Target hỗ trợ** | S3 bucket, ALB, EC2, HTTP custom servers | **ALB, Network Load Balancer (NLB), EC2, Elastic IP** |
| **Khối lượng công việc chuẩn**| Web tĩnh, phân phối media, video streaming, API cacheable | **Gaming nhiều người chơi, VoIP gọi điện, IoT, Ứng dụng cần IP cố định** |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Provide two static Anycast IP addresses for global users to connect to multi-region ALBs"*| **AWS Global Accelerator** |
| *"Improve performance for a global gaming application communicating over UDP"* | **AWS Global Accelerator** |
| *"Cache static images, videos, and dynamic web content at edge locations to reduce origin load"*| **Amazon CloudFront** |
| *"Immediate failover (< 1 minute) between two AWS Regions without waiting for DNS cache expiration"*| **AWS Global Accelerator** |
| *"Collapse multiple concurrent requests to origin using a centralized caching layer"* | **Amazon CloudFront (Origin Shield)** |
| *"Execute lightweight JavaScript URL rewrites at the edge with sub-millisecond latency"* | **Amazon CloudFront (CloudFront Functions)** |
