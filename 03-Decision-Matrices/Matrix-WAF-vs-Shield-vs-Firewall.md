---
title: "Decision Matrix: AWS WAF vs Shield vs Network Firewall vs Firewall Manager"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.2: Design secure workloads and applications"
exam_weight: "High"
services:
  - AWS WAF
  - AWS Shield
  - AWS Network Firewall
  - AWS Firewall Manager
tags:
  - decision-matrix
  - waf
  - shield
  - network-firewall
  - firewall-manager
  - ddos
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: WAF vs Shield vs Network Firewall vs Firewall Manager

> Deep Dive chi tiết: [[Edge-and-App-Defense]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Cần bảo vệ an ninh mạng / ứng dụng]) --> Q1{Cần tự động hóa triển khai luật an ninh<br/>tập trung trên HÀNG TRĂM TÀI KHOẢN trong Organization?}

    Q1 -- "CÓ" --> A1["✅ AWS Firewall Manager<br/>(Quản trị tập trung WAF, Shield, SG qua AWS Organizations)"]
    Q1 -- "KHÔNG" --> Q2{Mục tiêu bảo vệ ở TẦNG NÀO của mô hình mạng?}

    Q2 -- "Tầng 7 (Ứng dụng Web / HTTP / SQLi / XSS / Rate Limit)" --> A2["✅ AWS WAF<br/>(Gắn vào CloudFront, ALB, API Gateway, AppSync)"]
    Q2 -- "Tầng 3 & 4 (Chống tấn công DDoS hạ tầng / SYN Flood)" --> Q3{Có cần hỗ trợ đội phản ứng nhanh 24/7<br/>và bảo vệ chống sốc chi phí (Cost Protection)?}
    Q2 -- "Tầng VPC (Lọc gói tin sâu DPI / Lọc Domain URL ra ngoài / Suricata)" --> A3["✅ AWS Network Firewall<br/>(Triển khai ở cấp độ VPC, lọc Stateful/Stateless)"]

    Q3 -- "CÓ (Trả phí 3,000 USD/tháng)" --> A4["✅ AWS Shield Advanced<br/>(Hỗ trợ DRT Team, bảo vệ chi phí EC2/ALB bị scale đột biến)"]
    Q3 -- "KHÔNG (Cần bảo vệ cơ bản miễn phí)" --> A5["✅ AWS Shield Standard<br/>(Bật sẵn mặc định 100%, bảo vệ L3/L4 cơ bản)"]
```

---

## 2. Bảng so sánh 5 giải pháp phòng thủ biên & mạng

| Tiêu chí | AWS WAF | AWS Shield Standard | AWS Shield Advanced | AWS Network Firewall | AWS Firewall Manager |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Tầng bảo vệ (OSI)**| **Layer 7** (Application) | **Layer 3 & 4** (Network/Transport) | **Layer 3, 4 & 7** | **Layer 3 đến Layer 7** | Quản trị chính sách đa tầng |
| **Mục đích cốt lõi** | Chặn khai thác lỗ hổng Web | Chống DDoS hạ tầng phổ thông | Chống DDoS nâng cao + Hỗ trợ chuyên gia | Kiểm tra gói tin sâu (DPI) & lọc URL trong VPC | Triển khai luật tập trung toàn doanh nghiệp |
| **Mối đe dọa xử lý** | SQLi, XSS, HTTP Flood, Scraping, Bad Bots | SYN Flood, UDP Reflection | Tấn công DDoS phức tạp quy mô lớn | Malware, C2 callbacks, Domain cấm, Snort/Suricata rules | Sai lệch cấu hình (Configuration drift) |
| **Gắn vào đâu?** | CloudFront, ALB, API Gateway, AppSync | Tự động trên mọi tài nguyên | CloudFront, ALB, Route 53, Global Accel | Subnet chuyên dụng trong VPC | Toàn bộ tài khoản trong AWS Organizations |
| **Chi phí** | Rẻ ($5/ACL + request) | **Miễn phí ($0)** | **$3,000 / tháng** + phí data | Tính theo giờ firewall endpoint + GB data | Tính theo số chính sách áp dụng |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Dịch vụ được chọn |
| :--- | :--- |
| *"Protect web application from SQL injection (SQLi) and cross-site scripting (XSS)"* | **AWS WAF** |
| *"Rate limit requests from abusive IP addresses at layer 7"* | **AWS WAF (Rate-based rule)** |
| *"24/7 dedicated support team (DRT) to assist during active DDoS attacks"* | **AWS Shield Advanced** |
| *"Financial cost protection against bill spikes caused by DDoS attacks"* | **AWS Shield Advanced** |
| *"Filter outbound VPC traffic based on fully qualified domain names (FQDN)"* | **AWS Network Firewall** |
| *"Enforce consistent security group rules and WAF policies across all AWS accounts"* | **AWS Firewall Manager** |
| *"Prevent SYN flood attacks at no additional cost"* | **AWS Shield Standard** |
