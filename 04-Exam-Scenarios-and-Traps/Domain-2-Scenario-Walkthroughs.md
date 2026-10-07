---
title: "Domain 2 Scenario Walkthroughs: 10 Classic SAA-C03 Exam Scenarios with Distractor Analysis"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "All Tasks (2.1, 2.2)"
exam_weight: "Very High"
tags:
  - scenarios
  - practice-questions
  - distractor-analysis
  - exam-traps
  - domain2
last_updated: 2026-03-30
---

# 🎯 Domain 2 Scenario Walkthroughs: 10 Kịch Bản Đề Thi Thực Tế & Bẫy Distractor

> Trở về: [[00-Domain-2-MOC]]

---

## 📌 Kịch bản 1: SQS Visibility Timeout & Trùng lặp tác vụ (Task 2.1)

### Đề bài (Exam Prompt):
> A photo-sharing platform uses an Amazon SQS standard queue to buffer image processing tasks. A fleet of Amazon EC2 worker instances polls messages from the queue and converts raw images into multiple thumbnail resolutions. Each conversion job takes approximately 4 minutes to complete. However, developers notice that multiple worker instances are frequently processing the exact same image simultaneously. What is the MOST effective solution to resolve this issue?

- **A.** Convert the SQS queue to a FIFO queue to enforce strict ordering.
- **B.** Increase the Visibility Timeout of the SQS queue to 5 minutes, or call `ChangeMessageVisibility` from the worker code during execution.
- **C.** Enable SQS Long Polling by setting `ReceiveMessageWaitTimeSeconds` to 20 seconds.
- **D.** Configure a Dead Letter Queue (DLQ) with `maxReceiveCount` set to 1.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Khi một worker nhận tin nhắn từ SQS, tin nhắn sẽ bị ẩn đi trong một khoảng thời gian gọi là **Visibility Timeout** (mặc định là 30 giây). Nếu tác vụ mất 4 phút mà timeout chỉ là 30 giây, sau 30 giây tin nhắn sẽ tự động hiện lại trong hàng đợi và một worker khác sẽ nhặt được nó $\rightarrow$ dẫn đến **xử lý trùng lặp**. Tăng Visibility Timeout lên 5 phút (lớn hơn thời gian xử lý 4 phút) hoặc gọi API **`ChangeMessageVisibility`** để gia hạn thời gian ẩn tin nhắn là giải pháp chính xác.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** FIFO queue đảm bảo thứ tự và chống trùng lặp gửi tin, nhưng nếu Visibility Timeout bị hết hạn thì FIFO queue vẫn cho phép worker khác nhận lại tin nhắn đó!
  - **C SAI:** Long Polling giúp giảm chi phí gọi API rỗng khi queue trống, không giải quyết vấn đề tin nhắn bị lộ lại khi đang xử lý.
  - **D SAI:** DLQ với `maxReceiveCount = 1` sẽ đẩy ngay tin nhắn sang DLQ sau lần đầu tiên, khiến ảnh chưa kịp chuyển đổi xong đã bị vứt bỏ.

---

## 📌 Kịch bản 2: Chạy nhiều domain HTTPS trên 1 Load Balancer (Task 2.2)

### Đề bài (Exam Prompt):
> An organization hosts multiple secure web applications on an Amazon ECS cluster behind a load balancer. The applications serve traffic for `api.example.com`, `auth.example.com`, and `shop.example.com`. Each application requires its own dedicated SSL/TLS certificate issued by AWS Certificate Manager (ACM). The company wants to minimize operational cost and architectural complexity. Which load balancing strategy should be chosen?

- **A.** Deploy an Application Load Balancer (ALB) and add all three ACM certificates to the HTTPS listener using Server Name Indication (SNI).
- **B.** Deploy a separate Network Load Balancer (NLB) for each domain name, each with its own certificate.
- **C.** Deploy an ALB for each domain name, and configure DNS routing policies in Route 53.
- **D.** Deploy a Classic Load Balancer with a wildcard certificate covering all subdomains.

