---
title: "Domain 4 Quick Recall Flashcards: 30 Active-Recall Questions for Exam Readiness"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "All Tasks (4.1, 4.2, 4.3, 4.4)"
exam_weight: "Very High"
tags:
  - flashcards
  - active-recall
  - revision
  - quick-review
  - domain4
  - finops
last_updated: 2026-03-30
---

# 🧠 Domain 4 Quick Recall: 30 Câu Hỏi Phản Xạ Nhanh

> Trở về: [[00-Domain-4-MOC]]  
> **Hướng dẫn ôn tập:** Đọc câu hỏi, tự nhẩm câu trả lời trong đầu trước khi bấm vào thẻ `<details>` để xem đáp án giải thích.

---

### Q1: Lớp lưu trữ S3 nào được khuyến nghị khi tần suất truy cập dữ liệu không đoán trước được hoặc thay đổi liên tục?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon S3 Intelligent-Tiering</b> (tự động chuyển đổi giữa các tầng và <b>HOÀN TOÀN KHÔNG CÓ PHÍ TRUY XUẤT DỮ LIỆU</b>).
</details>

---

### Q2: S3 Standard-IA có quy định thời gian lưu trữ tối thiểu và kích thước tệp tối thiểu tính tiền là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
- Thời gian lưu trữ tối thiểu: <b>30 ngày</b> (xóa sớm vẫn bị tính đủ 30 ngày).
- Kích thước tệp tối thiểu tính tiền: <b>128 KB</b> (file 5 KB vẫn bị tính tiền lưu trữ như 128 KB).
</details>

---

### Q3: Lớp lưu trữ nào rẻ nhất toàn bộ AWS (~$1/TB/tháng) và thời gian lấy dữ liệu là bao lâu?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon S3 Glacier Deep Archive</b> (~$0.00099/GB/tháng). Thời gian lấy dữ liệu từ <b>12 đến 48 giờ</b>, thời gian lưu trữ tối thiểu tính tiền là <b>180 ngày</b>.
</details>

---

### Q4: S3 Glacier Instant Retrieval khác biệt gì so với Glacier Flexible Retrieval?
<details>
<summary>👉 Xem đáp án</summary>
<b>Glacier Instant Retrieval</b> cho phép lấy lại dữ liệu trong <b>vài mili-giây (Millisecond access)</b>, trong khi Glacier Flexible Retrieval mất từ 1-5 phút (Expedited) hoặc 3-5 giờ (Standard).
</details>

---

### Q5: Tính năng nào của S3 Lifecycle giúp dọn dẹp các mảnh tệp rác tải lên dở dang bị đứt mạng để tránh tốn tiền lưu trữ?
<details>
<summary>👉 Xem đáp án</summary>
Hành động <b>Abort Incomplete Multipart Uploads</b> (ví dụ cấu hình hủy sau 7 ngày).
</details>

---

### Q6: Nâng cấp ổ cứng EBS từ `gp2` sang `gp3` đem lại lợi ích tài chính và kỹ thuật gì?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Tiết kiệm ngay 20% chi phí dung lượng lưu trữ</b>.
- Cung cấp sẵn <b>3,000 IOPS và 125 MB/s hoàn toàn miễn phí</b> cho mọi kích cỡ ổ đĩa.
- Có thể đổi trực tiếp online không gián đoạn bằng <b>EBS Elastic Volumes</b>.
</details>

---

### Q7: Tại sao cần định kỳ kiểm tra và xóa các ổ đĩa EBS ở trạng thái `available`?
<details>
<summary>👉 Xem đáp án</summary>
Trạng thái <code>available</code> nghĩa là ổ đĩa đang <b>mồ côi (Unattached - không gắn vào máy chủ EC2 nào)</b> nhưng AWS vẫn tiếp tục tính tiền dung lượng lưu trữ đầy đủ 100% hàng tháng.
</details>

---

### Q8: Tính năng nào của Amazon EFS giúp giảm tới 92% chi phí lưu trữ cho các tệp ít khi được truy cập?
<details>
<summary>👉 Xem đáp án</summary>
<b>EFS Lifecycle Management</b> tự động chuyển tệp sang <b>EFS Infrequent Access (EFS IA)</b> ($0.025/GB so với $0.30/GB của EFS Standard).
</details>

---

