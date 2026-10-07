---
title: "Decision Matrix: Security Groups vs Network ACLs (NACL)"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.2: Design secure workloads and applications"
exam_weight: "Very High"
services:
  - Amazon VPC Security Groups
  - Amazon VPC Network ACLs
tags:
  - decision-matrix
  - security-groups
  - nacl
  - vpc
  - networking
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: Security Groups vs Network ACLs (NACL)

> Deep Dive chi tiết: [[VPC-Network-Security-Layers]] | Trở về: [[00-Domain-1-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Cần kiểm soát lưu lượng mạng trong VPC]) --> Q1{Cần CHẶN một địa chỉ IP độc hại cụ thể<br/>(Explicit DENY single IP)?}

    Q1 -- "CÓ" --> A1["✅ Network ACL (NACL)<br/>(Thêm DENY Rule với số thứ tự ưu tiên nhỏ hơn Allow)"]
    Q1 -- "KHÔNG" --> Q2{Cần áp dụng luật ở cấp độ máy chủ/ENI<br/>hay cho phép tham chiếu theo ID nhóm bảo mật?}

    Q2 -- "CÓ (Cấp Instance / Tham chiếu SG ID)" --> A2["✅ Security Group (SG)<br/>(Stateful, chỉ Allow, hỗ trợ SG Chaining sg-xxxx)"]
    Q2 -- "KHÔNG (Cần rào chắn toàn bộ Subnet)" --> A3["✅ Network ACL (NACL)<br/>(Stateless, quản lý toàn bộ Subnet, cần mở Ephemeral Ports)"]
```

---

## 2. Bảng so sánh toàn diện 10 tiêu chí

| Tiêu chí                           | Security Group (SG)                                                                   | Network ACL (NACL)                                                                         |
| :--------------------------------- | :------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------- |
| **Cấp độ hoạt động**               | **Instance / ENI** (Card mạng ảo)                                                     | **Subnet** (Toàn bộ subnet)                                                                |
| **Bản chất trạng thái**            | **Stateful (Có trạng thái):**<br/>Gói tin vào được thì tự động ra được, và ngược lại. | **Stateless (Không có trạng thái):**<br/>Chiều vào và chiều ra phải được cấu hình độc lập. |
| **Loại luật (Rule Types)**         | **Chỉ có ALLOW** (Mặc định ngầm hiểu là Deny hết).                                    | **Cả ALLOW và DENY**.                                                                      |
| **Thứ tự đánh giá**                | Đánh giá **đồng thời tất cả các luật**.                                               | Đánh giá theo **số thứ tự (Rule # 1-32766)** từ nhỏ đến lớn, gặp luật khớp là dừng ngay.   |
| **Luật mặc định (Default)**        | Inbound: Deny all.<br/>Outbound: Allow all.                                           | Default NACL: Allow all Inbound & Outbound.<br/>Custom NACL: Deny all Inbound & Outbound.  |
| **Nguồn/Đích hỗ trợ**              | Hỗ trợ CIDR Block, Prefix List, hoặc **ID của SG khác (`sg-xxxx`)**.                  | **Chỉ hỗ trợ CIDR Block** (không nhận diện được SG ID).                                    |
| **Cổng hồi đáp (Ephemeral Ports)** | Tự động mở ngầm.                                                                      | **Bắt buộc mở thủ công dải cổng 1024-65535** ở Outbound cho lưu lượng phản hồi.            |
| **Khả năng chặn IP đơn**           | **KHÔNG THỂ** (vì không có luật Deny).                                                | **CÓ THỂ** (Tạo luật Deny IP x.x.x.x/32).                                                  |
| **Gắn với tài nguyên**             | 1 ENI có thể gắn tối đa **5 Security Groups**.                                        | 1 Subnet **chỉ được gắn duy nhất với 1 NACL** tại một thời điểm.                           |
| **Tác động khi sửa đổi**           | Áp dụng **ngay lập tức** cho các kết nối hiện tại.                                    | Áp dụng **ngay lập tức** cho toàn bộ lưu lượng mạng của Subnet.                            |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Thành phần mạng được chọn |
| :--- | :--- |
| *"Block a specific malicious IP address or subnet"* | **Network ACL (NACL) Inbound Deny rule** |
| *"Stateful firewall at the instance level"* | **Security Group** |
| *"Reference another security group as the source"* | **Security Group (SG Chaining)** |
| *"Stateless subnet-level boundary filtering"* | **Network ACL (NACL)** |
| *"EC2 cannot receive response packets despite inbound port 80 open"* | **Missing Ephemeral Ports (1024-65535) in Outbound NACL** |
| *"Prevent web servers from directly exposing database port 3306"* | **DB Security Group: Allow port 3306 from Web Security Group ID** |