### ✅ Đáp án ĐÚNG: **A**
- **Giải thích kiến trúc:** **Application Load Balancer (ALB)** hỗ trợ tính năng **Server Name Indication (SNI)**. SNI cho phép bạn đính kèm nhiều chứng chỉ SSL/TLS khác nhau lên cùng 1 HTTPS Listener duy nhất. Khi client gửi request, ALB sẽ kiểm tra hostname trong gói tin TLS handshake và tự động phục vụ đúng chứng chỉ SSL tương ứng với chi phí tối ưu nhất (chỉ tốn tiền 1 ALB).
- **Phân tích bẫy (Distractor Analysis):**
  - **B & C SAI:** Tạo 3 Load Balancers riêng biệt làm tăng gấp 3 lần chi phí không cần thiết (vi phạm yêu cầu *minimize operational cost*).
  - **D SAI:** Classic Load Balancer là công nghệ thế hệ cũ (deprecated) và không tối ưu cho kiến trúc ECS hiện đại.

---

## 📌 Kịch bản 3: Yêu cầu IP tĩnh cố định cho đối tác Whitelist (Task 2.2)

### Đề bài (Exam Prompt):
> A financial web application hosted on AWS needs to receive secure transaction callbacks from external third-party banking partners. The banking partners strictly require two static public IP addresses to whitelist in their corporate on-premises firewalls. The application runs on EC2 instances distributed across two Availability Zones. Which architecture satisfies this requirement?

- **A.** Deploy an Application Load Balancer (ALB) in public subnets across the two AZs, and assign an Elastic IP to each ALB node.
- **B.** Deploy a Network Load Balancer (NLB) across the two AZs, allocate an Elastic IP to the NLB in each AZ, and forward traffic to the EC2 target group.
- **C.** Create an Auto Scaling Group with two EC2 instances, assign an Elastic IP to each instance, and use Route 53 Weighted routing.
- **D.** Deploy an ALB behind an Amazon CloudFront distribution, and provide CloudFront's anycast IP addresses to the banking partners.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Network Load Balancer (NLB)** hỗ trợ gán **Elastic IP cố định cho từng Availability Zone** ngay từ khi khởi tạo. Đối tác ngân hàng có thể yên tâm whitelist 2 địa chỉ IP tĩnh này trên tường lửa mà không sợ IP bị thay đổi.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** **ALB KHÔNG THỂ gán Elastic IP trực tiếp** (ALB chỉ cung cấp DNS name có IP động thay đổi liên tục). *(Lưu ý: Muốn ALB có IP tĩnh phải bọc AWS Global Accelerator phía trước).*
  - **C SAI:** Gán Elastic IP trực tiếp vào EC2 làm mất khả năng tự động co giãn và cân bằng tải linh hoạt của hệ thống.
  - **D SAI:** CloudFront sử dụng hàng trăm nghìn IP động toàn cầu, không thể cung cấp 2 IP tĩnh cố định để whitelist.

---

## 📌 Kịch bản 4: Sao lưu log trước khi Auto Scaling hủy máy chủ (Task 2.2)

### Đề bài (Exam Prompt):
> An application running on EC2 instances within an Auto Scaling Group (ASG) writes batch log files to the local instance store. The compliance team mandates that when the ASG scales in during low traffic periods, all local log files on terminating instances must be successfully transferred to an Amazon S3 bucket before the instances are completely terminated. Which solution achieves this?

