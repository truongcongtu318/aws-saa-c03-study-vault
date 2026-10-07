---
title: "Domain 3 Scenario Walkthroughs: 10 Classic SAA-C03 Exam Scenarios with Distractor Analysis"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "All Tasks (3.1, 3.2, 3.3, 3.4, 3.5)"
exam_weight: "Very High"
tags:
  - scenarios
  - practice-questions
  - distractor-analysis
  - exam-traps
  - domain3
last_updated: 2026-03-30
---

# 🎯 Domain 3 Scenario Walkthroughs: 10 Kịch Bản Đề Thi Thực Tế & Bẫy Distractor

> Trở về: [[00-Domain-3-MOC]]

---

## 📌 Kịch bản 1: Huấn luyện Machine Learning với S3 Express One Zone (Task 3.1)

### Đề bài (Exam Prompt):
> A machine learning research team runs distributed training jobs on an Amazon EKS cluster deployed in `us-east-1a`. The training jobs frequently access millions of small training images from Amazon S3, generating sustained rates of hundreds of thousands of requests per second. The team experiences data access bottlenecks and wants to achieve consistent single-digit millisecond read/write latency while minimizing request costs. Which storage solution satisfies these requirements?

- **A.** Store the training data in an Amazon S3 Standard bucket and configure S3 Transfer Acceleration.
- **B.** Store the training data in an Amazon S3 One Zone-Infrequent Access (One Zone-IA) bucket.
- **C.** Store the training data in an Amazon S3 Express One Zone directory bucket co-located in `us-east-1a`.
- **D.** Copy the training data to Amazon EBS General Purpose SSD (`gp3`) volumes attached to the worker nodes.

### ✅ Đáp án ĐÚNG: **C**
- **Giải thích kiến trúc:** **Amazon S3 Express One Zone** là lớp lưu trữ đối tượng dạng **Directory Buckets** được đặt tại **1 Availability Zone cụ thể**. Nó cung cấp độ trễ truy xuất dữ liệu nhất quán **dưới 10 mili-giây (Single-digit ms)** (nhanh gấp **10 lần** S3 Standard) và **chi phí gọi Request rẻ hơn tới 80%**. Khi co-locate cùng AZ với cụm EKS compute, nó loại bỏ hoàn toàn độ trễ mạng liên vùng.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** S3 Standard có độ trễ 10-20ms và chi phí request tiêu chuẩn, S3 Transfer Acceleration chỉ tối ưu cho đường truyền upload từ xa qua Internet chứ không giảm độ trễ nội bộ trong AZ.
  - **B SAI (BẪY KINH ĐIỂN):** S3 One Zone-IA dùng cho dữ liệu *ít truy cập*. Khi đọc hàng triệu file liên tục, **phí truy xuất dữ liệu (Data Retrieval Charge) của One Zone-IA sẽ cực kỳ đắt đỏ**!
  - **D SAI:** EBS gp3 không thể chia sẻ đồng thời cho hàng trăm pod EKS trên nhiều máy chủ khác nhau (EBS chỉ gắn 1 máy chủ, trừ khi dùng io2 Multi-Attach phức tạp).

---

## 📌 Kịch bản 2: Cơ sở dữ liệu Mission-Critical đòi hỏi 80,000 IOPS (Task 3.1)

### Đề bài (Exam Prompt):
> An enterprise is migrating an on-premises Oracle database to Amazon EC2. The database workload requires a 1 TB block storage volume with sustained performance of 80,000 IOPS and sub-millisecond response latency. High durability is mandatory. Which EBS volume configuration meets these performance criteria?

