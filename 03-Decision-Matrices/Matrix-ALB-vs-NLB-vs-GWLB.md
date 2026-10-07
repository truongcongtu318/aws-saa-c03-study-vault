---
title: "Decision Matrix: ALB vs NLB vs Gateway Load Balancer"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "Task 2.2: Design highly available and/or fault-tolerant architectures"
exam_weight: "Very High"
services:
  - Application Load Balancer (ALB)
  - Network Load Balancer (NLB)
  - Gateway Load Balancer (GWLB)
tags:
  - decision-matrix
  - load-balancer
  - alb
  - nlb
  - gwlb
  - comparison
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Application Load Balancer vs NLB vs GWLB

> Deep Dive chi tiết: [[Elastic-Load-Balancing-Deep-Dive]] | Trở về: [[00-Domain-2-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn loại Elastic Load Balancer]) --> Q1{Cần điều hướng lưu lượng qua cụm tường lửa ảo<br/>hoặc thiết bị IDS/IPS của bên thứ ba?}

    Q1 -- "CÓ" --> A1["✅ Gateway Load Balancer (GWLB)<br/>(Layer 3/4, giao thức GENEVE port 6081)"]
    Q1 -- "KHÔNG" --> Q2{Ứng dụng cần ĐỊA CHỈ IP TĨNH CỐ ĐỊNH (Static IP / Elastic IP)<br/>hoặc xử lý hàng triệu req/s với độ trễ micro-giây?}

    Q2 -- "CÓ (TCP/UDP, Cần IP tĩnh, Game/Tài chính)" --> A2["✅ Network Load Balancer (NLB)<br/>(Layer 4, siêu hiệu năng, cung cấp 1 IP tĩnh/AZ)"]
    Q2 -- "KHÔNG (HTTP/HTTPS, Routing theo Path/Host/Query)" --> A3["✅ Application Load Balancer (ALB)<br/>(Layer 7, hỗ trợ SNI, xác thực Cognito, Cross-Zone miễn phí)"]
```

---

## 2. Bảng so sánh toàn diện 12 tiêu chí kỹ thuật

| Tiêu chí | Application Load Balancer (ALB) | Network Load Balancer (NLB) | Gateway Load Balancer (GWLB) |
| :--- | :--- | :--- | :--- |
| **Tầng OSI** | **Layer 7** (Application) | **Layer 4** (Transport) | **Layer 3 & 4** (Network Gateway) |
| **Giao thức đầu vào** | HTTP, HTTPS, gRPC | TCP, UDP, TLS | IP (GENEVE encapsulation) |
| **Địa chỉ IP cung cấp** | **DNS Name động** (Không hỗ trợ gắn Elastic IP) | **Static IP / Elastic IP cố định cho từng AZ** | ENI Endpoint riêng biệt |
| **Khả năng định tuyến** | Path (`/api`), Host (`app.com`), Header, Query | Chỉ định tuyến theo Port & Protocol | Chuyển tiếp toàn bộ gói tin IP |
| **Độ trễ** | Mili-giây (Thích hợp cho Web) | **Micro-giây** (Cực thấp) | Trong suốt |
| **Server Name Indication (SNI)**| **Hỗ trợ** (Gắn nhiều chứng chỉ SSL trên 1 Listener) | **Hỗ trợ** (Gắn nhiều chứng chỉ TLS) | Không áp dụng |
| **Xác thực người dùng** | **Tích hợp sẵn Cognito / OIDC** | Không hỗ trợ | Không hỗ trợ |
| **Giữ nguyên Client IP** | Ghi vào Header `X-Forwarded-For` | **Giữ nguyên trực tiếp trong gói tin IP** | Giữ nguyên qua GENEVE header |
| **Cross-Zone Balancing** | **Mặc định BẬT (Miễn phí 100%)** | **Mặc định TẮT** (Tính phí data transfer nếu bật) | Không áp dụng |
| **Chuyển hướng (Redirect)**| Hỗ trợ HTTP sang HTTPS, đổi URL | Không hỗ trợ | Không hỗ trợ |
| **Loại Target hỗ trợ** | EC2, ECS, IP, Lambda | EC2, ECS, IP, **ALB (NLB bọc ngoài ALB)** | EC2, IP (Virtual Appliances) |
| **Ứng dụng tiêu biểu** | Microservices, Web application, REST API | Gaming, Kết nối IoT, Đối tác cần whitelist IP | Tường lửa Palo Alto, Fortinet, Check Point |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Loại Load Balancer được chọn |
| :--- | :--- |
| *"Provide a static IP address or Elastic IP for firewall whitelisting"* | **Network Load Balancer (NLB)** |
| *"Host multiple domain names with different SSL certificates at lowest cost"* | **Application Load Balancer (ALB) với SNI** |
| *"Route requests to different target groups based on URL path e.g. /orders and /users"* | **Application Load Balancer (ALB)** |
| *"Handle millions of requests per second with ultra-low latency"* | **Network Load Balancer (NLB)** |
| *"Deploy a fleet of 3rd-party network firewalls transparently using GENEVE protocol"* | **Gateway Load Balancer (GWLB)** |
| *"Authenticate end-users before reaching application servers"* | **Application Load Balancer (ALB với Cognito/OIDC)** |