- **A.** Write a cron job on the EC2 instances that uploads logs to Amazon S3 every 60 seconds.
- **B.** Configure an Auto Scaling lifecycle hook for the `EC2_INSTANCE_TERMINATING` transition, execute a script via Systems Manager or User Data to push logs to S3, and complete the lifecycle action.
- **C.** Modify the ASG Termination Policy to `OldestInstance` so that instances with logs are kept indefinitely.
- **D.** Change the EC2 root volume termination behavior to `DeleteOnTermination: false`.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Auto Scaling Lifecycle Hooks** (`autoscaling:EC2_INSTANCE_TERMINATING`) cho phép đưa máy chủ vào trạng thái chờ (**`Terminating:Wait`**). Trong khoảng thời gian này, máy chủ ngừng nhận traffic mới nhưng chưa bị tắt, tạo điều kiện cho các script chạy ngầm thu gom và đẩy toàn bộ log lên Amazon S3. Sau khi upload thành công, script gọi `CompleteLifecycleAction` để ASG tiến hành hủy máy chủ an toàn.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Cron job 60 giây vẫn có nguy cơ bị mất dữ liệu log của 59 giây cuối cùng nếu máy chủ bị ASG tắt đột ngột giữa chu kỳ.
  - **C SAI:** Đổi Termination Policy không ngăn được việc máy chủ cuối cùng cũng sẽ bị xóa khi quy mô giảm về mức tối thiểu.
  - **D SAI:** Đề bài lưu trữ trên *local instance store* (bộ nhớ tạm), tính năng `DeleteOnTermination: false` chỉ áp dụng cho ổ EBS và dữ liệu instance store vẫn sẽ mất trắng khi tắt máy.

---

## 📌 Kịch bản 5: Giảm tải đọc cho cơ sở dữ liệu bị quá tải (Task 2.2)

### Đề bài (Exam Prompt):
> A retail company uses a single Amazon RDS for MySQL DB instance. During promotional sales events, read traffic surges dramatically, causing CPU utilization on the database to reach 95% and leading to timeouts on the web tier. The application workload consists of 85% read operations (product catalog browsing) and 15% write operations (checkout). What is the MOST cost-effective architectural change to resolve the bottleneck?

- **A.** Convert the RDS DB instance into a Multi-AZ deployment.
- **B.** Create RDS Read Replicas, configure the application to direct catalog browsing queries to the Read Replicas, and keep checkout writes on the primary DB instance.
- **C.** Migrate the database to Amazon Redshift to accelerate SQL query throughput.
- **D.** Increase the compute instance size of the primary RDS database to an `m5.24xlarge` instance.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Với khối lượng công việc chiếm tới 85% là thao tác đọc (Read-heavy), giải pháp tối ưu và chuẩn mực là tạo **Amazon RDS Read Replicas**. Ứng dụng sẽ phân tách lưu lượng: Các truy vấn `SELECT` đọc dữ liệu danh mục được trỏ sang các Read Replicas, giảm tải hoàn toàn cho Primary DB để nó tập trung xử lý các giao dịch ghi `INSERT/UPDATE` đặt hàng.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI (BẪY KINH ĐIỂN):** Kích hoạt **RDS Multi-AZ KHÔNG GIÚP GIẢM TẢI ĐỌC**, vì Standby instance ở AZ phụ chỉ dùng để dự phòng khi sập nguồn và **hoàn toàn không cho phép kết nối để đọc dữ liệu**!
  - **C SAI:** Redshift là kho dữ liệu OLAP phục vụ phân tích dữ liệu lớn, không dùng làm cơ sở dữ liệu giao dịch trực tuyến OLTP cho website bán hàng.
  - **D SAI:** Mở rộng dọc (Vertical scaling) chỉ là giải pháp tạm thời, chi phí cực kỳ đắt đỏ và không giải quyết được tính phân tán mở rộng ngang.

---

## 📌 Kịch bản 6: Khôi phục thảm họa đa vùng với RTO < 1 phút, RPO < 1s (Task 2.2)

### Đề bài (Exam Prompt):
> A global financial trading application requires a multi-region disaster recovery solution for its relational database. The compliance department strictly specifies a Recovery Point Objective (RPO) of less than 1 second and a Recovery Time Objective (RTO) of less than 1 minute. Which database architecture satisfies these stringent requirements?

