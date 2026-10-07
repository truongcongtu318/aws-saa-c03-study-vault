---
title: "Domain 1 Quick Recall Flashcards: 30 Active-Recall Questions for Exam Readiness"
domain: "Domain 1: Design Secure Architectures"
task_statement: "All Tasks (1.1, 1.2, 1.3)"
exam_weight: "Very High"
tags:
  - flashcards
  - active-recall
  - revision
  - quick-review
  - domain1
last_updated: 2026-03-30
---

# 🧠 Domain 1 Quick Recall: 30 Câu Hỏi Phản Xạ Nhanh

> Trở về: [[00-Domain-1-MOC]]  
> **Hướng dẫn ôn tập:** Đọc câu hỏi, tự nhẩm câu trả lời trong đầu trước khi bấm vào thẻ `<details>` để xem đáp án giải thích.

---

### Q1: Thứ tự ưu tiên khi AWS đánh giá một IAM Policy là gì?
<details>
<summary>👉 Xem đáp án</summary>
<b>Explicit Deny > Explicit Allow > Implicit Deny</b>. Bất kỳ một điều khoản Deny nào ở bất kỳ cấp độ nào (Identity, Resource, SCP, Boundary) sẽ ngay lập tức vô hiệu hóa mọi quyền Allow.
</details>

---

### Q2: Ứng dụng chạy trên EC2 cần gọi S3 API thì cấp quyền như thế nào theo Best Practice?
<details>
<summary>👉 Xem đáp án</summary>
Tạo một <b>IAM Role</b> với policy cấp quyền S3, sau đó gán vào <b>EC2 Instance Profile</b> của máy chủ. Tuyệt đối không lưu trữ Access Key/Secret Key trên file hệ thống của EC2.
</details>

---

### Q3: Service Control Policies (SCPs) có trực tiếp cấp quyền (grant permissions) cho IAM User trong tài khoản con không?
<details>
<summary>👉 Xem đáp án</summary>
<b>KHÔNG</b>. SCP chỉ đóng vai trò là rào chắn trần (Guardrail/Filter). User vẫn bắt buộc phải có IAM Policy Allow nội bộ để thực hiện hành động.
</details>

---

### Q4: SCP có áp dụng lên Root User của Member Account không? Có áp dụng lên Management Account không?
<details>
<summary>👉 Xem đáp án</summary>
- Có áp dụng lên <b>Root User của Member Account</b>.
- <b>KHÔNG</b> áp dụng lên <b>Management Account</b> của Organization.
</details>

---

### Q5: Khi tích hợp với bên thứ ba (SaaS vendor) qua AssumeRole chéo tài khoản, tham số nào ngăn chặn lỗ hổng Confused Deputy?
<details>
<summary>👉 Xem đáp án</summary>
Tham số <b><code>sts:ExternalId</code></b> được cấu hình trong điều kiện <code>Condition</code> của IAM Trust Policy.
</details>

---

### Q6: Doanh nghiệp muốn người dùng dùng tài khoản Active Directory On-premises để đăng nhập AWS mà KHÔNG lưu trữ hay đồng bộ user lên AWS, dùng dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Directory Service AD Connector</b> (hoạt động như một proxy chuyển tiếp xác thực).
</details>

---

### Q7: Để thiết lập Two-way Forest Trust giữa Active Directory On-premises và AWS, cần dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Managed Microsoft AD</b>.
</details>

---

### Q8: Phân biệt vai trò của Cognito User Pools và Cognito Identity Pools?
<details>
<summary>👉 Xem đáp án</summary>
- <b>User Pools (Authentication):</b> Cơ sở dữ liệu user, xử lý đăng ký/đăng nhập, MFA, trả về JWT Token.
- <b>Identity Pools (Authorization):</b> Đổi JWT Token lấy <b>Temporary AWS Credentials</b> để truy cập trực tiếp tài nguyên AWS (S3, DynamoDB).
</details>

---

### Q9: Dịch vụ nào cho phép chia sẻ VPC Subnets hoặc Transit Gateway giữa các tài khoản AWS trong cùng một Organization mà không phải tạo mới?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Resource Access Manager (AWS RAM)</b>.
</details>

---

### Q10: Security Group có trạng thái gì? Network ACL có trạng thái gì?
<details>
<summary>👉 Xem đáp án</summary>
- Security Group là <b>Stateful</b> (gói tin vào được thì tự động ra được).
- Network ACL là <b>Stateless</b> (chiều vào và chiều ra phải mở độc lập).
</details>

---

### Q11: Muốn chặn ngay một địa chỉ IP độc hại tấn công máy chủ, dùng Security Group hay Network ACL?
<details>
<summary>👉 Xem đáp án</summary>
Dùng <b>Network ACL</b> tạo một inbound rule <b>DENY</b> với số thứ tự nhỏ hơn rule Allow. (Security Group không có luật Deny).
</details>

