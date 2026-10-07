---
title: "Decision Matrix: Disaster Recovery Tiers (RTO, RPO and Cost Trade-Offs)"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "Task 2.2: Design highly available and/or fault-tolerant architectures"
exam_weight: "Very High"
services:
  - AWS Elastic Disaster Recovery (AWS DRS)
  - Amazon Route 53
  - AWS Backup
tags:
  - decision-matrix
  - disaster-recovery
  - rto
  - rpo
  - pilot-light
  - warm-standby
  - active-active
last_updated: 2026-03-30
---

# ⚖️ Decision Matrix: 4 Cấp Độ Disaster Recovery (RTO / RPO Spectrum)

> Deep Dive chi tiết: [[Disaster-Recovery-Strategies-RTO-RPO]] | Trở về: [[00-Domain-2-MOC]]

---

## 1. Sơ đồ cây quyết định trong đề thi (Decision Flowchart)

```mermaid
flowchart TD
    Start([Chọn chiến lược Khắc phục Thảm họa - Disaster Recovery]) --> Q1{Yêu cầu RTO và RPO có xấp xỉ bằng 0<br/>và không chấp nhận bất kỳ thời gian chết nào không?}

    Q1 -- "CÓ (Kèm ngân sách dồi dào mức cao nhất)" --> A1["✅ Multi-Site Active-Active<br/>(Hệ thống full-capacity chạy song song ở 2+ Regions, Route 53 chia tải)"]
    Q1 -- "KHÔNG" --> Q2{RTO cho phép trong vòng VÀI PHÚT<br/>và chấp nhận duy trì một hệ thống THU NHỎ chạy 24/7?}

    Q2 -- "CÓ (Ngân sách trung bình)" --> A2["✅ Warm Standby<br/>(Hệ thống thu nhỏ scaled-down chạy 24/7 tại DR Region, scale-out khi có sự cố)"]
    Q2 -- "KHÔNG" --> Q3{RTO cho phép trong vòng HÀNG CHỤC PHÚT (10-30m)<br/>và muốn TIẾT KIỆM CHI PHÍ TỐI ĐA?}

    Q3 -- "CÓ (Chỉ muốn trả tiền cho Database sao chép nhỏ, Compute TẮT)" --> A3["✅ Pilot Light<br/>(Core DB chạy nhỏ và replicate liên tục, EC2 tắt hoàn toàn, bật khi thảm họa)"]
    Q3 -- "KHÔNG (Chấp nhận RTO/RPO nhiều giờ hoặc ngày, ưu tiên RẺ NHẤT)" --> A4["✅ Backup & Restore<br/>(Sao lưu định kỳ lên S3/AWS Backup, dựng lại toàn bộ hạ tầng từ con số 0)"]
```

---

## 2. Bảng so sánh ma trận 4 mô hình Disaster Recovery

| Tiêu chí so sánh | 1. Backup & Restore | 2. Pilot Light | 3. Warm Standby | 4. Multi-Site Active-Active |
| :--- | :--- | :--- | :--- | :--- |
| **RPO (Mất dữ liệu)** | **Vài giờ đến 24 giờ** | **Vài phút** | **Vài giây đến vài phút** | **Gần như bằng 0 (< 1s)** |
| **RTO (Thời gian chết)**| **Nhiều giờ đến nhiều ngày**| **10 đến 30 phút** | **Dưới 5 phút** | **Gần như bằng 0 (Tức thì)** |
| **Chi phí đầu tư** | **$ (Thấp nhất)** | **$$ (Tiết kiệm)** | **$$$ (Trung bình - Cao)** | **$$$$ (Đắt đỏ nhất)** |
| **Trạng thái Compute** | Không có máy chủ nào chạy | Máy chủ EC2 **TẮT HOÀN TOÀN** (chỉ có AMI/IaC) | Máy chủ EC2 **chạy thu nhỏ 24/7** (ví dụ: 2 t3.micro)| Máy chủ EC2 **chạy 100% công suất** ở cả 2 Region |
| **Trạng thái Database**| Không có DB chạy (chỉ lưu snapshot S3) | **DB chạy ở cấu hình nhỏ nhất** và replicate liên tục | **DB chạy sẵn sàng**, sync dữ liệu liên tục | **Database đồng bộ đa vùng 2 chiều** (Global Tables/Aurora) |
| **Hành động khi có sự cố**| Dựng lại toàn bộ hạ tầng từ IaC, restore DB | Bật Auto Scaling Group tạo EC2, scale-up DB | Route 53 chuyển hướng DNS, ASG scale-out thêm EC2 | Route 53 tự động gạch tên Region bị sập qua Health Check |
| **Độ phức tạp vận hành**| Thấp | Trung bình | Trung bình - Cao | Rất cao |

---

## 3. Bảng phản xạ nhanh từ khóa đề thi (Keyword Triggers)

| Cụm từ khóa trong đề thi SAA-C03 | Chiến lược DR được chọn |
| :--- | :--- |
| *"Disaster recovery with lowest cost, application can tolerate several hours of downtime"* | **Backup and Restore** |
| *"Maintain a minimal version of core database continuously replicated, compute launched on-demand"* | **Pilot Light** |
| *"Scaled-down version of environment running continuously, scaled up rapidly during disaster"* | **Warm Standby** |
| *"Zero downtime, business-critical application running full capacity across multiple regions"* | **Multi-Site Active-Active** |
| *"Disaster recovery for on-premises servers to AWS with RPO in seconds using continuous replication"* | **AWS Elastic Disaster Recovery (AWS DRS)** |