- **A.** Amazon RDS for PostgreSQL with cross-region automated snapshots taken every 1 hour.
- **B.** Amazon Aurora Global Database spanning two AWS Regions with dedicated replication storage.
- **C.** Amazon RDS Multi-AZ deployment deployed across two separate AWS Regions.
- **D.** Amazon DynamoDB with single-region tables and daily AWS Backup jobs.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon Aurora Global Database** sử dụng hạ tầng mạng chuyên dụng của AWS để sao chép dữ liệu ở tầng lưu trữ phân tán xuyên lục địa với độ trễ sao chép thông thường **dưới 1 giây (RPO < 1s)**. Khi Region chính gặp thảm họa, Region phụ có thể được kích hoạt thành Primary (Promote) với thời gian **dưới 1 phút (RTO < 1m)** mà không bị mất dữ liệu.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Snapshot mỗi 1 giờ sẽ có RPO lên tới 1 giờ và thời gian restore snapshot mất hàng chục phút (RTO >> 1m).
  - **C SAI:** RDS Multi-AZ truyền thống **chỉ hoạt động trong 1 Region** (giữa các Availability Zones), không thể kéo dài sang 2 Regions khác nhau.
  - **D SAI:** Đề bài yêu cầu cơ sở dữ liệu quan hệ (Relational database), DynamoDB là NoSQL.

---

## 📌 Kịch bản 7: Định tuyến người dùng tới Region có tốc độ nhanh nhất (Task 2.2)

### Đề bài (Exam Prompt):
> A mobile gaming company hosts identical backend server clusters in `us-east-1` (N. Virginia) and `eu-west-1` (Ireland). The company wants to ensure that players across the world are automatically directed to the AWS Region that provides the lowest network latency. Additionally, if the servers in one Region experience an outage, all players should automatically be routed to the healthy Region. How should Amazon Route 53 be configured?

- **A.** Configure a Route 53 Geolocation routing policy with country-level mappings.
- **B.** Configure a Route 53 Latency-based routing policy and associate Route 53 Health Checks with each regional endpoint.
- **C.** Configure a Route 53 Simple routing policy listing the IP addresses of both regional load balancers.
- **D.** Configure a Route 53 Weighted routing policy with equal weights of 50 for each Region.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Latency-based Routing Policy** liên tục đo độ trễ mạng thực tế giữa người dùng và các AWS Regions để điều hướng người dùng tới Region có độ trễ thấp nhất. Khi kết hợp với **Route 53 Health Checks**, nếu Region gần nhất bị lỗi, Route 53 sẽ tự động chuyển hướng người dùng sang Region khỏe mạnh còn lại.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Geolocation điều hướng theo vị trí địa lý thuần túy (quốc gia/châu lục), vị trí địa lý gần chưa chắc đã có tuyến cáp quang độ trễ thấp nhất.
  - **C SAI:** Simple routing trả về IP ngẫu nhiên, không xét đến độ trễ và không hỗ trợ Health Check tự động loại bỏ endpoint hỏng.
  - **D SAI:** Weighted 50/50 chia đều ngẫu nhiên lượng người chơi, một nửa người chơi sẽ bị trễ mạng cao vì bị gửi sang Region ở bên kia đại dương.

---

## 📌 Kịch bản 8: Lựa chọn mô hình Disaster Recovery cân bằng chi phí (Task 2.2)

### Đề bài (Exam Prompt):
> An enterprise needs to design a disaster recovery (DR) architecture for an internal CRM application. The business impact analysis mandates an RTO of 15 minutes and an RPO of 5 minutes. The enterprise wants to minimize ongoing infrastructure running costs in the secondary DR Region. Which DR strategy is the MOST cost-effective choice?

- **A.** Multi-Site Active-Active
- **B.** Warm Standby
- **C.** Pilot Light
- **D.** Backup and Restore

### ✅ Đáp án ĐÚNG: **C**
- **Giải thích kiến trúc:** **Pilot Light** là sự kết hợp hoàn hảo giữa chi phí thấp và tốc độ phục hồi:
  - Tầng dữ liệu cốt lõi (Database) được bật ở kích thước nhỏ nhất và liên tục nhận dữ liệu sao chép (thỏa mãn RPO = 5 phút).
  - Tầng tính toán (EC2/ASG) hoàn toàn **TẮT** để không tốn tiền duy trì 24/7.
  - Khi thảm họa xảy ra, Auto Scaling Group được kích hoạt để khởi tạo máy chủ EC2 từ AMI sẵn có và phóng to database lên, hoàn tất trong vòng 10-15 phút (thỏa mãn RTO = 15 phút) với chi phí rẻ hơn nhiều so với Warm Standby.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Multi-Site Active-Active chạy 100% công suất ở cả 2 Region, chi phí đắt đỏ nhất.
  - **B SAI:** Warm Standby phải bật một cụm máy chủ EC2 chạy liên tục 24/7 ở Region phụ, tốn kém chi phí hơn Pilot Light.
  - **D SAI:** Backup and Restore tuy rẻ nhất nhưng RTO mất nhiều giờ, không thể đạt được mốc RTO 15 phút.

