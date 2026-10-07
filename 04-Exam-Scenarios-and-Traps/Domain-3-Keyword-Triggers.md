---
title: "Domain 3 Keyword Triggers: Direct Exam Terminology to High-Performing Solutions"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "All Tasks (3.1, 3.2, 3.3, 3.4, 3.5)"
exam_weight: "Very High"
tags:
  - keywords
  - triggers
  - cheat-sheet
  - rapid-recall
  - domain3
last_updated: 2026-03-30
---

# ⚡ Domain 3 Keyword Triggers: Bảng Tra Cứu Từ Khóa Phản Xạ Nhanh

> Trở về: [[00-Domain-3-MOC]]  
> **Cách sử dụng:** Khi đọc câu hỏi trong phòng thi, quét tìm các cụm từ khóa (keywords) dưới đây để loại ngay các phương án sai và nhận diện đáp án đúng trong vòng 15 giây.

---

## 1. Storage Performance & S3 Optimization (Task 3.1)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Single-digit millisecond latency for frequently accessed data in a single AZ with 80% lower request costs"*| **Amazon S3 Express One Zone (Directory Buckets)** |
| *"Overcome 503 Slow Down request rate limits on Amazon S3"* | **Distribute objects across multiple S3 Prefixes (3,500/5,500 limit)** |
| *"Accelerate file upload from global branches to S3 using edge locations"*| **Amazon S3 Transfer Acceleration** |
| *"Upload large files exceeding 5 GB or optimize files > 100 MB"* | **Amazon S3 Multipart Upload** |
| *"Download parts of large files in parallel or stream video without full download"*| **S3 Byte-Range Fetches** |
| *"Filter large CSV/JSON files in S3 using simple SQL without transferring full dataset"*| **Amazon S3 Select** |
| *"Cost-effective boot volume with baseline 3,000 IOPS and 125 MB/s independent of size"*| **Amazon EBS `gp3`** |
| *"Mission-critical database requiring sub-millisecond latency and up to 256,000 IOPS"*| **Amazon EBS `io2 Block Express`** |
| *"Throughput-optimized sequential block storage for big data/Kafka (CANNOT be boot volume)"*| **Amazon EBS `st1`** |
| *"Millions of IOPS temporary scratchpad with microsecond latency, data lost on stop"*| **Amazon EC2 Instance Store (NVMe SSD)** |
| *"High-performance computing (HPC) file system with sub-ms latency linked directly to S3"*| **Amazon FSx for Lustre** |
| *"Shared Linux file system that automatically scales throughput based on active workload"*| **Amazon EFS with Elastic Throughput Mode** |

---

## 2. Compute Performance & Placement Groups (Task 3.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Tightly coupled HPC cluster requiring lowest inter-node network latency and 100 Gbps"*| **Cluster Placement Group (single AZ)** |
| *"Isolate a small number of critical instances on distinct underlying hardware racks (max 7/AZ)"*| **Spread Placement Group** |
| *"Distributed big data cluster (HDFS/Cassandra/Kafka) spanning hundreds of nodes across racks"*| **Partition Placement Group** |
| *"OS-bypass communication protocol for MPI-based HPC and distributed deep learning"*| **Elastic Fabric Adapter (EFA)** |
| *"Eliminate cold start latency for AWS Lambda serverless APIs"* | **AWS Lambda Provisioned Concurrency** |
| *"Increase CPU and network throughput for a compute-heavy Lambda function"*| **Increase Lambda Memory (RAM) allocation** |
| *"ARM-based custom silicon offering 25% better performance and 20% lower cost"*| **AWS Graviton3 / Graviton4 instances (e.g. `c7g`, `r7g`)"** |

---

