---
title: "Domain 2 Quick Recall Flashcards: 30 Active-Recall Questions for Exam Readiness"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "All Tasks (2.1, 2.2)"
exam_weight: "Very High"
tags:
  - flashcards
  - active-recall
  - revision
  - quick-review
  - domain2
last_updated: 2026-03-30
---

# 🧠 Domain 2 Quick Recall: 30 Câu Hỏi Phản Xạ Nhanh

> Trở về: [[00-Domain-2-MOC]]  
> **Hướng dẫn ôn tập:** Đọc câu hỏi, tự nhẩm câu trả lời trong đầu trước khi bấm vào thẻ `<details>` để xem đáp án giải thích.

---

### Q1: Visibility Timeout trong Amazon SQS là gì? Mặc định là bao lâu và tối đa là bao lâu?
<details>
<summary>👉 Xem đáp án</summary>
Là khoảng thời gian tin nhắn bị <b>ẩn khỏi các worker khác</b> sau khi một worker đã nhận nó. Mặc định là <b>30 giây</b>, tối đa cấu hình được lên tới <b>12 giờ</b>.
</details>

---

### Q2: Nếu một worker EC2 cần 3 phút để xử lý xong một tin nhắn trong SQS, worker nên làm gì để tránh việc worker khác nhận lại tin nhắn đó sau 30 giây?
<details>
<summary>👉 Xem đáp án</summary>
Worker phải chủ động gọi API <b><code>ChangeMessageVisibility</code></b> để gia hạn thêm thời gian Visibility Timeout trong quá trình xử lý.
</details>

---

### Q3: SQS Long Polling hoạt động như thế nào và đem lại lợi ích gì so với Short Polling?
<details>
<summary>👉 Xem đáp án</summary>
Long Polling giữ kết nối mở và chờ tới khi có tin nhắn mới (cấu hình <code>WaitTimeSeconds</code> từ 1 đến 20s). Giúp <b>giảm chi phí gọi API calls rỗng</b> và giảm độ trễ khi có tin nhắn mới tới hàng đợi.
</details>

---

### Q4: Muốn gửi một thông điệp có kích thước 50 MB qua Amazon SQS (vượt quá giới hạn 256 KB), giải pháp là gì?
<details>
<summary>👉 Xem đáp án</summary>
Sử dụng <b>Amazon SQS Extended Client Library</b> (tự động lưu payload lớn vào <b>Amazon S3</b> và chỉ truyền con trỏ tham chiếu qua SQS).
</details>

---

### Q5: Khi nào cần chọn SQS FIFO Queue thay vì SQS Standard Queue? Tên hàng đợi FIFO bắt buộc phải có định dạng gì?
<details>
<summary>👉 Xem đáp án</summary>
Khi nghiệp vụ yêu cầu <b>thứ tự xử lý tuyệt đối (First-In-First-Out)</b> và <b>không được trùng lặp tin nhắn (Exactly-once)</b>. Tên queue bắt buộc phải kết thúc bằng hậu tố <b><code>.fifo</code></b>.
</details>

---

### Q6: Mô hình SNS Fan-out kết hợp với SQS là gì?
<details>
<summary>👉 Xem đáp án</summary>
Publisher chỉ cần gửi message 1 lần vào một <b>SNS Topic</b>, sau đó nhiều hàng đợi SQS độc lập đăng ký (subscribe) vào Topic đó để cùng nhận bản sao message và xử lý song song bất đồng bộ.
</details>

---

### Q7: SNS Message Filtering giúp ích gì cho kiến trúc microservices?
<details>
<summary>👉 Xem đáp án</summary>
Cho phép từng subscriber chỉ nhận những tin nhắn thỏa mãn thuộc tính JSON quy định sẵn (Filter Policy), giúp tránh việc worker phải nhận toàn bộ tin nhắn rồi tự code logic bỏ qua.
</details>

---

### Q8: Dịch vụ nào cho phép phát lại (Replay) các sự kiện trong quá khứ sau khi đã sửa xong lỗi ở hệ thống downstream?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon EventBridge Archive & Replay</b>.
</details>

---

### Q9: Doanh nghiệp muốn di chuyển ứng dụng cũ đang dùng RabbitMQ lên AWS mà không sửa code ứng dụng, chọn dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon MQ</b> (dịch vụ Managed Message Broker hỗ trợ RabbitMQ và Apache ActiveMQ).
</details>

---

### Q10: Phân biệt sự khác biệt lớn nhất giữa Standard Workflows và Express Workflows trong AWS Step Functions?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Standard Workflows:</b> Chạy tối đa 1 năm, tính phí theo số bước chuyển trạng thái (State transitions), lịch sử lưu trữ chi tiết.
- <b>Express Workflows:</b> Chạy tối đa 5 phút, xử lý throughput siêu cao (>100k req/s), tính phí theo thời gian chạy và bộ nhớ RAM.
</details>