- **A.** An Amazon EBS General Purpose SSD (`gp3`) volume with provisioned 80,000 IOPS.
- **B.** An Amazon EBS Provisioned IOPS SSD (`io2 Block Express`) volume.
- **C.** An Amazon EBS Throughput Optimized HDD (`st1`) volume.
- **D.** An Amazon EC2 Instance Store NVMe SSD volume.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon EBS `io2 Block Express`** cung cấp hiệu năng tối đa lên tới **256,000 IOPS** và **4,000 MB/s Throughput**, độ trễ **dưới 1 mili-giây** và độ bền bỉ đạt **99.999%** (5 số 9). Tỷ lệ IOPS/GB của `io2 Block Express` đạt tới 1,000:1 (với 1 TB có thể cấp phát tới 80,000 IOPS dễ dàng).
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Ổ `gp3` bị giới hạn cứng ở mức tối đa **16,000 IOPS**, không thể đáp ứng mức 80,000 IOPS.
  - **C SAI:** Ổ `st1` là ổ từ tính HDD, tối đa chỉ đạt 500 IOPS và độ trễ cao.
  - **D SAI:** EC2 Instance Store tuy có IOPS cao nhưng là **bộ nhớ tạm thời (Ephemeral)**, dữ liệu bị mất trắng khi máy chủ dừng (Stop) $\rightarrow$ không đảm bảo an toàn cho cơ sở dữ liệu quan trọng.

---

## 📌 Kịch bản 3: Tối ưu mạng cho tính toán hiệu năng cao HPC (Task 3.2)

### Đề bài (Exam Prompt):
> An engineering company runs a tightly coupled computational fluid dynamics (CFD) simulation on a cluster of Amazon EC2 GPU instances. The simulation uses Message Passing Interface (MPI) libraries and requires ultra-low inter-node latency and high network throughput to exchange calculations between nodes. Which combination of networking and placement configurations should the company implement?

- **A.** Launch instances in a Spread Placement Group and attach Enhanced Networking (ENA) interfaces.
- **B.** Launch instances in a Cluster Placement Group and attach Elastic Fabric Adapters (EFA).
- **C.** Launch instances in a Partition Placement Group and configure an AWS Transit Gateway.
- **D.** Launch instances across multiple Availability Zones behind a Network Load Balancer.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** 
  1. **Cluster Placement Group:** Gom toàn bộ các máy chủ vào cùng 1 rack vật lý trong 1 AZ duy nhất, đạt băng thông mạng tối đa (100 - 400 Gbps) và độ trễ liên node thấp nhất.
  2. **Elastic Fabric Adapter (EFA):** Card mạng chuyên dụng có tính năng **OS-bypass** cho phép các máy chủ trao đổi trực tiếp bộ nhớ RAM qua giao thức **MPI** mà không qua nhân hệ điều hành.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Spread Placement Group phân tán máy chủ trên các rack khác nhau, làm tăng độ trễ mạng giữa các node.
  - **C SAI:** Partition Placement Group dùng cho Hadoop/Kafka dữ liệu lớn, không tối ưu cho giao tiếp song song MPI.
  - **D SAI:** NLB và đa vùng làm tăng độ trễ mạng thêm hàng chục mili-giây, phá hủy hiệu năng của cụm CFD.

---

## 📌 Kịch bản 4: Giải quyết cạn kiệt kết nối DB từ AWS Lambda (Task 3.3)

### Đề bài (Exam Prompt):
> A retail company operates a serverless application using AWS Lambda functions that process customer orders and write records to an Amazon RDS for PostgreSQL database. During peak shopping events, Lambda scales out to thousands of concurrent executions, causing database CPU spikes and leading to `connection limit exceeded` errors. What is the MOST effective architectural solution to resolve this bottleneck?

- **A.** Increase the `max_connections` parameter in the PostgreSQL DB Parameter Group to 100,000.
- **B.** Deploy Amazon RDS Proxy between the Lambda functions and the PostgreSQL database.
- **C.** Convert the PostgreSQL database to Amazon DynamoDB.
- **D.** Configure an Amazon ElastiCache for Memcached cluster to buffer order writes.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon RDS Proxy** nằm giữa Lambda và cơ sở dữ liệu, cung cấp tính năng **Connection Pooling & Multiplexing**. Thay vì mở hàng nghìn kết nối riêng rẽ, RDS Proxy gom các request lại và tái sử dụng một nhóm nhỏ kết nối mở sẵn tới DB, ngăn chặn hoàn toàn việc cạn kiệt tài nguyên bộ nhớ của PostgreSQL.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Tăng `max_connections` quá cao sẽ khiến RAM của máy chủ RDS bị cạn kiệt cho việc quản lý process kết nối, dẫn đến crash toàn bộ cơ sở dữ liệu.
  - **C SAI:** Đổi từ RDBMS sang NoSQL đòi hỏi phải viết lại toàn bộ mã nguồn ứng dụng và thay đổi cấu trúc dữ liệu, không phải là giải pháp kiến trúc giải quyết bài toán kết nối.
  - **D SAI:** Memcached là bộ nhớ đệm đọc, không phải là hàng đợi đệm ghi giao dịch (Transaction buffering).

