---
title: "Domain 4 Scenario Walkthroughs: 10 Classic SAA-C03 Exam Scenarios with Distractor Analysis"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "All Tasks (4.1, 4.2, 4.3, 4.4)"
exam_weight: "Very High"
tags:
  - scenarios
  - practice-questions
  - distractor-analysis
  - exam-traps
  - domain4
  - finops
last_updated: 2026-03-30
---

# 🎯 Domain 4 Scenario Walkthroughs: 10 Kịch Bản Đề Thi Thực Tế & Bẫy Distractor

> Trở về: [[00-Domain-4-MOC]]

---

## 📌 Kịch bản 1: Tối ưu chi phí S3 với mô hình truy cập bất định (Task 4.1)

### Đề bài (Exam Prompt):
> A media company stores hundreds of terabytes of user-uploaded video footage in Amazon S3. The access frequency of the footage is completely unpredictable: some videos become viral months after upload and are accessed heavily, while others are rarely accessed. The company wants to minimize ongoing storage costs automatically without any operational overhead and without paying data retrieval charges. Which storage strategy should the company choose?

- **A.** Configure an S3 Lifecycle policy to transition objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days.
- **B.** Move all video footage to Amazon S3 Intelligent-Tiering.
- **C.** Transition objects to Amazon S3 Glacier Flexible Retrieval and use Expedited retrieval when videos become viral.
- **D.** Keep all videos in Amazon S3 Standard and enable S3 Transfer Acceleration.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon S3 Intelligent-Tiering** là lớp lưu trữ hoàn hảo cho dữ liệu có mô hình truy cập **không đoán trước được hoặc biến động liên tục**. Nó tự động chuyển các object giữa các tầng truy cập (Frequent, Infrequent sau 30 ngày, Archive Instant sau 90 ngày) hoàn toàn tự động. Điểm then chốt: **S3 Intelligent-Tiering KHÔNG THU PHÍ TRUY XUẤT DỮ LIỆU (No retrieval fees)**, giúp công ty không bị sốc chi phí khi video cũ đột nhiên viral trở lại.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** S3 Standard-IA thu phí truy xuất dữ liệu ($0.01/GB). Khi video cũ viral và được tải về hàng triệu lần, phí retrieval sẽ làm hóa đơn tăng đột biến.
  - **C SAI:** Glacier Flexible Retrieval mất từ 1-5 phút tới vài giờ để khôi phục, người dùng xem video trực tuyến không thể chờ đợi được.
  - **D SAI:** S3 Standard có phí lưu trữ cao nhất và S3 Transfer Acceleration phát sinh thêm chi phí, không giải quyết bài toán giảm chi phí lưu trữ.

---

## 📌 Kịch bản 2: Aurora MySQL có phí I/O đọc ghi quá cao (Task 4.3)

### Đề bài (Exam Prompt):
> A financial services company runs a high-volume transactional trading application on Amazon Aurora MySQL. Over the last 6 months, user activity surged, causing read and write I/O charges to account for 42% of the total monthly database bill. The finance department complains about unpredictable and volatile database expenses. What is the MOST cost-effective architectural change to reduce and stabilize database costs?

- **A.** Scale up the Aurora primary instance to the largest available `db.r6g.16xlarge` instance class.
- **B.** Modify the Aurora DB cluster storage configuration from Aurora Standard to Aurora I/O-Optimized.
- **C.** Migrate the database tables to Amazon DynamoDB using On-Demand capacity mode.
- **D.** Enable Aurora Auto Scaling to dynamically provision additional Aurora Read Replicas during peak hours.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Theo quy tắc vàng của AWS FinOps, khi **chi phí I/O vượt quá 25% đến 33%** tổng hóa đơn của cụm Aurora Standard, việc chuyển đổi sang **Amazon Aurora I/O-Optimized** sẽ tiết kiệm ngay **30% đến 40% chi phí tổng thể**. Aurora I/O-Optimized **HOÀN TOÀN MIỄN PHÍ TIỀN I/O ($0 I/O charges)**, giúp cố định hóa đơn hàng tháng và loại bỏ hoàn toàn các đợt phát sinh chi phí bất thường do I/O cao.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Nâng cấp instance type làm tăng gấp đôi tiền Compute nhưng không hề làm giảm tiền phí I/O (vẫn tính $0.20/triệu requests) $\rightarrow$ tổng chi phí sẽ tăng cao hơn!
  - **C SAI:** Chuyển đổi từ RDBMS sang NoSQL đòi hỏi phải viết lại mã nguồn toàn bộ hệ thống giao dịch, vi phạm tính khả thi.
  - **D SAI:** Thêm Read Replicas chỉ giải quyết độ trễ đọc, nhưng mỗi Read Replica lại tiếp tục phát sinh thêm phí I/O đọc riêng biệt.

