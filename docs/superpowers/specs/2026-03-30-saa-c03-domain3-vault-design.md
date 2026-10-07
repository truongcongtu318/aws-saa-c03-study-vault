# Spec: AWS SAA-C03 Domain 3 (Design High-Performing Architectures) Study Vault

- **Date**: 2026-03-30
- **Target**: AWS Certified Solutions Architect – Associate (SAA-C03 / 2026)
- **Primary Focus**: Domain 3: Design High-Performing Architectures (24% Exam Score Weight)
- **Format**: Obsidian-Compatible Markdown Vault & GitHub-Friendly Knowledge Base
- **Language Policy**: Bilingual (Official AWS English terminology & exam questions; Vietnamese architectural explanations, decision logic, and exam trap analysis)

---

## 1. Executive Summary & Intent

Domain 3 accounts for **24%** of the SAA-C03 exam score. It tests the architect's ability to select and tune high-speed storage, compute, database, networking, and data ingestion architectures for maximum throughput, lowest latency (sub-millisecond), and optimal scalability.

This module incorporates the latest 2025-2026 SAA-C03 syllabus updates, specifically:
- **Amazon S3 Express One Zone** (Directory buckets, single-digit millisecond latency, single AZ co-location).
- **Amazon EBS io2 Block Express** (sub-millisecond latency, up to 256,000 IOPS and 4,000 MB/s).
- **Serverless Instant Scaling**: **Amazon Aurora Serverless v2** (fine-grained ACU scaling in fractions of a second) and **Amazon ElastiCache Serverless**.
- **Data Lake Query Tuning**: **Amazon Athena + AWS Glue + Apache Parquet** (columnar compression, partition pruning, avoiding the "small files trap", partition projection).
- **Specialized HPC & AI Accelerators**: **Elastic Fabric Adapter (EFA)**, **Cluster Placement Groups**, and **AWS Graviton3/4**.
- **Edge Acceleration**: **AWS Global Accelerator** (Static Anycast IPs for TCP/UDP) vs **Amazon CloudFront** (CDN caching), and **CloudFront Functions** vs **Lambda@Edge**.

---

## 2. Five Task Statements & Core Technical Pillars

1. **Task 3.1: Determine high-performing and/or scalable storage solutions**
   - S3 Express One Zone: Directory buckets, single-digit ms latency, 80% lower request costs, single AZ co-location for ML/financial models.
   - S3 Performance: Partition prefixes (3,500 PUT/POST/DELETE & 5,500 GET/HEAD req/s per prefix), Multipart Upload (>100MB / >5GB), S3 Transfer Acceleration, Byte-Range Fetches, S3 Select.
   - EBS Performance: `gp3` (baseline 3,000 IOPS & 125 MB/s free, scales to 16,000 IOPS & 1,000 MB/s independent of size) vs `io2 Block Express` (1,000 IOPS/GB up to 256,000 IOPS, SAN in the cloud).
   - EBS Throughput Optimized HDD (`st1`) vs Cold HDD (`sc1`) - sequential large I/O, non-bootable.
   - Instance Store: NVMe local SSD, microsecond latency, ephemeral (data lost on stop/failure), RAID 0 striping.
   - EFS: Elastic Throughput mode vs Provisioned/Bursting; General Purpose mode vs Max I/O mode.
   - FSx Family: FSx for Lustre (HPC/AI, sub-ms latency, bidirectional sync with S3) vs FSx for Windows File Server (SMB/Active Directory).
2. **Task 3.2: Design high-performing and elastic compute solutions**
   - EC2 Instance Families: Compute Optimized (`C`), Memory Optimized (`R`/`X`/`Z`), Storage Optimized (`I`/`D`), Accelerated Computing (`P`/`G`/`Trn`/`Inf`), Graviton (`c7g`/`r7g`).
   - Placement Groups: Cluster (single AZ, lowest latency, 100Gbps network) vs Spread (max 7 instances per AZ on distinct hardware racks) vs Partition (distributed big data Hadoop/Cassandra/Kafka).
   - HPC Accelerators: Enhanced Networking Adapter (ENA) vs Elastic Fabric Adapter (EFA, OS-bypass MPI for tightly coupled HPC).
   - AWS Lambda Performance: Memory-to-CPU scaling (up to 10GB RAM), Provisioned Concurrency to eliminate cold starts.
3. **Task 3.3: Determine high-performing database solutions**
   - In-Memory Caching: Amazon ElastiCache for Redis (complex data structures, multi-AZ, failover, pub/sub) vs Memcached (multi-threaded simple key-value) vs ElastiCache Serverless.
   - Caching Strategies: Lazy Loading (Cache-Aside) vs Write-Through vs TTL.
   - DynamoDB Accelerator (DAX): Microsecond read latency for DynamoDB.
   - Amazon Aurora Serverless v2: Instant ACU scaling, Multi-AZ, Read Replicas (up to 15), Aurora Global Database.
   - Amazon RDS Proxy: Connection pooling, preventing Lambda from exhausting database connection limits, 66% faster failover.
   - Amazon Redshift: Columnar storage, MPP architecture, Redshift Spectrum (direct SQL on S3 data lake).