---

### Q11: Muốn chạy nhiều website HTTPS có các chứng chỉ SSL khác nhau trên cùng một Application Load Balancer với chi phí tối ưu, dùng tính năng gì?
<details>
<summary>👉 Xem đáp án</summary>
Tính năng <b>Server Name Indication (SNI)</b> trên Listener của <b>Application Load Balancer (ALB)</b>.
</details>

---

### Q12: Đối tác yêu cầu 2 địa chỉ IP tĩnh cố định để whitelist trên tường lửa on-premise, nên chọn loại Load Balancer nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>Network Load Balancer (NLB)</b> (hỗ trợ gán Elastic IP cố định cho từng Availability Zone).
</details>

---

### Q13: Cross-Zone Load Balancing mặc định BẬT hay TẮT trên ALB? Mặc định BẬT hay TẮT trên NLB?
<details>
<summary>👉 Xem đáp án</summary>
- Trên <b>ALB:</b> Mặc định luôn <b>BẬT (Enabled)</b> và <b>MIỄN PHÍ</b>.
- Trên <b>NLB:</b> Mặc định <b>TẮT (Disabled)</b>; nếu bật sẽ bị <b>tính phí Inter-AZ data transfer</b>.
</details>

---

### Q14: Tính năng Deregistration Delay (Connection Draining) trên Load Balancer có tác dụng gì? Mặc định là bao lâu?
<details>
<summary>👉 Xem đáp án</summary>
Cho phép các kết nối đang xử lý dang dở (in-flight requests) hoàn thành nốt trước khi instance bị gỡ hoàn toàn khỏi Target Group (mặc định <b>300 giây</b>), tránh làm đứt kết nối của người dùng.
</details>

---

### Q15: Thứ tự ưu tiên mặc định của Auto Scaling Group khi chọn máy chủ để tắt (Termination Policy) lúc Scale-In là gì?
<details>
<summary>👉 Xem đáp án</summary>
1. Tìm Availability Zone có <b>nhiều instance nhất</b>.
2. Tìm instance sử dụng <b>Launch Template / Configuration cũ nhất</b>.
3. Tìm instance <b>gần mốc tính tiền giờ tiếp theo nhất</b>.
</details>

---

### Q16: Muốn thực thi một script sao lưu log từ ổ cứng cục bộ lên S3 trước khi máy chủ EC2 bị Auto Scaling hủy bỏ, dùng tính năng gì?
<details>
<summary>👉 Xem đáp án</summary>
<b>Auto Scaling Lifecycle Hook</b> cho sự kiện <b><code>autoscaling:EC2_INSTANCE_TERMINATING</code></b> (đưa máy chủ vào trạng thái <code>Terminating:Wait</code>).
</details>

---

### Q17: Điểm khác biệt quan trọng nhất giữa RDS Multi-AZ Deployment và RDS Read Replicas là gì?
<details>
<summary>👉 Xem đáp án</summary>
- <b>RDS Multi-AZ:</b> Sao chép đồng bộ sang Standby ở AZ khác để phục vụ <b>High Availability và Failover tự động</b> (Standby <b>KHÔNG THỂ DÙNG ĐỂ ĐỌC</b>).
- <b>RDS Read Replicas:</b> Sao chép bất đồng bộ để phục vụ <b>tăng hiệu năng đọc (Read scaling)</b>, đọc được thoải mái.
</details>

---

### Q18: Cơ chế tự động Failover của RDS Multi-AZ diễn ra như thế nào? Ứng dụng có cần đổi chuỗi kết nối (Connection String) không?
<details>
<summary>👉 Xem đáp án</summary>
RDS tự động đổi <b>bản ghi CNAME DNS</b> của Endpoint trỏ sang Standby instance trong vòng 60-120 giây. Ứng dụng <b>KHÔNG CẦN</b> đổi chuỗi kết nối.
</details>

---

### Q19: Kiến trúc lưu trữ của Amazon Aurora có gì đặc biệt? Chịu được mất bao nhiêu bản sao mà vẫn ghi được?
<details>
<summary>👉 Xem đáp án</summary>
Aurora tự động lưu trữ <b>6 bản sao dữ liệu trên 3 Availability Zones</b>. Chịu được mất <b>2 bản sao (4/6 Write Quorum)</b> mà vẫn ghi được, và mất <b>3 bản sao (3/6 Read Quorum)</b> mà vẫn đọc được.
</details>

---

