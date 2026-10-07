---
title: "Decision Matrix: AWS KMS Key Types and Cryptographic Architecture"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.3: Determine appropriate data security controls"
exam_weight: "High"
services:
  - AWS Key Management Service (AWS KMS)
  - AWS CloudHSM
tags:
  - decision-matrix
  - kms
  - cloudhsm
  - encryption
  - cmk
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: AWS KMS Key Types & Cryptographic Options

> Deep Dive chi tiết: [[AWS-KMS-and-CloudHSM]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn giải pháp mã hóa dữ liệu]) --> Q1{Cần chuẩn FIPS 140-2 Level 3 ĐỘC LẬP<br/>và toàn quyền kiểm soát thiết bị phần cứng?}

    Q1 -- "CÓ" --> A1["✅ AWS CloudHSM<br/>(Single-tenant, chuẩn phần cứng chuyên dụng, quản lý qua PKCS#11)"]
    Q1 -- "KHÔNG" --> Q2{Cần kiểm soát Key Policy, tự đặt Alias,<br/>hoặc kiểm toán chi tiết qua CloudTrail?}

    Q2 -- "CÓ" --> A2["✅ KMS Customer Managed Key (CMK)<br/>($1/tháng/key, toàn quyền phân quyền và bật/tắt xoay khóa)"]
    Q2 -- "KHÔNG" --> Q3{Có muốn chi phí $0 cho việc tạo khóa<br/>và chấp nhận dùng khóa mặc định của AWS?}

    Q3 -- "CÓ (theo từng dịch vụ aws/s3, aws/rds)" --> A3["✅ KMS AWS Managed Key<br/>(Miễn phí tạo key, tự động xoay mỗi 1 năm, không sửa được policy)"]
    Q3 -- "CÓ (mã hóa nội bộ ngầm)" --> A4["✅ AWS Owned Key<br/>(Miễn phí 100%, không hiện trong Console)"]
```

---

## 2. Bảng so sánh 5 loại khóa mã hóa trên AWS

| Đặc tính kỹ thuật | AWS Owned Keys | AWS Managed Keys | Customer Managed Keys (CMK) | AWS CloudHSM | External Key Store (XKS) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Phí tạo khóa** | **Miễn phí ($0)** | **Miễn phí ($0)** | **$1 / key / tháng** | Hàng nghìn USD/tháng | Phí KMS + Chi phí HSM On-prem |
| **Hiển thị Console**| Không hiển thị | Có (`aws/s3`, `aws/ebs`) | Tự đặt tên `alias/xxxx` | Quản lý qua HSM CLI | Thông qua KMS Console |
| **Sửa Key Policy** | Không thể | Không thể | **Toàn quyền cấu hình** | Toàn quyền kiểm soát | Kiểm soát tại HSM ngoài |
| **Xoay khóa (Rotation)**| AWS tự quản lý | **Tự động mỗi 1 năm** | Tùy chọn (Bật xoay 1 năm hoặc thủ công)| Khách hàng tự xoay | Khách hàng tự xoay |
| **Xóa khóa** | Không thể | Không thể | **Có** (Chờ 7-30 ngày) | **Có** (Xóa tức thì) | Khách hàng tự xóa |
| **Chuẩn phần cứng** | FIPS 140-2 L2/L3 | FIPS 140-2 L2/L3 | FIPS 140-2 L2/L3 | **FIPS 140-2 Level 3 (Dedicated)** | Tùy chọn On-premise HSM |

---

## 3. Symmetric vs Asymmetric Keys

| Tiêu chí | Khóa đối xứng (Symmetric Keys) | Khóa bất đối xứng (Asymmetric Keys) |
| :--- | :--- | :--- |
| **Thuật toán** | **AES-256** | **RSA** hoặc **ECC (Elliptic Curve)** |
| **Cấu trúc khóa** | Duy nhất 1 khóa dùng chung cho cả Mã hóa và Giải mã. | Gồm 1 cặp: **Public Key** (Mã hóa / Verify) và **Private Key** (Giải mã / Sign). |
| **Khả năng tải về**| **Không bao giờ rời khỏi KMS** ở dạng unencrypted. | **Public Key có thể tải về** để chia sẻ ra bên ngoài AWS. |
| **Ứng dụng tiêu biểu**| Mã hóa dữ liệu lưu trữ S3, EBS, RDS (chiếm 95% câu hỏi SAA-C03). | Ký số tài liệu điện tử, mã hóa dữ liệu từ đối tác ngoài AWS gửi vào mà không cấp quyền truy cập AWS. |

---

## 4. Single-Region vs Multi-Region Keys

- **KMS thông thường (Single-Region):** Gắn liền với Region tạo ra. Không thể giải mã dữ liệu ở Region khác.
- **KMS Multi-Region Keys:** Khóa có cùng Key ID và Key Material được nhân bản giữa nhiều Region.
- **Quy tắc thi cử:** Chọn **Multi-Region Keys** khi câu hỏi mô tả kịch bản **Multi-Region Disaster Recovery, DynamoDB Global Tables, hoặc Aurora Global Databases** yêu cầu mã hóa đồng nhất mà không cần decrypt rồi re-encrypt qua đường truyền mạng.
