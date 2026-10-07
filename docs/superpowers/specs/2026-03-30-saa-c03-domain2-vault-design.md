# Spec: AWS SAA-C03 Domain 2 (Design Resilient Architectures) Study Vault

- **Date**: 2026-03-30
- **Target**: AWS Certified Solutions Architect – Associate (SAA-C03 / 2026)
- **Primary Focus**: Domain 2: Design Resilient Architectures (26% Exam Score Weight)
- **Format**: Obsidian-Compatible Markdown Vault & GitHub-Friendly Knowledge Base
- **Language Policy**: Bilingual (Official AWS English terminology & exam questions; Vietnamese architectural explanations, decision logic, and exam trap analysis)

---

## 1. Executive Summary & Intent

Domain 2 accounts for **26%** of the SAA-C03 exam score (the 2nd largest domain after Domain 1). It tests the candidate's ability to architect distributed cloud systems that survive component outages, scale dynamically under variable load, decouple dependencies to avoid cascading failure, and meet strict Disaster Recovery (DR) Recovery Point Objectives (RPO) and Recovery Time Objectives (RTO).

This spec outlines the design for the Domain 2 module inside the vault `D:\xbrain\saa`, integrating with Domain 1 through bidirectional wikilinks (`[[...]]`) and standard YAML frontmatter.

---

## 2. Seven Core Technical Pillars (Deep Dive Scope)

1. **Decoupling & Asynchronous Messaging:**
   - Amazon SQS: Standard vs FIFO, Visibility Timeout (30s default, max 12h, `ChangeMessageVisibility`), Long Polling (`WaitTimeSeconds`: 1-20s), Dead Letter Queues (DLQ with `maxReceiveCount`), SQS Extended Client Library (payloads > 256KB stored in S3).
   - Amazon SNS: Pub/Sub, Fan-out pattern to SQS queues, SNS Message Filtering (Filter Policy by JSON attributes).
   - Amazon EventBridge: Event Bus, JSON schema rules, Archive & Replay of historical events.
   - AWS Step Functions: State machines, Standard vs Express workflows, error handling, retries and catchers.
   - Amazon MQ: Managed ActiveMQ/RabbitMQ for legacy lift-and-shift without code rewrite.
2. **Elastic Load Balancing (ELB):**
   - Application Load Balancer (ALB): Layer 7, path/host/header/query routing, Server Name Indication (SNI) for multiple SSL certs, Cognito/OIDC user authentication, cross-zone balancing enabled by default (free).
   - Network Load Balancer (NLB): Layer 4, millions of requests/sec with ultra-low latency, static IP / Elastic IP per AZ, client IP preservation, cross-zone balancing disabled by default (cross-AZ data transfer fee if enabled).
   - Gateway Load Balancer (GWLB): Layer 3/4, GENEVE protocol, transparent routing to 3rd-party virtual appliances.
   - Connection Draining / Deregistration Delay (default 300s).
3. **Compute Resilience & Auto Scaling Groups (ASG):**
   - Launch Templates vs Launch Configurations (Launch Templates are current best practice).
   - Scaling Policies: Target Tracking, Step Scaling, Simple Scaling, Scheduled Scaling, Predictive Scaling.
   - Instance Termination Policies: Default prioritization (AZ with most instances -> oldest launch template/config -> closest to billing hour).
   - ASG Lifecycle Hooks: `autoscaling:EC2_INSTANCE_LAUNCHING` (`Pending:Wait`) and `autoscaling:EC2_INSTANCE_TERMINATING` (`Terminating:Wait`).
4. **Data Layer High Availability & Fault Tolerance:**
   - Amazon RDS Multi-AZ: Synchronous physical replication, standby instance in another AZ, automatic failover (60-120s, DNS CNAME switch), standby cannot be used for read/write.
   - Amazon RDS Read Replicas: Asynchronous replication, read scaling (up to 15 replicas for MySQL/PostgreSQL), can be cross-region for DR, can be promoted to standalone DB.
   - Amazon Aurora: 6 copies of data across 3 AZs (4/6 quorum write, 3/6 quorum read), auto-healing, up to 15 Aurora Replicas with sub-10ms replication lag, Aurora Global Database (storage replication across regions with <1s latency, <1min failover).
   - Amazon DynamoDB: Multi-AZ native, DynamoDB Global Tables (multi-region active-active), Point-in-Time Recovery (PITR, 35-day continuous backup).
5. **Amazon Route 53 Routing & Failover:**
   - Alias Records (Zone Apex `example.com`, free AWS queries) vs CNAME (subdomain only, external queries).
   - 7 Routing Policies: Simple, Weighted, Latency-based, Failover (Active-Passive with Health Checks), Geolocation, Geoproximity (Traffic Flow with Bias), Multi-value Answer.