---

## 📌 Kịch bản 3: Tối ưu chi phí Compute ổn định trên đa dịch vụ (Task 4.2)

### Đề bài (Exam Prompt):
> A company runs a production web application consisting of 50 Amazon EC2 instances running Linux, a containerized microservice on AWS Fargate, and several backend AWS Lambda functions. The workloads run continuously 24/7 with steady-state baseline compute usage. Over the next 3 years, the company plans to gradually migrate more EC2 instances to AWS Fargate and adopt AWS Graviton processors. Which purchasing option provides the HIGHEST cost savings with the required architectural flexibility?

- **A.** Purchase 3-year EC2 Standard Reserved Instances.
- **B.** Purchase a 3-year Compute Savings Plan.
- **C.** Purchase 3-year EC2 Instance Savings Plans.
- **D.** Run all workloads using Amazon EC2 Spot Instances with an Auto Scaling group.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Compute Savings Plans** cung cấp mức giảm giá lên tới **66%** với tính linh hoạt tối đa:
  - Tự động áp dụng xuyên suốt giữa **Amazon EC2, AWS Fargate và AWS Lambda**.
  - Không bị ràng buộc vào Region, Hệ điều hành (Linux/Windows), hay Họ máy chủ (cho phép chuyển đổi tự do từ x86 sang chip ARM Graviton).
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** EC2 Standard Reserved Instances bị khóa chặt vào 1 họ máy chủ EC2 cụ thể trong 1 Region, không áp dụng được cho Fargate hay Lambda và không cho phép đổi sang Graviton.
  - **C SAI:** EC2 Instance Savings Plans chỉ áp dụng cho một họ máy chủ EC2 cụ thể (ví dụ chỉ dòng `c5`), không thể áp dụng cho Fargate hay Lambda.
  - **D SAI:** Spot Instances có thể bị AWS thu hồi sau 2 phút, không phù hợp cho các dịch vụ web production ổn định 24/7.

---

## 📌 Kịch bản 4: Xử lý Big Data hàng loạt với Spot Instances (Task 4.2)

### Đề bài (Exam Prompt):
> A genomics lab runs intensive distributed batch processing jobs on an Amazon EMR cluster. The jobs run every weekend, take approximately 12 hours, and are designed to be fault-tolerant with automated checkpointing to Amazon S3. If an individual node fails, the job can resume without restarting from the beginning. What is the MOST cost-effective compute architecture for the EMR worker task nodes?

- **A.** Provision the EMR core and task nodes using On-Demand Instances with Scheduled Scaling.
- **B.** Provision the EMR primary and core nodes on On-Demand Instances, and provision the task nodes on Spot Instances using an EMR instance fleet with the `capacity-optimized` allocation strategy.
- **C.** Purchase 1-year All Upfront Convertible Reserved Instances for all task nodes.
- **D.** Run all cluster nodes on AWS Fargate serverless containers.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** 
  - Trong cụm Amazon EMR: Primary và Core nodes chứa HDFS metadata và lưu trữ dữ liệu phân tán nên cần chạy **On-Demand** để tránh mất cụm.
  - Task nodes chỉ làm nhiệm vụ tính toán thuần túy (không lưu HDFS) và tác vụ có lưu checkpoint trên S3, nên việc chạy **Spot Instances** giúp **tiết kiệm tới 90% chi phí**.
  - Sử dụng chiến lược **`capacity-optimized`** giúp chọn các nhóm Spot Pool có dung lượng dồi dào nhất, giảm thiểu tối đa nguy cơ bị thu hồi máy.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Chạy On-Demand cho các tác vụ batch cuối tuần sẽ đắt hơn gấp nhiều lần so với Spot Instances.
  - **C SAI:** Mua Reserved Instances cho các tác vụ chỉ chạy 2 ngày cuối tuần mỗi tuần là sự lãng phí tiền bạc khổng lồ (phải trả tiền 24/7 cả năm).
  - **D SAI:** EMR chạy trên EC2 clusters hiệu năng cao hơn và tối ưu chi phí hơn Fargate cho các thuật toán Big Data tính toán phân tán nặng.

