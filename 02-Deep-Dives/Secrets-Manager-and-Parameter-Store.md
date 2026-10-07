---
title: "Secrets Manager and Parameter Store: Architecture and Selection Strategy"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.3: Determine appropriate data security controls"
exam_weight: "High"
services:
  - AWS Secrets Manager
  - AWS Systems Manager Parameter Store
tags:
  - secrets-manager
  - parameter-store
  - ssm
  - credentials
  - rotation
  - configuration
last_updated: 2026-03-30
---

# 🗝️ Secrets Manager vs Parameter Store: Quản lý Mật khẩu & Cấu hình

> Thuộc: [[Task-1.3-Determine-Data-Security-Controls]] | So sánh nhanh: [[Matrix-Secrets-vs-Parameter-Store]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Khái niệm cốt lõi (Core Concepts)

Trong kiến trúc bảo mật đám mây, một trong những lỗi nghiêm trọng nhất là lưu thông tin đăng nhập (Credentials, Passwords, API Keys) trực tiếp trong mã nguồn hoặc file cấu hình. AWS cung cấp 2 giải pháp quản lý bí mật:
- **AWS Secrets Manager:** Dịch vụ chuyên dụng để lưu trữ, mã hóa và **tự động xoay vòng (Automatic Rotation)** mật khẩu cơ sở dữ liệu và API keys.
- **AWS Systems Manager (SSM) Parameter Store:** Kho lưu trữ khóa-giá trị (Key-Value) phân cấp dùng để quản lý cấu hình ứng dụng (Configuration Data) và các biến môi trường nhạy cảm (`SecureString`).

---

## 2. Lưu đồ ra quyết định trong phòng thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Cần lưu trữ thông tin nhạy cảm/cấu hình]) --> NeedRotation{Có yêu cầu TỰ ĐỘNG XOAY VÒNG mật khẩu<br/>(Automatic Rotation) định kỳ không?}

    NeedRotation -- "CÓ (RDS / Aurora / API Key)" --> SM["✅ Chọn: AWS Secrets Manager<br/>(Tích hợp sẵn Lambda Rotation)"]
    NeedRotation -- "KHÔNG" --> NeedConfig{Dữ liệu là cấu hình ứng dụng thông thường<br/>hoặc chuỗi kết nối tĩnh?}

    NeedConfig -- "CÓ, và ưu tiên MIỄN PHÍ/TIẾT KIỆM" --> PS_Std["✅ Chọn: SSM Parameter Store Standard<br/>(Miễn phí, tối đa 4KB, hỗ trợ SecureString)"]
    NeedConfig -- "CÓ, nhưng cần dung lượng lớn hơn 4KB hoặc TTL" --> PS_Adv["✅ Chọn: SSM Parameter Store Advanced<br/>(0.05 USD/param/tháng, tối đa 8KB, có Parameter Policies)"]
```

---

## 3. So sánh chuyên sâu: Secrets Manager vs Parameter Store

| Tiêu chí so sánh | AWS Secrets Manager | SSM Parameter Store |
| :--- | :--- | :--- |
| **Mục đích chính** | **Quản lý bí mật & Mật khẩu database** | **Quản lý cấu hình & Biến môi trường** |
| **Tự động xoay mật khẩu (Auto Rotation)**| **Tích hợp sẵn (Out-of-the-box):**<br/>Cung cấp sẵn template AWS Lambda để tự đổi pass định kỳ cho Amazon RDS, Aurora, DocumentDB, Redshift. | **Không có sẵn:**<br/>Nếu muốn xoay phải tự viết Lambda thủ công và thiết lập EventBridge trigger phức tạp. |
| **Chi phí (Pricing)** | **$0.40 / secret / tháng** + $0.05 mỗi 10,000 API calls (Đắt hơn). | **Standard: HOÀN TOÀN MIỄN PHÍ**.<br/>Advanced: $0.05 / parameter / tháng. |
| **Dung lượng tối đa** | Lên tới **64 KB** cho mỗi secret. | Standard: **4 KB**.<br/>Advanced: **8 KB**. |
| **Sao chép đa vùng (Multi-Region)** | **Hỗ trợ sao chép tự động** (Secret Replication) sang nhiều Region chỉ bằng 1 cú click. | Không hỗ trợ tự động (phải tự sync). |
| **Tạo mật khẩu ngẫu nhiên** | Tích hợp sẵn API `GetRandomPassword` để sinh pass phức tạp. | Không có sẵn. |
| **Cơ chế mã hóa** | Bắt buộc mã hóa bằng **AWS KMS** (AWS Managed hoặc CMK). | Hỗ trợ 3 kiểu: `String`, `StringList`, và `SecureString` (mã hóa qua KMS). |

---

## 4. Cơ chế hoạt động của Automatic Rotation trong Secrets Manager

Khi bật tính năng xoay vòng tự động (ví dụ: mỗi 30 ngày):
1. **Secrets Manager** kích hoạt một hàm **AWS Lambda** chuyên dụng chạy trong VPC của cơ sở dữ liệu.
2. Lambda tạo một mật khẩu mới ngẫu nhiên và lưu ở trạng thái `AWSPENDING`.
3. Lambda đăng nhập vào Database bằng mật khẩu hiện tại (`AWSCURRENT`) và thực thi lệnh đổi mật khẩu của Database sang mật khẩu mới.
4. Lambda kiểm tra kết nối đăng nhập thành công với mật khẩu mới.
5. Secrets Manager chuyển nhãn mật khẩu mới thành `AWSCURRENT` và lưu mật khẩu cũ thành `AWSPREVIOUS` (phòng trường hợp cần rollback).

> **Từ khóa ăn điểm trong đề thi:** Bất cứ khi nào câu hỏi nhắc đến việc *"Tự động xoay mật khẩu database định kỳ 30/60/90 ngày với ít công sức quản trị nhất (Least operational effort)"* $\rightarrow$ **Đáp án luôn là AWS Secrets Manager**!

---

## ⚠️ Bẫy đề thi & Mẹo ghi nhớ (Exam Traps)

| Tình huống đề thi | Đáp án bẫy (SAI) | Đáp án chuẩn (ĐÚNG) |
| :--- | :--- | :--- |
| Lưu trữ mật khẩu RDS và yêu cầu tự động đổi mật khẩu mỗi 60 ngày mà không gây gián đoạn app | Lưu trong SSM Parameter Store dạng `SecureString` | Lưu trong **AWS Secrets Manager** và bật **Automatic Rotation** |
| Lưu trữ cấu hình ứng dụng gồm 500 biến môi trường với chi phí tối ưu nhất | Lưu 500 biến vào AWS Secrets Manager (Tốn 500 x $0.40 = $200/tháng) | Lưu vào **SSM Parameter Store Standard** (Hoàn toàn **miễn phí**) |
| Cần đồng bộ mật khẩu API Key cho ứng dụng chạy Active-Active ở 2 Region (us-east-1 và eu-west-1) | Viết script đồng bộ cronjob | Dùng **AWS Secrets Manager Multi-Region Secret Replication** |
| Ứng dụng cần lưu trữ một chứng chỉ SSL hoặc license file có dung lượng 60 KB dạng chuỗi | Dùng SSM Parameter Store (Tối đa chỉ được 8 KB ở tầng Advanced) | Dùng **AWS Secrets Manager** (Hỗ trợ dung lượng tới 64 KB) |

---

## 🔗 Liên kết liên quan
- [[Task-1.3-Determine-Data-Security-Controls]]
- [[Matrix-Secrets-vs-Parameter-Store]]
- [[AWS-KMS-and-CloudHSM]]
- [[Domain-1-Scenario-Walkthroughs]]