---

### Q12: Khi cấu hình Network ACL cho phép tải dữ liệu web (port 80/443), tại sao chiều Outbound bắt buộc phải mở cổng 1024-65535?
<details>
<summary>👉 Xem đáp án</summary>
Vì NACL là Stateless, các gói tin HTTP phản hồi từ máy chủ gửi về client sử dụng dải cổng tạm thời <b>Ephemeral Ports (1024-65535)</b> của client.
</details>

---

### Q13: VPC Gateway Endpoint hỗ trợ những dịch vụ AWS nào? Chi phí ra sao?
<details>
<summary>👉 Xem đáp án</summary>
Chỉ hỗ trợ <b>Amazon S3</b> và <b>Amazon DynamoDB</b>. Chi phí là <b>hoàn toàn MIỄN PHÍ ($0)</b>.
</details>

---

### Q14: Muốn kết nối riêng tư từ VPC tới AWS KMS hoặc Secrets Manager mà không qua Internet, dùng loại Endpoint nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>VPC Interface Endpoint (AWS PrivateLink)</b>. Tạo một ENI có địa chỉ IP riêng trong Subnet và có tính phí theo giờ + dung lượng GB.
</details>

---

### Q15: Làm sao để quản trị viên truy cập terminal máy chủ EC2 trong Private Subnet mà không mở cổng Inbound 22/3389 và không cần Bastion Host?
<details>
<summary>👉 Xem đáp án</summary>
Sử dụng <b>AWS Systems Manager Session Manager</b> (giao tiếp qua kết nối outbound an toàn của SSM Agent).
</details>

---

### Q16: AWS WAF bảo vệ ứng dụng ở tầng nào của mô hình OSI? Gắn được vào những dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
Hoạt động ở <b>Layer 7 (Application Layer)</b>. Gắn được vào: <b>Amazon CloudFront, Application Load Balancer (ALB), Amazon API Gateway, AWS AppSync, Amazon Cognito</b>.
</details>

---

### Q17: Điểm khác biệt quan trọng nhất giữa AWS Shield Standard và AWS Shield Advanced là gì?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Shield Standard:</b> Miễn phí, tự động bật sẵn cho L3/L4 DDoS.
- <b>Shield Advanced:</b> $3,000/tháng, hỗ trợ đội phản ứng nhanh <b>DDoS Response Team (DRT) 24/7</b> và có chính sách bảo vệ tài chính <b>DDoS Cost Protection</b>.
</details>

---

### Q18: Dịch vụ nào cho phép lọc lưu lượng Outbound từ VPC ra ngoài Internet dựa trên tên miền cụ thể (FQDN domain e.g. *.example.com)?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Network Firewall</b> với tính năng Stateful Domain List Rule Group.
</details>

---

### Q19: Dịch vụ nào giúp tự động triển khai WAF Rules và kiểm soát Security Group đồng nhất trên hàng trăm tài khoản trong AWS Organizations?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Firewall Manager</b> (Yêu cầu phải bật AWS Organizations và AWS Config).
</details>

---

### Q20: Phân biệt Amazon GuardDuty và Amazon Inspector?
<details>
<summary>👉 Xem đáp án</summary>
- <b>GuardDuty:</b> Phát hiện mối đe dọa và xâm nhập (Threat Detection) dựa trên ML đọc VPC Flow Logs, DNS Logs, CloudTrail ngầm (Không cần Agent).
- <b>Inspector:</b> Quét lỗ hổng phần mềm đã biết (CVE) trên EC2 (cần SSM Agent), ECR Container Images, và Lambda.
</details>

---

### Q21: Dịch vụ nào chuyên quét phát hiện số thẻ tín dụng hoặc thông tin PII bị lộ trong Amazon S3?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon Macie</b>.
</details>

---

### Q22: Dịch vụ nào đóng vai trò Single Pane of Glass gom cảnh báo bảo mật từ GuardDuty, Inspector, Macie và chấm điểm tuân thủ CIS AWS Benchmark?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Security Hub</b>.
</details>

---

### Q23: Dịch vụ nào giúp điều tra nguyên nhân gốc rễ (Root Cause Analysis) bằng biểu đồ trực quan (Graph visualization) sau khi có cảnh báo an ninh?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon Detective</b>.
</details>

---

### Q24: Tính năng nào của CloudTrail chứng minh file log lưu trên S3 chưa từng bị chỉnh sửa hoặc xóa?
<details>
<summary>👉 Xem đáp án</summary>
<b>CloudTrail Log File Integrity Validation</b> (Sử dụng hàm băm SHA-256 và chữ ký số RSA).
</details>