---

## 📌 Kịch bản 5: Cắt giảm chi phí NAT Gateway bằng VPC Endpoints (Task 4.4)

### Đề bài (Exam Prompt):
> A company has 200 Amazon EC2 instances residing in private subnets across multiple Availability Zones. The instances process analytics data and write 80 TB of transformed data every month to an Amazon S3 bucket. Currently, all outbound traffic from the private subnets routes through a central NAT Gateway in a public subnet. The monthly AWS bill reveals thousands of dollars in NAT Gateway data processing charges. How can the solutions architect eliminate these NAT Gateway processing fees with the LEAST architectural effort?

- **A.** Deploy an additional NAT Gateway in each Availability Zone to load-balance data processing.
- **B.** Create a VPC Gateway Endpoint for Amazon S3 and associate it with the route tables of the private subnets.
- **C.** Create a VPC Interface Endpoint (AWS PrivateLink) for Amazon S3 in each private subnet.
- **D.** Assign public IPv4 addresses to the EC2 instances and route traffic directly through an Internet Gateway.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **VPC Gateway Endpoint cho Amazon S3** là dịch vụ **HOÀN TOÀN MIỄN PHÍ ($0/giờ và $0/GB)**. Bằng cách cập nhật Route Table của các Private Subnet, toàn bộ lưu lượng 80 TB gửi lên S3 sẽ đi thẳng qua mạng nội bộ riêng tư của AWS thay vì đi qua NAT Gateway, **tiết kiệm ngay lập tức $3,600 mỗi tháng** ($0.045 x 80,000 GB = $3,600)!
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Thêm NAT Gateway ở từng AZ chỉ tăng thêm chi phí duy trì theo giờ ($0.045/giờ mỗi NAT) mà không hề làm giảm cước $0.045/GB dữ liệu xử lý.
  - **C SAI (BẪY LỚN):** VPC Interface Endpoint có tính phí **$0.01/GB** dữ liệu xử lý và $0.01/giờ mỗi ENI. Mặc dù rẻ hơn NAT Gateway nhưng vẫn tốn tiền, trong khi **VPC Gateway Endpoint là hoàn toàn MIỄN PHÍ ($0)**!
  - **D SAI:** Vi phạm nguyên tắc an ninh bảo mật mạng vì gán Public IP cho máy chủ nội bộ trong private subnet.

---

## 📌 Kịch bản 6: Nâng cấp ổ cứng gp2 sang gp3 không downtime (Task 4.1)

### Đề bài (Exam Prompt):
> A fleet of 50 Amazon EC2 instances runs on Amazon EBS General Purpose SSD (`gp2`) root volumes totaling 20 TB of provisioned storage. The operations team wants to reduce EBS storage expenses by 20% while providing baseline 3,000 IOPS and 125 MB/s throughput independently of volume size, without incurring any service interruption. Which solution achieves this goal?

- **A.** Create a snapshot of each `gp2` volume, restore each snapshot to a Throughput Optimized HDD (`st1`) volume, and attach it to the instances.
- **B.** Use the EBS Elastic Volumes feature to modify the volume type of all existing volumes from `gp2` to `gp3` while the instances are in service.
- **C.** Stop each EC2 instance, detach the `gp2` volume, attach a newly created `gp3` volume, and restart the instance.
- **D.** Launch an AWS DataSync task to synchronize data from `gp2` volumes to an Amazon EFS file system.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon EBS `gp3`** rẻ hơn `gp2` đúng **20%** và cung cấp sẵn 3,000 IOPS và 125 MB/s miễn phí. Sử dụng tính năng **EBS Elastic Volumes**, quản trị viên có thể thay đổi trực tiếp thuộc tính volume từ `gp2` sang `gp3` ngay khi máy chủ đang chạy mà **không cần dừng máy (Zero Downtime)**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Ổ `st1` là ổ từ tính HDD, **KHÔNG THỂ dùng làm Root Volume (Boot Volume)** cho máy chủ EC2.
  - **C SAI:** Dừng máy chủ (Stop instance) gây gián đoạn dịch vụ không cần thiết (vi phạm yêu cầu *without incurring any service interruption*).
  - **D SAI:** EFS là hệ thống lưu trữ tệp qua mạng NFS, không thể dùng làm ổ đĩa boot hệ điều hành thay cho EBS.