---

## 📌 Kịch bản 5: Bảng xếp hạng game thời gian thực (Task 3.3)

### Đề bài (Exam Prompt):
> A mobile gaming company needs to implement a global real-time leaderboard for an online multiplayer game. The leaderboard must rank millions of active players by their game score, support frequent score updates with sub-millisecond latency, and retrieve top-ranking player lists instantaneously. Which database/caching technology is BEST suited for this workload?

- **A.** Amazon RDS for MySQL with Read Replicas.
- **B.** Amazon DynamoDB with standard Scan operations.
- **C.** Amazon ElastiCache for Redis using the Sorted Sets data structure.
- **D.** Amazon DocumentDB (with MongoDB compatibility).

### ✅ Đáp án ĐÚNG: **C**
- **Giải thích kiến trúc:** **Amazon ElastiCache for Redis** hỗ trợ cấu trúc dữ liệu chuyên biệt **Sorted Sets (`ZADD`, `ZRANGE`)**, lưu trữ toàn bộ dữ liệu trên RAM. Redis tự động duy trì thứ tự xếp hạng của hàng triệu phần tử theo điểm số với độ phức tạp $O(\log N)$ và độ trễ **dưới 1 mili-giây**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Cơ sở dữ liệu quan hệ RDS khi phải sắp xếp (Sort/Order By) liên tục trên hàng triệu dòng dữ liệu ghi dồn dập sẽ bị nghẽn I/O đĩa từ.
  - **B SAI:** DynamoDB Scan rất chậm và tốn kém RCU, không hỗ trợ sắp xếp toàn bảng theo điểm số thời gian thực nếu không query qua GSI.
  - **D SAI:** DocumentDB lưu trữ trên ổ đĩa phân tán, không đạt được tốc độ in-memory và không có sẵn thuật toán xếp hạng Sorted Sets.

---

## 📌 Kịch bản 6: Tối ưu hóa truy vấn Data Lake với Athena và Parquet (Task 3.5)

### Đề bài (Exam Prompt):
> A data analytics team analyzes 20 TB of application access logs stored in Amazon S3 as raw JSON files. The team uses Amazon Athena to run ad-hoc analytical SQL queries. The queries frequently run for over 10 minutes and incur significant costs due to scanning terabytes of data. Most queries only filter by the `event_timestamp` and aggregate two specific numeric metrics. How can the team optimize query performance and reduce costs?

- **A.** Load the JSON logs into Amazon Redshift and run queries using Redshift Spectrum.
- **B.** Use an AWS Glue ETL job to convert the raw JSON files into Apache Parquet format, partition the data by date (`year/month/day`), and query the partitioned Parquet files using Athena.
- **C.** Increase the provisioned query processing units (QPUs) allocated to Amazon Athena.
- **D.** Create an Amazon CloudFront distribution in front of the S3 bucket to cache query results.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:**
  1. **Apache Parquet:** Là định dạng lưu trữ theo cột (**Columnar Format**). Athena chỉ cần đọc đúng 2 cột số liệu cần thiết thay vì đọc toàn bộ file JSON, giảm tới **90% lượng dữ liệu quét (Data Scanned)**.
  2. **Partitioning theo ngày:** Cho phép **Partition Pruning**, Athena bỏ qua toàn bộ các thư mục ngày không liên quan trong mệnh đề `WHERE` $\rightarrow$ truy vấn nhanh gấp hàng chục lần và giảm 90% chi phí.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** Tạo cụm Redshift tốn thêm chi phí duy trì cụm máy chủ 24/7 và công sức nạp dữ liệu.
  - **C SAI:** Amazon Athena là dịch vụ Serverless thuần túy, người dùng **không thể tự cấp phát QPU** cho Athena query tiêu chuẩn.
  - **D SAI:** CloudFront dùng để cache tải file tĩnh cho người dùng web, không dùng để tối ưu hóa truy vấn SQL của Athena.