### Q20: Amazon Aurora Global Database cung cấp độ trễ sao chép dữ liệu (RPO) và thời gian failover (RTO) là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
- <b>RPO:</b> Dưới <b>1 giây</b> (sao chép ngầm tầng storage).
- <b>RTO:</b> Dưới <b>1 phút</b> (khi promote secondary cluster lên standalone).
</details>

---

### Q21: DynamoDB Global Tables cung cấp mô hình sao chép dữ liệu gì? Yêu cầu điều kiện gì trước khi bật?
<details>
<summary>👉 Xem đáp án</summary>
Cung cấp mô hình <b>Multi-Region Active-Active</b> (cho phép đọc và ghi tại tất cả các Region). Bắt buộc phải bật <b>DynamoDB Streams</b> trước khi tạo Global Table.
</details>

---

### Q22: Tính năng Point-In-Time Recovery (PITR) của DynamoDB cho phép khôi phục dữ liệu trong vòng bao nhiêu ngày?
<details>
<summary>👉 Xem đáp án</summary>
Trong vòng <b>35 ngày gần nhất</b> với độ chính xác chuẩn đến từng giây, và <b>không làm ảnh hưởng đến hiệu năng</b> của bảng đang chạy.
</details>

---

### Q23: Tại sao không thể tạo bản ghi CNAME để trỏ tên miền gốc (Zone Apex ví dụ: `example.com`) tới Application Load Balancer? Giải pháp là gì?
<details>
<summary>👉 Xem đáp án</summary>
Chuẩn DNS cấm tạo CNAME cho Zone Apex. Giải pháp là sử dụng <b>Amazon Route 53 Alias Record (A-Record)</b> (trỏ trực tiếp tới ALB và hoàn toàn miễn phí truy vấn DNS).
</details>

---

### Q24: Muốn điều hướng người dùng quốc tế tới Region có độ trễ mạng thấp nhất, dùng Routing Policy nào của Route 53?
<details>
<summary>👉 Xem đáp án</summary>
<b>Latency-based Routing Policy</b>.
</details>

---

### Q25: Muốn triển khai thử nghiệm A/B Testing hoặc Canary Deployment gửi 10% lưu lượng tới app v2 và 90% tới app v1, dùng Routing Policy nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>Weighted Routing Policy</b>.
</details>

---

### Q26: Amazon EBS Volume có thể gắn trực tiếp vào một máy chủ EC2 ở Availability Zone khác được không? Giải pháp di chuyển là gì?
<details>
<summary>👉 Xem đáp án</summary>
<b>KHÔNG</b>. EBS Volume bị khóa chặt trong 1 AZ duy nhất. Muốn di chuyển, phải chụp một bản <b>EBS Snapshot</b> (lưu trên S3), sau đó tạo một Volume mới từ Snapshot đó tại AZ mục tiêu.
</details>

---

### Q27: Ràng buộc của tính năng Amazon EBS Multi-Attach là gì?
<details>
<summary>👉 Xem đáp án</summary>
1. Chỉ hỗ trợ ổ đĩa Provisioned IOPS SSD: <b><code>io1</code> và <code>io2</code></b>.
2. Các máy chủ EC2 bắt buộc phải nằm trong <b>CÙNG MỘT AVAILABILITY ZONE</b>.
</details>

---

### Q28: Muốn hàng trăm máy chủ EC2 Linux ở nhiều AZ khác nhau cùng đọc ghi đồng thời vào một hệ thống tệp chia sẻ POSIX, dùng dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon EFS (Elastic File System)</b> (mặc định là Multi-AZ Regional).
</details>

---

### Q29: Phân biệt 4 cấp độ Disaster Recovery xếp theo thứ tự RTO/RPO từ lớn nhất đến nhỏ nhất?
<details>
<summary>👉 Xem đáp án</summary>
1. <b>Backup & Restore:</b> RTO/RPO nhiều giờ (Chi phí rẻ nhất $).
2. <b>Pilot Light:</b> RTO hàng chục phút, RPO vài phút (Chi phí $$ - DB nhỏ chạy liên tục, EC2 tắt).
3. <b>Warm Standby:</b> RTO vài phút, RPO vài giây/phút (Chi phí $$$ - Bản thu nhỏ chạy 24/7).
4. <b>Multi-Site Active-Active:</b> RTO/RPO ~ 0 (Chi phí đắt nhất $$$$ - 100% công suất song song).
</details>

---

### Q30: Dịch vụ nào cung cấp khả năng sao chép khối đĩa liên tục (Continuous Block-Level Replication) để khôi phục thảm họa từ máy chủ On-Premise lên AWS?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Elastic Disaster Recovery (AWS DRS)</b>.
</details>