---

## 📌 Kịch bản 7: Chọn chế độ DynamoDB cho ứng dụng nội bộ ít dùng (Task 4.3)

### Đề bài (Exam Prompt):
> A corporate HR department launches an internal employee peer-recognition application. The application stores recognition badges in an Amazon DynamoDB table. Usage is sporadic: the table receives only a few dozen requests per day during business hours, with zero traffic overnight and on weekends. Which DynamoDB capacity setting provides the MOST cost-effective solution?

- **A.** DynamoDB Provisioned capacity mode with minimum 1 RCU and 1 WCU.
- **B.** DynamoDB On-Demand capacity mode.
- **C.** DynamoDB Provisioned capacity mode with Auto Scaling enabled between 5 and 100 RCUs/WCUs.
- **D.** Amazon DynamoDB Global Tables with provisioned capacity across two Regions.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** Ở chế độ **On-Demand Capacity Mode**, bạn chỉ trả tiền chính xác cho số lượng request đọc và ghi thực tế. Khi hệ thống không có người sử dụng (ban đêm, cuối tuần, giữa các giờ làm việc), chi phí dung lượng là **HOÀN TOÀN BẰNG $0.00**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A & C SAI:** Ở chế độ Provisioned, ngay cả khi không có bất kỳ ai truy cập vào ban đêm hay thứ Bảy/Chủ nhật, bạn **vẫn phải trả tiền duy trì số lượng RCU và WCU tối thiểu 24/7** hàng tháng.
  - **D SAI:** Global Tables nhân đôi chi phí và không cần thiết cho một ứng dụng nội bộ đơn giản.

---

## 📌 Kịch bản 8: Tự động khóa tạo tài nguyên khi vượt ngân sách (Task 4.4)

### Đề bài (Exam Prompt):
> A software development company wants to enforce financial governance across its sandbox AWS accounts. The management sets a monthly spending budget of $1,000 per sandbox account. If an account's forecasted spending exceeds the budget threshold, the company must automatically prevent developers from launching any new EC2 instances or creating new RDS databases, while allowing existing workloads to continue running. Which solution satisfies these requirements?

- **A.** Create an Amazon CloudWatch billing alarm that triggers an AWS Lambda function to terminate all running EC2 instances.
- **B.** Configure an AWS Budget with a forecasted threshold of 100% and attach an automated Budget Action that applies a restrictive Service Control Policy (SCP) to the sandbox account.
- **C.** Enable AWS Cost Anomaly Detection to automatically delete unused resources.
- **D.** Write a Python cron job using the AWS SDK to inspect Cost Explorer daily and revoke IAM roles.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **AWS Budgets** hỗ trợ tính năng **Budget Actions**. Khi chi phí thực tế hoặc chi phí dự báo vượt ngưỡng quy định, Budget Action có thể **tự động áp dụng một Service Control Policy (SCP)** lên tài khoản trong AWS Organizations để cấm các lệnh tạo tài nguyên mới (`ec2:RunInstances`, `rds:CreateDBInstance`) mà **không làm gián đoạn các máy chủ đang chạy**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Tiêu hủy toàn bộ máy chủ đang chạy (Terminate instances) sẽ phá hỏng công việc thử nghiệm dở dang của lập trình viên.
  - **C SAI:** Cost Anomaly Detection chỉ phát hiện và gửi cảnh báo SNS, không có tính năng tự động xóa tài nguyên.
  - **D SAI:** Tự viết script vừa tốn công sức vận hành vừa không bảo mật và không có SLA bằng tính năng tích hợp sẵn của AWS Budgets.