4. **Task 3.4: Determine high-performing and/or scalable network architectures**
   - Amazon CloudFront: Global CDN, Origin Shield, TLS session reuse, caching static/dynamic content.
   - Edge Compute: CloudFront Functions (JS, sub-ms runtime, viewer request/response, header manipulation) vs Lambda@Edge (Node/Python, 10s runtime, origin request/response, network access, heavy logic).
   - AWS Global Accelerator: 2 Static Anycast IPs, routing over AWS private backbone, TCP/UDP traffic acceleration without caching, instant failover (<1 min).
   - Hybrid Connectivity: AWS Direct Connect (dedicated 1-100 Gbps fiber) vs AWS Transit Gateway with Equal-Cost Multi-Path (ECMP) over multiple VPN tunnels.
5. **Task 3.5: Determine high-performing data ingestion and transformation solutions**
   - Amazon Kinesis Data Streams: Real-time streaming, On-Demand mode vs Provisioned Shards (1MB/s in, 2MB/s out), multi-consumer.
   - Amazon Kinesis Data Firehose: Serverless near-real-time ingestion (buffering by size/time), built-in Lambda transform, direct delivery to S3/Redshift/OpenSearch.
   - Analytics Query Optimization: Amazon Athena serverless SQL on S3; AWS Glue ETL converting raw CSV/JSON to Apache Parquet/ORC; Partition Pruning vs Small Files Trap; Partition Projection.
   - Amazon OpenSearch Service: Full-text search, autocomplete, log analytics.
   - AWS DataSync: High-speed automated data transfer over network (10x faster than rsync) between on-prem and S3/EFS/FSx.
   - AWS Snow Family: Snowcone (8TB), Snowball Edge (80TB), Snowmobile (100PB) for offline petabyte ingestion.

---

## 3. Vault Information Architecture for Domain 3

```text
D:\xbrain\saa/
├── 00-Index/
│   └── 00-Domain-3-MOC.md                                    # Master MOC & Study Tracker for Domain 3
├── 01-Task-Statements/                                       # 5 Task Statements
│   ├── Task-3.1-High-Performing-Storage.md
│   ├── Task-3.2-High-Performing-Compute.md
│   ├── Task-3.3-High-Performing-Databases.md
│   ├── Task-3.4-High-Performing-Networking.md
│   └── Task-3.5-High-Performing-Data-Ingestion.md
├── 02-Deep-Dives/                                            # 10 Chuyên đề kỹ thuật chuyên sâu
│   ├── S3-Express-One-Zone-and-Storage-Performance.md
│   ├── EBS-Volume-Types-gp3-and-io2-Block-Express.md
│   ├── EFS-and-FSx-High-Throughput-Modes.md
│   ├── EC2-Placement-Groups-and-HPC-Accelerators.md
│   ├── Lambda-Performance-and-Provisioned-Concurrency.md
│   ├── Caching-Deep-Dive-ElastiCache-and-DAX.md
│   ├── Aurora-Serverless-v2-and-RDS-Proxy.md
│   ├── Data-Lake-Query-Tuning-Athena-Glue-Parquet.md
│   ├── CloudFront-vs-Global-Accelerator-Edge-Performance.md
│   └── Real-Time-Streaming-Kinesis-and-MSK.md
├── 03-Decision-Matrices/                                     # 7 Ma trận so sánh & Lưu đồ ra quyết định
│   ├── Matrix-EBS-Volume-Types.md
│   ├── Matrix-S3-Performance-Options.md
│   ├── Matrix-EC2-Placement-Groups.md
│   ├── Matrix-ElastiCache-Redis-vs-Memcached.md
│   ├── Matrix-Analytics-Athena-vs-Redshift-vs-OpenSearch.md
│   ├── Matrix-CloudFront-vs-Global-Accelerator.md
│   └── Matrix-CloudFront-Functions-vs-Lambda-at-Edge.md
├── 04-Exam-Scenarios-and-Traps/                              # Luyện thi & Phản xạ bẫy đề thi
│   ├── Domain-3-Scenario-Walkthroughs.md
│   └── Domain-3-Keyword-Triggers.md
└── 05-Flashcards-and-Review/
    └── Domain-3-Quick-Recall.md
```

---

## 4. Acceptance Criteria

1. **Obsidian Compatibility**: Complete bidirectional Wikilinks (`[[...]]`), valid YAML frontmatter, and Mermaid diagrams.
2. **Exhaustive Domain 3 Coverage**: All 5 Task Statements and 2026 modern features (S3 Express One Zone, Aurora Serverless v2, ElastiCache Serverless, Parquet partition pruning, io2 Block Express, EFA) fully articulated.
3. **7 Decision Matrices**: Crisp comparison tables and decision trees.
4. **10 Detailed Scenario Walkthroughs**: Complete with question, 4 choices, correct answer explanation, and distractor traps analysis.
5. **30 Active Recall Flashcards**: Interactive `<details><summary>` toggle cards.
