---
title: "Decision Matrix: CloudFront Functions vs Lambda@Edge"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "Task 3.4: Determine high-performing and/or scalable network architectures"
exam_weight: "High"
services:
  - Amazon CloudFront
  - CloudFront Functions
  - Lambda@Edge
tags:
  - decision-matrix
  - cloudfront-functions
  - lambda-at-edge
  - edge-compute
  - serverless
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: CloudFront Functions vs Lambda@Edge

> Deep Dive chi tiết: [[CloudFront-vs-Global-Accelerator-Edge-Performance]] | Trở về: [[00-Domain-3-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn công nghệ tính toán tại biên Edge Compute]) --> Q1{Có cần GỌI MẠNG RA NGOÀI (Gọi API / Database),<br/>ĐỌC HTTP REQUEST BODY, hoặc chạy code Node.js/Python nặng?}

    Q1 -- "CÓ" --> A1["✅ Lambda@Edge<br/>(Node.js / Python, thời gian chạy tới 30s, hỗ trợ gọi mạng ngoài, đọc Body)"]

    Q1 -- "KHÔNG" --> Q2{Chỉ cần chỉnh sửa Headers, chuyển hướng URL Redirect / Rewrite,<br/>chuẩn hóa Cache Key với JavaScript siêu nhẹ và chi phí rẻ nhất?}

    Q2 -- "CÓ" --> A2["✅ CloudFront Functions<br/>(JavaScript thuần ECMAScript 5.1, chạy tại 400+ Edge Locations, dưới 1ms, rẻ hơn 6 lần)"]
```

---

## 2. Bảng so sánh toàn diện 10 tiêu chí kỹ thuật

| Tiêu chí kỹ thuật | CloudFront Functions | Lambda@Edge |
| :--- | :--- | :--- |
| **Ngôn ngữ hỗ trợ** | **JavaScript (ECMAScript 5.1 compliant)** | **Node.js, Python** |
| **Vị trí thực thi** | **400+ CloudFront Edge Locations** (Sát người dùng nhất) | **13 Regional Edge Caches** |
| **Thời gian chạy tối đa** | **Dưới 1 mili-giây (< 1ms)** | **5 giây (Viewer triggers) / 30 giây (Origin triggers)** |
| **Dung lượng RAM tối đa** | **2 MB** | **128 MB đến 10,240 MB** |
| **Dung lượng gói Code** | Tối đa 10 KB | Tối đa 1 MB (Viewer) / 50 MB (Origin) |
| **Truy cập mạng bên ngoài**| **KHÔNG ĐƯỢC PHÉP** (Không thể gọi API, DB ngoài) | **CÓ ĐƯỢC PHÉP** (Truy cập Internet, VPC, DynamoDB, API) |
| **Đọc HTTP Request Body**| **KHÔNG THỂ** (Chỉ đọc URL, Headers, Query, Cookies) | **CÓ THỂ** (Đọc, chỉnh sửa, hoặc sinh ra HTTP Body) |
| **Các điểm kích hoạt (Triggers)**| **Chỉ 2:** Viewer Request & Viewer Response | **Cả 4:** Viewer Request/Response & Origin Request/Response |
| **Mô hình chi phí** | **Rẻ hơn 6 lần** (~$0.10 / 1 triệu requests) | Chi phí Lambda tiêu chuẩn (~$0.60 / 1 triệu requests) |
| **Ứng dụng tiêu biểu** | URL Rewrite/Redirect, Header normalization, Cache key manipulation | Xác thực JWT token phức tạp, thay đổi kích thước ảnh theo thiết bị |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ Edge Compute được chọn |
| :--- | :--- |
| *"Lightweight JavaScript execution at the edge to rewrite request URLs with sub-millisecond latency"*| **CloudFront Functions** |
| *"Normalize HTTP request headers or query strings to optimize cache hit ratio at lowest cost"* | **CloudFront Functions** |
| *"Authenticate user requests at the edge by inspecting JWT tokens and making external network calls"*| **Lambda@Edge** |
| *"Resize and optimize images dynamically at the edge based on viewer User-Agent headers"* | **Lambda@Edge** |
| *"Inspect or modify the HTTP request body before forwarding to origin"* | **Lambda@Edge** |
| *"Execute logic on Origin Request or Origin Response events"* | **Lambda@Edge** |