6. **Disaster Recovery (DR) Strategies & RTO/RPO Spectrum:**
   - RPO (maximum acceptable data loss) vs RTO (maximum acceptable downtime).
   - 4 Architectural Tiers:
     1. Backup & Restore (RTO: hours, RPO: hours, lowest cost).
     2. Pilot Light (Core DB replicated & running small, compute stopped/AMIs ready, RTO: 10s of mins, RPO: mins).
     3. Warm Standby (Scaled-down version running continuously, RTO: mins, RPO: seconds).
     4. Multi-Site Active-Active (Full production running in multiple regions, RTO: ~0, RPO: ~0, highest cost).
   - AWS Elastic Disaster Recovery (AWS DRS): Continuous block-level replication.
7. **Storage Durability & Data Resiliency:**
   - Amazon S3: 99.999999999% (11 9's) durability across 3 AZs, Cross-Region Replication (CRR), Versioning, S3 Replication Time Control (S3 RTC - 99.9% replicated within 15 mins).
   - Amazon EBS: Replicated within a single AZ, EBS Snapshots (incremental, stored in S3), Fast Snapshot Restore (FSR), EBS Multi-Attach (`io1`/`io2` only, single AZ only, clustered filesystem needed).
   - Amazon EFS: Multi-AZ POSIX shared file system (Regional), concurrently accessed by hundreds of EC2 instances.
   - Amazon FSx: FSx for Windows File Server (Single-AZ vs Multi-AZ SMB), FSx for Lustre (HPC scratch vs persistent).

---

## 3. Vault Information Architecture for Domain 2

```text
D:\xbrain\saa/
├── 00-Index/
│   └── 00-Domain-2-MOC.md                                    # Master Map of Content & Study Tracker for Domain 2
├── 01-Task-Statements/
│   ├── Task-2.1-Scalable-and-Loosely-Coupled.md              # Task 2.1 official blueprint & concepts
│   └── Task-2.2-Highly-Available-and-Fault-Tolerant.md       # Task 2.2 official blueprint & concepts
├── 02-Deep-Dives/
│   ├── EC2-Auto-Scaling-and-Compute-Resilience.md            # ASG policies, Lifecycle hooks, Termination policy
│   ├── Elastic-Load-Balancing-Deep-Dive.md                   # ALB vs NLB vs GWLB, SNI, Cross-Zone, Draining
│   ├── Route-53-Routing-Policies-and-Failover.md             # 7 Routing policies, Health Checks, Alias vs CNAME
│   ├── Decoupling-with-SQS-and-SNS.md                        # SQS Visibility Timeout, DLQ, FIFO, SNS Fan-out & Filter
│   ├── Event-Driven-Architectures-EventBridge-StepFunctions.md # EventBridge Event Bus, Archive/Replay, Step Functions
│   ├── RDS-and-Aurora-High-Availability.md                   # RDS Multi-AZ vs Read Replica, Aurora 6-copy & Global DB
│   ├── DynamoDB-Resilience-and-Global-Tables.md              # Global Tables Active-Active, Streams, PITR
│   ├── Storage-Durability-S3-EBS-EFS-FSx.md                  # S3 CRR/RTC, EBS Snapshots/Multi-Attach, EFS, FSx
│   └── Disaster-Recovery-Strategies-RTO-RPO.md               # 4 DR tiers (Backup/Pilot/Warm/Active-Active), AWS DRS
├── 03-Decision-Matrices/
│   ├── Matrix-ALB-vs-NLB-vs-GWLB.md                          # So sánh chuyên sâu 3 loại Load Balancer
│   ├── Matrix-SQS-vs-SNS-vs-EventBridge-vs-Kinesis.md        # Queue vs PubSub vs Event Bus vs Streaming
│   ├── Matrix-RDS-Multi-AZ-vs-Read-Replica.md                # Tính sẵn sàng (HA) vs Tăng tốc đọc (Read scale)
│   ├── Matrix-Disaster-Recovery-Tiers.md                     # Bảng so sánh RTO, RPO, Chi phí 4 mô hình DR
│   ├── Matrix-Route-53-Routing-Policies.md                   # Bảng tra cứu "Khi nào dùng Routing Policy nào"
│   └── Matrix-Storage-Resilience-EBS-vs-EFS-vs-S3.md         # Block vs File vs Object Storage Resilience
├── 04-Exam-Scenarios-and-Traps/
│   ├── Domain-2-Scenario-Walkthroughs.md                     # 10 Kịch bản đề thi thực tế Domain 2 có phân tích bẫy
│   └── Domain-2-Keyword-Triggers.md                          # Bảng tra cứu từ khóa phản xạ nhanh Domain 2
└── 05-Flashcards-and-Review/
    └── Domain-2-Quick-Recall.md                              # 30 Câu hỏi phản xạ nhanh Domain 2
```

---

## 4. Acceptance Criteria

1. **Obsidian Compatibility**: Complete bidirectional Wikilinks (`[[...]]`), valid YAML frontmatter, and Mermaid diagrams.
2. **Exhaustive Domain 2 Coverage**: All 7 technical nuance pillars completely articulated.
3. **6 Decision Matrices**: Crisp comparison tables and decision trees.
4. **10 Detailed Scenario Walkthroughs**: Full question, 4 choices, correct answer explanation, and distractor traps analysis.
5. **30 Active Recall Flashcards**: Interactive `<details><summary>` toggle cards.