---

### Q25: Khách hàng cần lưu thông tin đăng nhập database và yêu cầu tự động đổi mật khẩu mỗi 30 ngày, chọn Secrets Manager hay SSM Parameter Store?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Secrets Manager</b> vì có tích hợp sẵn <b>Automatic Rotation</b> với Lambda cho RDS/Aurora.
</details>

---

### Q26: Khách hàng cần lưu trữ hàng trăm biến môi trường cấu hình ứng dụng với chi phí rẻ nhất (ưu tiên $0), chọn dịch vụ nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Systems Manager Parameter Store (Standard Tier)</b> (Hoàn toàn miễn phí, hỗ trợ SecureString mã hóa KMS).
</details>

---

### Q27: Cơ chế Envelope Encryption trong KMS hoạt động như thế nào khi mã hóa file dung lượng lớn?
<details>
<summary>👉 Xem đáp án</summary>
KMS dùng CMK tạo ra <b>Data Key (Plaintext + Ciphertext)</b>. Ứng dụng dùng Plaintext Data Key để mã hóa dữ liệu cục bộ, sau đó <b>xóa Plaintext Data Key khỏi RAM</b> và lưu Ciphertext Data Key cùng với file đã mã hóa.
</details>

---

### Q28: Dịch vụ mã hóa nào cung cấp thiết bị phần cứng chuyên dụng Single-tenant đạt chuẩn FIPS 140-2 Level 3 mà khách hàng toàn quyền kiểm soát?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS CloudHSM</b>.
</details>

---

### Q29: Phân biệt Governance Mode và Compliance Mode trong S3 Object Lock?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Governance Mode:</b> Người dùng có quyền đặc biệt <code>s3:BypassGovernanceRetention</code> (Admin/Root) vẫn có thể xóa/sửa file trước hạn.
- <b>Compliance Mode:</b> <b>TUYỆT ĐỐI KHÔNG AI</b> có thể sửa hoặc xóa file trước thời hạn retention, kể cả Root User của tài khoản AWS.
</details>

---

### Q30: Làm thế nào để bắt buộc mọi yêu cầu đọc/ghi dữ liệu lên S3 Bucket phải sử dụng giao thức HTTPS?
<details>
<summary>👉 Xem đáp án</summary>
Thêm một điều khoản trong S3 Bucket Policy với <b><code>"Effect": "Deny"</code></b> và điều kiện <b><code>"Bool": {"aws:SecureTransport": "false"}</code></b>.
</details>

---

### Q31: Dịch vụ nào cung cấp giải pháp thiết lập môi trường đa tài khoản chuẩn mực (Landing Zone) với ít công sức vận hành nhất?
<details>
<summary>👉 Xem đáp án</summary>
<b>AWS Control Tower</b> (tự động điều phối ngầm AWS Organizations, AWS Config, IAM Identity Center và Service Catalog).
</details>

---

### Q32: Phân biệt sự khác nhau giữa Preventive Guardrails và Detective Guardrails trong AWS Control Tower?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Preventive Guardrails:</b> Sử dụng <b>Service Control Policies (SCPs)</b> để <b>CHẶN ĐỨNG NGAY TỨC THÌ</b> các hành động vi phạm tại thời điểm gọi API.
- <b>Detective Guardrails:</b> Sử dụng <b>AWS Config Rules</b> để <b>CHO PHÉP TẠO</b> nhưng sau đó gắn cờ và <b>BẮN CẢNH BÁO vi phạm (Non-compliant)</b> trên Dashboard.
</details>

---

### Q33: Vai trò của Log Archive Account và Audit Account trong kiến trúc Security OU của Control Tower là gì?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Log Archive Account:</b> Chứa S3 Bucket bất khả xâm phạm gom toàn bộ nhật ký CloudTrail và AWS Config từ tất cả các tài khoản con đổ về.
- <b>Audit Account (Security Tooling):</b> Cung cấp quyền chỉ đọc và phân tích tập trung cho đội SecOps quản lý Security Hub và GuardDuty.
</details>

---

### Q34: Tính năng Account Factory trong AWS Control Tower được xây dựng dựa trên dịch vụ nào và mang lại lợi ích gì?
<details>
<summary>👉 Xem đáp án</summary>
Xây dựng dựa trên <b>AWS Service Catalog</b>, cung cấp cơ chế tự phục vụ (Self-service) cho phép các nhóm dự án tự tạo tài khoản mới đã cấu hình sẵn <b>VPC baseline</b>, kết nối <b>SSO</b> và tự động thừa hưởng toàn bộ <b>Guardrails</b> của OU.
</details>
