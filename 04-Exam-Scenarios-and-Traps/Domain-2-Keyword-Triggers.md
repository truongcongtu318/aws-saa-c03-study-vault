---
title: "Domain 2 Keyword Triggers: Direct Exam Terminology to Resilient Architecture Solutions"
domain: "Domain 2: Design Resilient Architectures"
task_statement: "All Tasks (2.1, 2.2)"
exam_weight: "Very High"
tags:
  - keywords
  - triggers
  - cheat-sheet
  - rapid-recall
  - domain2
last_updated: 2026-03-30
---

# ⚡ Domain 2 Keyword Triggers: Bảng Tra Cứu Từ Khóa Phản Xạ Nhanh

> Trở về: [[00-Domain-2-MOC]]  
> **Cách sử dụng:** Khi đọc câu hỏi trong phòng thi, quét tìm các cụm từ khóa (keywords) dưới đây để loại ngay các phương án sai và nhận diện đáp án đúng trong vòng 15 giây.

---

## 1. Decoupling, Messaging & Event-Driven (Task 2.1)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Buffer incoming requests to decouple web frontend from database"* | **Amazon SQS Queue** |
| *"Worker processing takes longer than default, messages processed twice"* | **Increase Visibility Timeout / `ChangeMessageVisibility`** |
| *"Reduce SQS cost and empty responses when queue is mostly empty"* | **Enable SQS Long Polling (`WaitTimeSeconds: 1-20s`)** |
| *"Isolate poisoned messages after failing processing multiple times"* | **SQS Dead Letter Queue (DLQ) with `maxReceiveCount`** |
| *"Send SQS messages with payload size exceeding 256 KB up to 2 GB"* | **Amazon SQS Extended Client Library (using Amazon S3)** |
| *"Strict FIFO ordering and exactly-once processing with deduplication"* | **Amazon SQS FIFO Queue (with `MessageDeduplicationId`)** |
| *"Broadcast one event to multiple independent downstream queues"* | **Amazon SNS Fan-out to Amazon SQS** |
| *"Deliver only specific messages to specific subscribers based on JSON attributes"*| **SNS Message Filtering (Filter Policy)** |
| *"Lift-and-shift legacy application using RabbitMQ or ActiveMQ without rewriting code"*| **Amazon MQ** |
| *"Replay past events after fixing downstream bug in event-driven architecture"*| **Amazon EventBridge Archive & Replay** |
| *"Coordinate multi-step workflow with branching logic and wait states up to 1 year"*| **AWS Step Functions Standard Workflow** |
| *"Ultra-high throughput workflow execution (100k/s) under 5 minutes duration"*| **AWS Step Functions Express Workflow** |

---

## 2. Load Balancing, Scaling & Compute Resilience (Task 2.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Route traffic based on URL path (/api) or host header"* | **Application Load Balancer (ALB)** |
| *"Host multiple SSL certificates on a single load balancer at lowest cost"* | **Application Load Balancer with SNI (Server Name Indication)** |
| *"Require a fixed static IP or Elastic IP per AZ for firewall whitelisting"*| **Network Load Balancer (NLB)** |
| *"Handle millions of requests per second with ultra-low microsecond latency"*| **Network Load Balancer (NLB)** |
| *"Transparently scale 3rd-party virtual firewalls using GENEVE protocol"*| **Gateway Load Balancer (GWLB)** |
| *"Ensure in-flight requests complete before instance is removed from target group"*| **Deregistration Delay / Connection Draining** |
| *"Send subsequent requests from same client to same EC2 instance (Stateful app)"*| **Sticky Sessions (Session Affinity)** |
| *"Perform cleanup actions or export logs to S3 before ASG terminates instance"*| **Auto Scaling Lifecycle Hook (`EC2_INSTANCE_TERMINATING`)** |
| *"Maintain average fleet CPU utilization at 50% with least operational overhead"*| **ASG Target Tracking Scaling Policy** |
| *"Scale instance count based on time-of-day predictable schedule"* | **ASG Scheduled Scaling** |
| *"Combine On-Demand and Spot instances across multiple instance types"* | **EC2 Auto Scaling with Launch Templates** |

---

## 3. High Availability Databases & DNS Failover (Task 2.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Relational database high availability with automatic failover in 60-120 seconds"*| **Amazon RDS Multi-AZ Deployment** |
| *"Offload heavy analytical/read queries from primary database instance"*| **Amazon RDS Read Replicas (point queries to Reader Endpoint)** |
| *"Aurora storage resilience surviving loss of an entire AZ without losing write capability"*| **Amazon Aurora 6-copy storage across 3 AZs (4/6 Quorum)** |
| *"Cross-region relational database DR with RPO < 1s and RTO < 1 minute"* | **Amazon Aurora Global Database** |
| *"Multi-region active-active NoSQL database with local read/write access globally"*| **Amazon DynamoDB Global Tables** |
| *"Restore DynamoDB table to any second in the past 35 days without performance impact"*| **DynamoDB Point-In-Time Recovery (PITR)** |
| *"Capture real-time item modifications in DynamoDB to trigger Lambda"*| **DynamoDB Streams** |
| *"Microsecond read latency for DynamoDB cache without application code rewrite"*| **Amazon DynamoDB Accelerator (DAX)** |
| *"Map the root domain (zone apex) to an ALB at zero DNS query cost"* | **Amazon Route 53 Alias Record (A-Record)** |
| *"Divert global users to the AWS Region with lowest network latency"* | **Route 53 Latency-based Routing Policy** |
| *"Automatically failover to secondary DR site when primary health check fails"*| **Route 53 Failover Routing Policy (Active-Passive)** |
| *"Canary testing / A/B testing directing 10% traffic to new software version"*| **Route 53 Weighted Routing Policy** |

---

## 4. Storage Durability & Disaster Recovery (Task 2.2)

| Cụm từ khóa trong đề thi (Exam Keyword Trigger) | Đáp án kỹ thuật chính xác (Correct Solution) |
| :--- | :--- |
| *"Replicate S3 objects to another Region with guaranteed 15-minute SLA"*| **S3 Cross-Region Replication (CRR) with S3 RTC** |
| *"Instantly restore EBS volume from snapshot with full baseline IOPS without latency"*| **Amazon EBS Fast Snapshot Restore (FSR)** |
| *"Attach a single block storage volume concurrently to multiple EC2 instances in the same AZ"*| **Amazon EBS Multi-Attach (`io1` / `io2` only)** |
| *"Shared POSIX file system accessible concurrently by hundreds of Linux EC2s across AZs"*| **Amazon EFS (Regional Multi-AZ)** |
| *"Shared file storage for Windows workloads supporting SMB protocol and Active Directory"*| **Amazon FSx for Windows File Server (Multi-AZ)** |
| *"High-performance computing (HPC) file system linked to Amazon S3"* | **Amazon FSx for Lustre** |
| *"Lowest cost DR strategy, application can tolerate several hours of downtime"*| **Backup and Restore** |
| *"DR strategy: core database running small continuously, compute launched on-demand"*| **Pilot Light** |
| *"DR strategy: scaled-down minimum version running continuously, scaled up rapidly during disaster"*| **Warm Standby** |
| *"Zero downtime, business-critical application running full capacity in 2+ regions"*| **Multi-Site Active-Active** |
| *"Continuous block-level replication from on-premise servers to AWS with RPO in seconds"*| **AWS Elastic Disaster Recovery (AWS DRS)** |