## 3. High-Performing Databases & Caching (Task 3.3)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Relational database scaling instantly in fractions of a second for unpredictable workloads"*| **Amazon Aurora Serverless v2 (ACU scaling)** |
| *"Lambda functions exhausting RDS connection limits with timeouts and connection spikes"*| **Amazon RDS Proxy (Connection Pooling & Multiplexing)** |
| *"Maintain a real-time gaming leaderboard with player scores ranked instantaneously"*| **Amazon ElastiCache for Redis (Sorted Sets: `ZADD`)"** |
| *"In-memory cache with Multi-AZ replication, automatic failover, and persistence"*| **Amazon ElastiCache for Redis** |
| *"Simple multi-threaded in-memory key-value cache with no persistence requirements"*| **Amazon ElastiCache for Memcached** |
| *"In-memory caching layer that automatically scales memory and CPU in seconds without node sizing"*| **Amazon ElastiCache Serverless** |
| *"Sub-millisecond to microsecond read latency cache for Amazon DynamoDB with zero code change"*| **Amazon DynamoDB Accelerator (DAX)** |
| *"Avoid hot partitions and throttle errors in DynamoDB tables"* | **Design high-cardinality Partition Keys with random prefixes** |

---

## 4. Edge Networking & Acceleration (Task 3.4)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Provide two static Anycast IP addresses for global users connecting over TCP or UDP"*| **AWS Global Accelerator** |
| *"Accelerate online multiplayer game running custom UDP protocol with low latency"*| **AWS Global Accelerator** |
| *"Edge caching CDN for HTTP/HTTPS static assets, video streaming, and dynamic content"*| **Amazon CloudFront** |
| *"Centralized caching layer to collapse concurrent requests before hitting origin server"*| **Amazon CloudFront (Origin Shield)** |
| *"Execute lightweight JavaScript at 400+ edge locations with sub-millisecond latency"*| **CloudFront Functions** |
| *"Inspect/modify HTTP request body or make external API calls at the edge"*| **Lambda@Edge** |
| *"Dedicated private fiber network connection between on-premise and AWS with 1-100 Gbps"*| **AWS Direct Connect** |
| *"Aggregate bandwidth across multiple VPN tunnels into AWS Transit Gateway"*| **Equal-Cost Multi-Path (ECMP)** |

---

## 5. Data Ingestion & Analytics Tuning (Task 3.5)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Real-time data streaming pipeline with multi-consumer fan-out and sub-70ms read latency"*| **Amazon Kinesis Data Streams with Enhanced Fan-Out (EFO)** |
| *"Serverless near-real-time streaming delivery into S3/Redshift with automated buffer window"*| **Amazon Kinesis Data Firehose** |
| *"Convert raw JSON/CSV streams automatically into Apache Parquet before writing to S3"*| **Amazon Kinesis Data Firehose (Data Format Conversion)** |
| *"Ad-hoc serverless SQL queries directly on Amazon S3 data lake files at lowest cost"*| **Amazon Athena** |
| *"Optimize Athena query speed and reduce data scan cost by 90%"* | **Convert data to Apache Parquet and partition by date** |
| *"Avoid millions of tiny files slowing down Athena and Glue catalog"* | **Avoid high-cardinality partition keys (The Small Files Trap)** |
| *"Eliminate MSCK REPAIR TABLE commands for heavily partitioned tables in Athena"*| **Enable Partition Projection in AWS Glue Data Catalog** |
| *"Enterprise petabyte-scale data warehouse with columnar storage and complex SQL joins"*| **Amazon Redshift** |
| *"Query data residing in both Redshift and open S3 data lake formats without loading"*| **Amazon Redshift Spectrum** |
| *"Full-text search, autocomplete suggestions, and security log visualization dashboards"*| **Amazon OpenSearch Service (with Dashboards)** |
| *"Online high-speed automated file synchronization over network between on-prem and AWS"*| **AWS DataSync** |
| *"Offline physical data transfer of 80 TB to 100 PB when network bandwidth is insufficient"*| **AWS Snowball Edge / AWS Snowmobile** |