### Q9: Amazon EC2 Spot Instances đem lại mức giảm giá tối đa bao nhiêu và AWS cảnh báo trước bao lâu khi thu hồi máy?
<details>
<summary>👉 Xem đáp án</summary>
Giảm giá lên tới <b>90%</b> so với On-Demand. AWS gửi cảnh báo thu hồi trước <b>2 phút (2-minute interruption notice)</b>.
</details>

---

### Q10: Compute Savings Plans khác biệt gì so với EC2 Instance Savings Plans?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Compute Savings Plans (Linh hoạt nhất):</b> Giảm tới 66%, tự động áp dụng xuyên suốt mọi họ EC2, Fargate và Lambda, không phân biệt Region hay OS.
- <b>EC2 Instance Savings Plans:</b> Giảm tới 72%, nhưng bị khóa chặt vào 1 họ máy chủ EC2 cụ thể trong 1 Region cụ thể.
</details>

---

### Q11: Chiến lược phân bổ nào trong Spot Fleet giúp giảm thiểu tối đa rủi ro máy chủ bị AWS thu hồi giữa chừng?
<details>
<summary>👉 Xem đáp án</summary>
Chiến lược phân bổ <b><code>capacity-optimized</code></b> (tự động chọn nhóm Spot pool có dung lượng phần cứng dư thừa nhiều nhất).
</details>

---

### Q12: Dịch vụ nào sử dụng Machine Learning phân tích số liệu CloudWatch trong 14-32 ngày để khuyến nghị hạ cỡ (Right-Sizing) các tài nguyên bị dư thừa?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Compute Optimizer</b> (hỗ trợ EC2, Auto Scaling Groups, EBS, Lambda, RDS).
</details>

---

### Q13: Điểm hòa vốn (Tipping Point) để chuyển đổi từ Amazon Aurora Standard sang Aurora I/O-Optimized là gì?
<details>
<summary>👉 Xem đáp án</summary>
Khi <b>chi phí I/O đọc/ghi chiếm từ 25% đến 33% trở lên</b> trong tổng hóa đơn database hàng tháng. Chuyển sang I/O-Optimized sẽ được <b>MIỄN PHÍ HOÀN TOÀN TIỀN I/O ($0)</b> và tiết kiệm 30-40% tổng chi phí.
</details>

---

### Q14: Khi nào nên chọn DynamoDB On-Demand Mode và khi nào nên chọn Provisioned Mode?
<details>
<summary>👉 Xem đáp án</summary>
- <b>On-Demand:</b> Khi lưu lượng thấp, khó dự đoán, đột biến bất thường, hoặc có thời gian rảnh kéo dài (không mất tiền khi rảnh).
- <b>Provisioned (kèm Auto Scaling):</b> Khi lưu lượng lớn, đều đặn, ổn định (Steady-State), rẻ hơn từ 5 đến 7 lần.
</details>

---

### Q15: Làm thế nào để giảm 50% chi phí đọc (RCU) trên Amazon DynamoDB mà không cần thay đổi cấu trúc bảng?
<details>
<summary>👉 Xem đáp án</summary>
Chuyển đổi các câu truy vấn từ <b>Strongly Consistent Reads sang Eventually Consistent Reads</b> (1 RCU đọc được 2 items 4KB thay vì chỉ 1 item).
</details>

---

### Q16: Làm thế nào để tiết kiệm 65% chi phí compute cho các cơ sở dữ liệu Amazon RDS môi trường Dev và Test?
<details>
<summary>👉 Xem đáp án</summary>
Cấu hình tự động <b>Stop các RDS instances ngoài giờ làm việc (ban đêm và cuối tuần)</b> bằng AWS Instance Scheduler hoặc AWS Lambda.
</details>

---

### Q17: Chi phí truyền tải dữ liệu từ ngoài Internet nạp vào AWS (Data Transfer In) là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
<b>HOÀN TOÀN MIỄN PHÍ ($0.00 / GB)</b> trên toàn bộ các dịch vụ của AWS.
</details>

---

### Q18: Truyền tải dữ liệu giữa hai máy chủ EC2 trong cùng 1 Availability Zone có mất tiền không? Có bẫy gì cần lưu ý?
<details>
<summary>👉 Xem đáp án</summary>
- <b>MIỄN PHÍ ($0)</b> nếu giao tiếp qua <b>Private IP</b>.
- <b>TỐN TIỀN ($0.01/GB mỗi chiều)</b> nếu giao tiếp qua <b>Public IP hoặc Elastic IP</b> (vì gói tin bị định tuyến ra Internet Gateway).
</details>

---