---

## 📌 Kịch bản 7: Tăng tốc Game UDP toàn cầu với IP tĩnh (Task 3.4)

### Đề bài (Exam Prompt):
> A gaming studio launches an online multiplayer game running custom UDP game servers deployed across two AWS Regions: `us-west-2` and `eu-central-1`. Global players report high ping latency and packet loss when connecting across public Internet transit providers. The client game client requires two fixed static IP addresses to establish UDP socket connections. Which solution resolves this problem?

- **A.** Place the game servers behind an Amazon CloudFront distribution and enable CloudFront Origin Shield.
- **B.** Deploy an Application Load Balancer in each Region and assign two Elastic IP addresses to each ALB.
- **C.** Deploy an AWS Global Accelerator, configure two Static Anycast IP addresses, and set the regional game server endpoints as targets.
- **D.** Use Amazon Route 53 with Latency-based routing pointing directly to the EC2 instances.

### ✅ Đáp án ĐÚNG: **C**
- **Giải thích kiến trúc:** **AWS Global Accelerator** cung cấp **2 địa chỉ Static Anycast IP cố định**. Toàn bộ lưu lượng **UDP** của người chơi được dẫn vào Edge Location gần nhất và truyền tải xuyên lục địa trên mạng cáp quang riêng (AWS Global Backbone) có độ trễ cực thấp, không bị packet loss của Internet công cộng.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** **Amazon CloudFront KHÔNG HỖ TRỢ giao thức UDP thuần** (chỉ hỗ trợ HTTP, HTTPS, WebSocket).
  - **B SAI:** **ALB không hỗ trợ gán Elastic IP trực tiếp**, và ALB là Layer 7 (chỉ hỗ trợ HTTP/HTTPS, không hỗ trợ game server UDP).
  - **D SAI:** Route 53 chỉ phân giải DNS ban đầu, các gói tin UDP sau đó vẫn phải đi qua Internet công cộng đầy rủi ro và không cung cấp 2 IP tĩnh cố định theo yêu cầu.

---

## 📌 Kịch bản 8: Chuyển hướng URL siêu nhẹ tại biên (Task 3.4)

### Đề bài (Exam Prompt):
> An e-commerce website uses Amazon CloudFront to serve global traffic. The marketing team requires a solution to inspect incoming request URLs, strip unnecessary query parameters, and redirect mobile visitors (`User-Agent: Mobile`) to `/mobile/index.html`. The solution must execute with sub-millisecond latency, handle millions of requests per second, and minimize operational costs. Which technology should be chosen?

- **A.** Deploy an AWS Lambda function behind an Application Load Balancer to perform URL rewrites.
- **B.** Associate a CloudFront Functions function with the Viewer Request trigger on the CloudFront distribution.
- **C.** Associate a Lambda@Edge function with the Origin Request trigger on the CloudFront distribution.
- **D.** Configure Apache rewrite rules in an EC2 Auto Scaling group behind CloudFront.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **CloudFront Functions** được xây dựng bằng JavaScript siêu nhẹ, chạy trực tiếp tại hơn **400+ Edge Locations** của CloudFront với thời gian thực thi **dưới 1 mili-giây (< 1ms)** và chi phí **rẻ hơn 6 lần so với Lambda@Edge**. Hoàn hảo cho các tác vụ: URL Rewrite/Redirect, chuẩn hóa Header, chỉnh sửa Query parameters trên sự kiện **Viewer Request**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A & D SAI:** Đẩy request ngược về ALB/EC2 làm mất toàn bộ ý nghĩa của việc xử lý tại biên, gây trễ mạng và tốn kém chi phí máy chủ.
  - **C SAI:** Lambda@Edge có thời gian khởi động lâu hơn, chi phí đắt hơn 6 lần, và chạy tại 13 Regional Edge Caches (xa người dùng hơn so với 400+ Edge Locations của CloudFront Functions).

