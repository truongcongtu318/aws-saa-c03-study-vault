---
title: "Domain 4 Keyword Triggers: Direct Exam Terminology to Cost-Optimized Solutions"
domain: "Domain 4: Design Cost-Optimized Architectures"
task_statement: "All Tasks (4.1, 4.2, 4.3, 4.4)"
exam_weight: "Very High"
tags:
  - keywords
  - triggers
  - cheat-sheet
  - rapid-recall
  - domain4
  - finops
last_updated: 2026-03-30
---

# ⚡ Domain 4 Keyword Triggers: Bảng Tra Cứu Từ Khóa Phản Xạ Nhanh

> Trở về: [[00-Domain-4-MOC]]  
> **Cách sử dụng:** Khi đọc câu hỏi trong phòng thi, quét tìm các cụm từ khóa (keywords) dưới đây để loại ngay các phương án lãng phí chi phí và nhận diện đáp án tối ưu nhất trong vòng 15 giây.

---

## 1. Storage Cost Optimization (Task 4.1)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Unpredictable or changing access patterns, no retrieval fees, automated cost savings"*| **Amazon S3 Intelligent-Tiering** |
| *"Archived records retrieved rarely, but requires immediate millisecond access when requested"*| **Amazon S3 Glacier Instant Retrieval** |
| *"Lowest cost long-term archival storage in AWS for regulatory compliance (7-10 years)"*| **Amazon S3 Glacier Deep Archive (~$1/TB/month)** |
| *"Clean up storage costs from abandoned multi-part file uploads in S3"*| **S3 Lifecycle rule: Abort Incomplete Multipart Uploads** |
| *"Reduce EBS SSD storage costs by 20% while keeping baseline 3,000 IOPS with zero downtime"*| **Migrate EBS volume type from `gp2` to `gp3` (Elastic Volumes)** |
| *"Sequential high-throughput big data or log storage at lowest block storage cost"*| **Amazon EBS Throughput Optimized HDD (`st1`)** |
| *"Reduce file storage costs by up to 92% for files not accessed in 30 days without path changes"*| **Amazon EFS Lifecycle Management to EFS Infrequent Access (EFS IA)** |
| *"Find and delete unused EBS volumes not attached to any EC2 instance"*| **Delete EBS volumes in `available` state** |
| *"Make third-party data consumers pay for their own S3 download transfer costs"*| **Enable S3 Requester Pays on the bucket** |

---

## 2. Compute Cost Optimization (Task 4.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Fault-tolerant batch jobs, rendering, or CI/CD pipelines requiring up to 90% cost savings"*| **Amazon EC2 Spot Instances** |
| *"Steady-state 24/7 compute workloads across EC2, Fargate, and Lambda for 1 or 3 years"*| **Compute Savings Plans (up to 66% discount)** |
| *"Commitment to a specific EC2 instance family in a specific Region for 3 years (up to 72% off)"*| **EC2 Instance Savings Plans** |
| *"Minimize risk of Spot instance interruptions across multiple instance types and AZs"*| **Spot Fleet with `capacity-optimized` allocation strategy** |
| *"Receive machine learning recommendations to downsize over-provisioned EC2 instances"*| **AWS Compute Optimizer** |
| *"Run stateless microservices without paying for idle provisioned server infrastructure"*| **AWS Lambda or AWS Fargate** |
| *"Shut down development and test environments during non-business hours automatically"*| **AWS Instance Scheduler or scheduled Lambda functions** |

---

## 3. Database Cost Optimization (Task 4.3)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Aurora I/O charges exceed 25%-33% of total database bill, need predictable costs"*| **Switch storage configuration to Amazon Aurora I/O-Optimized** |
| *"Low-to-moderate I/O workload on Amazon Aurora with large storage footprint"*| **Keep on Amazon Aurora Standard** |
| *"Sporadic or unpredictable DynamoDB traffic with long idle periods and zero traffic overnight"*| **Amazon DynamoDB On-Demand Capacity Mode** |
| *"Predictable, steady-state high-volume DynamoDB read/write traffic at lowest cost"*| **DynamoDB Provisioned Capacity Mode with Auto Scaling** |
| *"Offload heavy repetitive SELECT read queries from Amazon RDS to avoid expensive up-sizing"*| **Amazon ElastiCache (In-memory caching)** |
| *"Reduce DynamoDB read costs by 50% without changing application schema"*| **Switch queries to Eventually Consistent Reads** |

---

## 4. Networking & FinOps Governance (Task 4.4)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Eliminate expensive NAT Gateway data processing fees ($0.045/GB) for EC2 accessing S3"*| **Create a VPC Gateway Endpoint for Amazon S3 (Route Table)** |
| *"Free private connectivity between private subnets and DynamoDB without NAT Gateway"*| **Create a VPC Gateway Endpoint for DynamoDB (Route Table)** |
| *"Target audience located only in US and Europe, reduce CloudFront distribution costs"*| **Set CloudFront Price Class to `PriceClass_100`** |
| *"Set spending limits and automatically apply an SCP to restrict resource creation when exceeded"*| **AWS Budgets with automated Budget Actions** |
| *"Machine learning tool to detect unexpected cost spikes and identify root cause proactively"*| **AWS Cost Anomaly Detection** |
| *"Track and allocate cloud spending across departments, projects, and cost centers"*| **Activate Cost Allocation Tags in AWS Billing Console** |
| *"Consolidate billing across 20 AWS accounts to qualify for tier-based volume discounts"*| **Consolidated Billing in AWS Organizations** |
| *"Forecast future cloud spending for the next 12 months based on historical usage"*| **AWS Cost Explorer** |