---

## 📌 Kịch bản 9: Sao chép dữ liệu S3 sang Region khác với cam kết 15 phút (Task 2.2)

### Đề bài (Exam Prompt):
> A legal firm stores sensitive contracts in an Amazon S3 bucket. Regulatory compliance requires that all newly uploaded contracts must be replicated to a secondary AWS Region within 15 minutes of upload, and the firm must have SLA-backed verification of this replication window. Which configuration meets this compliance mandate?

- **A.** Configure S3 Same-Region Replication (SRR) with an S3 Lifecycle transition rule.
- **B.** Enable S3 Cross-Region Replication (CRR) and enable S3 Replication Time Control (S3 RTC).
- **C.** Create an AWS Lambda function triggered by S3 `s3:ObjectCreated:*` events to copy objects to the destination bucket.
- **D.** Configure AWS DataSync to run a scheduled synchronization task every 15 minutes.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **S3 Cross-Region Replication (CRR)** kết hợp tính năng **S3 Replication Time Control (S3 RTC)** được thiết kế riêng để đáp ứng các yêu cầu tuân thủ này. S3 RTC cung cấp cam kết thỏa thuận mức dịch vụ (SLA) sao chép **99.9% đối tượng trong vòng 15 phút** và cung cấp các chỉ số CloudWatch giám sát độ trễ sao chép chi tiết.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Đề bài yêu cầu sao chép sang Region khác, SRR là sao chép trong cùng 1 Region.
  - **C SAI:** Lambda tự viết dễ gặp lỗi khi file kích thước lớn, không có cam kết SLA chính thức từ AWS.
  - **D SAI:** AWS DataSync tốn chi phí và công sức quản lý hơn, không phải là tính năng native của S3 và không có SLA 15 phút của S3.

---

## 📌 Kịch bản 10: Chia sẻ ổ đĩa Block Storage cho 2 EC2 trong cùng 1 AZ (Task 2.2)

### Đề bài (Exam Prompt):
> A legacy high-availability application runs on two Amazon EC2 instances in the same Availability Zone. The application architecture requires both instances to concurrently attach to the same shared block storage volume with sub-millisecond latency. Which storage configuration should the solutions architect choose?

- **A.** Create an Amazon EBS General Purpose SSD (`gp3`) volume and attach it to both instances.
- **B.** Create an Amazon EBS Provisioned IOPS SSD (`io2`) volume, enable EBS Multi-Attach, and attach the volume to both EC2 instances.
- **C.** Deploy an Amazon EFS file system with Multi-AZ configuration.
- **D.** Create an Amazon EBS Throughput Optimized HDD (`st1`) volume and configure an NFS server on one EC2 instance.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon EBS Multi-Attach** cho phép gắn 1 volume EBS đồng thời vào tối đa 16 máy chủ EC2 **TRONG CÙNG MỘT AVAILABILITY ZONE**. Tính năng này chỉ hỗ trợ duy nhất trên dòng ổ đĩa Provisioned IOPS SSD (**`io1` và `io2`**), đáp ứng đúng yêu cầu lưu trữ khối đĩa (Block storage) với độ trễ dưới 1 mili-giây.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Ổ `gp3` (và `gp2`) không hỗ trợ tính năng EBS Multi-Attach.
  - **C SAI:** Amazon EFS là hệ thống lưu trữ tệp (File Storage / NFS), không phải lưu trữ khối đĩa (Block storage).
  - **D SAI:** Ổ `st1` là ổ đĩa từ tính HDD, không thể gắn Multi-Attach và không đáp ứng được độ trễ sub-millisecond.