---

## 📌 Kịch bản 9: Cơ sở dữ liệu tự mở rộng tức thì không gián đoạn (Task 3.3)

### Đề bài (Exam Prompt):
> A news publication experiences unpredictable traffic spikes whenever breaking news occurs. During these events, database transaction volume increases by 500% within 30 seconds. The database runs on relational MySQL. The management mandates that the database must scale compute capacity automatically in fractions of a second without downtime or dropping active user sessions, while maintaining enterprise Multi-AZ high availability. Which database option meets these requirements?

- **A.** Amazon RDS for MySQL with an Auto Scaling storage policy.
- **B.** Amazon Aurora Serverless v2 MySQL-Compatible Edition.
- **C.** Amazon DynamoDB with On-Demand capacity mode.
- **D.** Amazon Aurora Serverless v1 MySQL-Compatible Edition.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon Aurora Serverless v2** có khả năng tự động mở rộng và thu hẹp tài nguyên tính toán (ACU) **trong vài phần trăm giây** một cách hoàn toàn trong suốt, không làm rớt kết nối của người dùng. Hỗ trợ đầy đủ các tính năng doanh nghiệp: **Multi-AZ, Read Replicas và Global Database**.
- **Phân tích bẫy (Distractor Analysis):**
  - **A SAI:** RDS Storage Auto Scaling chỉ mở rộng dung lượng ổ đĩa đĩa cứng (GB), không thể tự mở rộng CPU hay RAM của máy chủ database.
  - **C SAI:** Đề bài yêu cầu cụ thể hệ thống cơ sở dữ liệu quan hệ tương thích MySQL (Relational MySQL), DynamoDB là NoSQL.
  - **D SAI:** Aurora Serverless v1 mất hàng chục giây đến cả phút để scale, không thể scale trong 30 giây mà không bị đứt kết nối.

---

## 📌 Kịch bản 10: Tự động chuyển đổi định dạng Streaming sang Parquet (Task 3.5)

### Đề bài (Exam Prompt):
> A fleet of 50,000 IoT delivery trucks emits GPS telemetry records as continuous JSON streams. A data engineering team needs to ingest these streams, convert the raw JSON records into Apache Parquet format, and deliver the compressed files directly into an Amazon S3 data lake bucket with minimal operational overhead. Which architecture should be deployed?

- **A.** Stream data into Amazon SQS FIFO, use an EC2 worker fleet to batch records into Parquet, and upload to S3.
- **B.** Stream data into Amazon Kinesis Data Firehose, configure built-in data format conversion to Apache Parquet with an AWS Glue Data Catalog schema, and set Amazon S3 as the destination.
- **C.** Stream data into Amazon Kinesis Data Streams, write a custom Lambda consumer to parse JSON, and write individual objects to S3.
- **D.** Stream data into Amazon OpenSearch Service, and export indices to S3 nightly.

### ✅ Đáp án ĐÚNG: **B**
- **Giải thích kiến trúc:** **Amazon Kinesis Data Firehose** là dịch vụ Serverless hoàn toàn được thiết kế sẵn tính năng **Tự động chuyển đổi định dạng (Data Format Conversion)** sang **Apache Parquet hoặc ORC**. Nó tự động tham chiếu lược đồ từ **AWS Glue Data Catalog**, đệm dữ liệu theo kích thước tối ưu (ví dụ 128 MB) và đẩy thẳng vào Amazon S3 mà không cần tự dựng hay quản lý bất kỳ máy chủ nào (**Minimal operational overhead**).
- **Phân tích bẫy (Distractor Analysis):**
  - **A & C SAI:** Tự viết code EC2 hoặc Lambda để parse JSON và lưu file đòi hỏi nhiều công sức vận hành, dễ tạo ra bẫy file nhỏ (Small files trap) trên S3 và tốn kém chi phí hơn nhiều.
  - **D SAI:** OpenSearch dùng để tìm kiếm log, không phải là công cụ nạp dữ liệu data lake và việc export hàng đêm làm mất tính chất liên tục của luồng dữ liệu.