### Q19: Chi phí truyền tải dữ liệu xuyên Availability Zone (Cross-AZ) là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
<b>$0.01 / GB mỗi chiều</b> ($0.02 / GB cho cả 2 chiều gửi và nhận khứ hồi).
</details>

---

### Q20: NAT Gateway tính cước như thế nào? Bẫy chi phí khổng lồ của nó là gì?
<details>
<summary>👉 Xem đáp án</summary>
- $0.045 / giờ duy trì NAT Gateway.
- <b>$0.045 / GB dữ liệu xử lý</b>. Khi máy chủ private truyền tải hàng chục TB dữ liệu lên S3/DynamoDB qua NAT Gateway, phí xử lý dữ liệu sẽ tốn hàng ngàn USD!
</details>

---

### Q21: Làm thế nào để loại bỏ 100% chi phí xử lý dữ liệu của NAT Gateway khi máy chủ private subnet truy cập Amazon S3 hoặc DynamoDB?
<details>
<summary>👉 Xem đáp án</summary>
Tạo <b>VPC Gateway Endpoint cho Amazon S3 và DynamoDB</b> (cập nhật Route Table). Dịch vụ này <b>HOÀN TOÀN MIỄN PHÍ ($0/giờ và $0/GB)</b>.
</details>

---

### Q22: Chi phí truyền dữ liệu từ Amazon S3 sang Amazon CloudFront là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
<b>HOÀN TOÀN MIỄN PHÍ ($0.00 / GB)</b>.
</details>

---

### Q23: Muốn giảm chi phí phân phối CloudFront khi công ty chỉ có người dùng tại Mỹ và Châu Âu, chọn Price Class nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>`PriceClass_100`</b> (chỉ sử dụng các trạm PoPs tại Bắc Mỹ và Châu Âu - rẻ nhất).
</details>

---

### Q24: Tính năng CloudFront Origin Shield giúp tiết kiệm chi phí như thế nào?
<details>
<summary>👉 Xem đáp án</summary>
Tạo ra một tầng cache tập trung duy nhất gần Origin, gom các request trùng lặp từ nhiều Edge Caches, giúp <b>hạ kích thước máy chủ Origin (ALB/EC2) xuống mức nhỏ nhất</b> và giảm cước dữ liệu.
</details>

---

### Q25: AWS Budgets Actions có thể thực hiện những hành động tự động nào khi tài khoản vượt ngân sách?
<details>
<summary>👉 Xem đáp án</summary>
1. Tự động áp dụng một <b>Service Control Policy (SCP)</b> hạn chế quyền tạo tài nguyên.
2. Tự động áp dụng <b>IAM Policy</b> thu hồi quyền của User/Role.
3. Tự động <b>dừng (Stop) các máy chủ EC2 hoặc RDS</b> mục tiêu.
</details>

---

### Q26: Dịch vụ nào sử dụng Machine Learning để tự động phát hiện chi phí tăng đột biến bất thường do rò rỉ tài nguyên mà không cần đặt ngưỡng trước?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Cost Anomaly Detection</b>.
</details>

---

### Q27: Điều kiện bắt buộc để các thẻ Cost Allocation Tags xuất hiện trong báo cáo chi phí của AWS Cost Explorer là gì?
<details>
<summary>👉 Xem đáp án</summary>
Bắt buộc phải vào <b>AWS Billing Console để KÍCH HOẠT (Activate)</b> các thẻ tag đó.
</details>

---

### Q28: Tính năng Consolidated Billing trong AWS Organizations giúp tiết kiệm chi phí lưu trữ S3 như thế nào?
<details>
<summary>👉 Xem đáp án</summary>
Cộng dồn tổng dung lượng lưu trữ S3 của tất cả các tài khoản thành viên lại với nhau để chạm tới các mốc <b>chiết khấu theo bậc dung lượng lớn (Volume Tier Discounts)</b> của AWS.
</details>

---

### Q29: Muốn xuất toàn bộ dữ liệu chi phí chi tiết đến từng dòng tài nguyên theo từng giờ ra Amazon S3 để phân tích bằng Amazon Athena, dùng công cụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Cost and Usage Report (CUR)</b>.
</details>

---

### Q30: Muốn chia sẻ S3 bucket cho cộng đồng tải dữ liệu công khai nhưng bắt người tải phải trả tiền cước Data Transfer Out, bật tính năng gì?
<details>
<summary>👉 Xem đáp án</summary>
Bật tính năng <b>Requester Pays</b> trên S3 Bucket.
</details>