---

## 📌 Kịch bản 9: Cắt giảm chi phí CloudFront khi chỉ phục vụ Âu - Mỹ (Task 4.4)

### Đề bài (Exam Prompt):
> A digital marketing agency operates a website whose target audience resides exclusively in the United States, Canada, and Western Europe. Currently, the website uses an Amazon CloudFront distribution configured to use all edge locations globally (`PriceClass_All`). Analysis of CloudFront billing shows high costs due to traffic routed through edge locations in South America and Australia. What should the solutions architect do to reduce CDN distribution costs?

- **A.** Change the CloudFront Price Class to `PriceClass_100`.
- **B.** Implement geo-restriction to block traffic originating from South America and Australia.
- **C.** Delete the CloudFront distribution and use Route 53 Geolocation routing pointing directly to S3.
- **D.** Enable CloudFront Origin Shield in an Asia-Pacific Region.

### ✅ Đáp án ĐÚNG: **A**
- **Giải thích kiến trúc:** **CloudFront `PriceClass_100`** là hạng giá rẻ nhất của CloudFront, chỉ kích hoạt các trạm Edge Locations tại **Bắc Mỹ và Châu Âu** (nơi có chi phí viễn thông rẻ nhất). Bằng cách chuyển sang `PriceClass_100`, công ty loại bỏ hoàn toàn việc bị tính cước theo mức giá đắt đỏ của các trạm tại Nam Mỹ hay Úc.
- **Phân tích bẫy (Distractor Analysis):**
  - **B SAI:** Geo-restriction chỉ chặn người dùng tại các quốc gia đó truy cập website, nhưng nếu distribution vẫn ở `PriceClass_All` thì cấu hình tài nguyên vẫn duy trì giá cao.
  - **C SAI:** Bỏ CloudFront sẽ làm tăng chi phí Data Transfer Out trực tiếp từ S3 và làm chậm tốc độ tải trang của người dùng.
  - **D SAI:** Origin Shield tại Châu Á làm tăng thêm chi phí và đi ngược lại vị trí của khách hàng mục tiêu (Âu - Mỹ).

---

## 📌 Kịch bản 10: Tự động chuyển tệp EFS ít dùng sang Infrequent Access (Task 4.1)

### Đề bài (Exam Prompt):
> An engineering company shares a 30 TB Amazon EFS file system among Linux compute instances. A review of access logs shows that project design files are accessed frequently during the first 14 days of creation, but are rarely accessed after 30 days. The company wants to reduce ongoing file storage expenses without moving files out of the directory tree or altering application access paths. Which solution achieves this?

- **A.** Create an S3 Lifecycle rule to transition old files from EFS to Amazon S3 Glacier Deep Archive.
- **B.** Enable Amazon EFS Lifecycle Management and configure files to transition to EFS Infrequent Access (EFS IA) after 30 days of inactivity.
- **C.** Deploy an AWS Storage Gateway File Gateway to cache files locally.
- **D.** Convert the EFS file system to an Amazon EBS Throughput Optimized HDD (`st1`) volume.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon EFS Lifecycle Management** tự động chuyển các tệp không được truy cập sau số ngày cấu hình (ví dụ 30 ngày) sang tầng lưu trữ **EFS Infrequent Access (EFS IA)**. Tầng EFS IA có chi phí chỉ **$0.025/GB/tháng** (so với $0.30/GB của EFS Standard $\rightarrow$ **tiết kiệm tới 92% chi phí lưu trữ**). Quá trình này hoàn toàn trong suốt: đường dẫn tệp không đổi và ứng dụng đọc tệp bình thường.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** S3 Lifecycle rules chỉ áp dụng cho Amazon S3, không áp dụng trực tiếp lên Amazon EFS.
  - **C SAI:** Storage Gateway dùng để kết nối giữa máy chủ on-premises và AWS, không dùng cho việc tối ưu nội bộ EFS trên cloud.
  - **D SAI:** EBS chỉ gắn được vào 1 máy chủ, không thể chia sẻ đồng thời cho nhiều máy chủ Linux như EFS.
