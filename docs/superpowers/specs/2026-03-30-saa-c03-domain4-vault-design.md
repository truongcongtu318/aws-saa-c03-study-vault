# Spec: AWS SAA-C03 Domain 4 (Design Cost-Optimized Architectures) Study Vault

- **Date**: 2026-03-30
- **Target**: AWS Certified Solutions Architect – Associate (SAA-C03 / 2026)
- **Primary Focus**: Domain 4: Design Cost-Optimized Architectures (20% Exam Score Weight)
- **Milestone**: Reaching 100% Complete SAA-C03 Exam Blueprint Coverage (Domains 1 + 2 + 3 + 4)
- **Format**: Obsidian-Compatible Markdown Vault & GitHub-Friendly Knowledge Base
- **Language Policy**: Bilingual (Official AWS English terminology & exam questions; Vietnamese architectural explanations, decision logic, and exam trap analysis)

---

## 1. Executive Summary & Intent

Domain 4 accounts for **20%** of the SAA-C03 exam score. It tests an architect's ability to eliminate waste, leverage optimal purchasing models, minimize data transfer fees, and select cost-effective architectural tiers without compromising security, reliability, or performance.

With Domain 4, this vault completes the full 100% blueprint of the AWS Certified Solutions Architect – Associate exam:
- **Domain 1**: Design Secure Architectures (30%)
- **Domain 2**: Design Resilient Architectures (26%)
- **Domain 3**: Design High-Performing Architectures (24%)
- **Domain 4**: Design Cost-Optimized Architectures (20%)

This module incorporates the latest 2025-2026 SAA-C03 syllabus updates, specifically:
- **Amazon Aurora I/O-Optimized** vs **Standard** (the 25%–33% I/O spend tipping point).
- **Amazon S3 Intelligent-Tiering** vs **Glacier Instant / Flexible / Deep Archive** with minimum duration (30/90/180 days) and minimum size (128 KB) billing constraints.
- **Compute Pricing Models**: **Spot Instances** (up to 90% discount, 2-minute interruption notice, Spot Fleets) vs **Compute Savings Plans** (up to 66% discount, flexible across EC2/Fargate/Lambda) vs **EC2 Instance Savings Plans** vs **Reserved Instances**.
- **Data Transfer Pricing Traps**: **NAT Gateway data processing charges** ($0.045/GB) vs **VPC Gateway Endpoints** ($0/GB and $0/hr) for S3 and DynamoDB; Cross-AZ ($0.01/GB) vs Internet Egress ($0.09/GB).
- **Cost Governance & FinOps**: **AWS Compute Optimizer** ML right-sizing recommendations, **AWS Budgets** with automated actions, **AWS Cost Anomaly Detection**, and **Cost Allocation Tags**.

---

## 2. Four Task Statements & Core Technical Pillars

1. **Task 4.1: Design cost-optimized storage solutions**
   - S3 Storage Classes: Standard, Intelligent-Tiering (no retrieval fees), Standard-IA, One Zone-IA, Glacier Instant Retrieval (ms access, 90-day min), Glacier Flexible Retrieval (minutes/hours, 90-day min), Glacier Deep Archive ($1/TB/month, 180-day min).
   - S3 Lifecycle Rules: Transition actions, expiration actions, abort incomplete multipart uploads.
   - S3 Requester Pays buckets.
   - EBS Optimization: Migrating `gp2` to `gp3` (20% savings + free 3,000 IOPS/125 MB/s), using `st1`/`sc1` for sequential throughput, finding and deleting unattached EBS volumes and stale snapshots.
   - EFS Lifecycle Management: Automatic transition to EFS Infrequent Access (EFS IA) for 92% cost savings.
2. **Task 4.2: Design cost-optimized compute solutions**
   - EC2 Pricing Models: On-Demand (unpredictable/short-term), Spot Instances (fault-tolerant/batch up to 90% off), Compute Savings Plans (flexible EC2/Fargate/Lambda up to 66% off), EC2 Instance Savings Plans (up to 72% off), Reserved Instances (Standard vs Convertible).
   - Right-Sizing with AWS Compute Optimizer: Identifying underutilized instances based on 14-32 days of CloudWatch metrics.
   - Auto Scaling: Dynamic & scheduled scale-in during off-peak hours to eliminate idle capacity.
   - Serverless Compute Economics: AWS Lambda pay-per-use vs AWS Fargate vs EC2 cluster management overhead.
3. **Task 4.3: Design cost-optimized database solutions**
   - Amazon Aurora Storage Configurations: Aurora Standard vs Aurora I/O-Optimized (tipping point when I/O costs exceed 25%-33% of total database bill, $0 I/O charges).
   - Amazon DynamoDB Capacity Modes: On-Demand (unpredictable/infrequent/spiky traffic) vs Provisioned with Auto Scaling (steady/predictable high volume).
   - Amazon RDS Cost Reduction: Stopping non-production databases (Dev/Test) during off-hours, purchasing RDS Reserved Instances for production, using ElastiCache to offload reads instead of upgrading instance size.
4. **Task 4.4: Design cost-optimized network architectures**
   - Data Transfer Pricing Rules: Ingress ($0), Same AZ Private IP ($0), Cross-AZ ($0.01/GB each way), Cross-Region ($0.02/GB), Internet Egress ($0.09/GB).
   - NAT Gateway vs VPC Endpoints: Eliminating NAT Gateway data processing fees ($0.045/GB) by implementing free VPC Gateway Endpoints for S3 and DynamoDB.
   - Amazon CloudFront Cost Optimization: Reducing data transfer out (DTO) from origin to edge, selecting CloudFront Price Classes (`PriceClass_100`, `PriceClass_200`, `PriceClass_All`).
   - Hybrid Networking: Direct Connect vs Internet VPN egress economics.
   - Cost Governance & FinOps: AWS Cost Explorer, AWS Budgets with action triggers (SNS, SCP, stop EC2), AWS Cost Anomaly Detection, Cost Allocation Tags, Consolidated Billing volume discounts.

---

## 3. Vault Information Architecture for Domain 4

```text
D:\xbrain\saa/
├── 00-Index/
│   └── 00-Domain-4-MOC.md                                    # Master MOC & Study Tracker cho Domain 4
├── 01-Task-Statements/                                       # 4 Task Statements
│   ├── Task-4.1-Cost-Optimized-Storage.md
│   ├── Task-4.2-Cost-Optimized-Compute.md
│   ├── Task-4.3-Cost-Optimized-Databases.md
│   └── Task-4.4-Cost-Optimized-Networking.md
├── 02-Deep-Dives/                                            # 9 Chuyên đề kỹ thuật chuyên sâu
│   ├── S3-Lifecycle-and-Storage-Classes-Cost-Tuning.md
│   ├── EC2-Pricing-Models-Spot-Savings-Plans-and-RIs.md
│   ├── Aurora-Storage-Configurations-Standard-vs-IO-Optimized.md
│   ├── Data-Transfer-Costs-and-VPC-Endpoints-vs-NAT.md
│   ├── DynamoDB-Cost-Models-On-Demand-vs-Provisioned.md
│   ├── EFS-and-EBS-Storage-Cost-Optimization.md
│   ├── AWS-Compute-Optimizer-and-Right-Sizing.md
│   ├── AWS-Cost-Governance-Budgets-Anomaly-and-Tags.md
│   └── CloudFront-Cost-Optimization-and-Price-Classes.md
├── 03-Decision-Matrices/                                     # 6 Ma trận so sánh & Lưu đồ ra quyết định
│   ├── Matrix-EC2-Pricing-Models.md
│   ├── Matrix-S3-Storage-Classes-Cost-and-Retrieval.md
│   ├── Matrix-Aurora-Standard-vs-IO-Optimized.md
│   ├── Matrix-NAT-Gateway-vs-VPC-Endpoints.md
│   ├── Matrix-DynamoDB-On-Demand-vs-Provisioned.md
│   └── Matrix-AWS-Cost-Management-Tools.md
├── 04-Exam-Scenarios-and-Traps/                              # Luyện thi & Phản xạ bẫy đề thi
│   ├── Domain-4-Scenario-Walkthroughs.md
│   └── Domain-4-Keyword-Triggers.md
└── 05-Flashcards-and-Review/
    └── Domain-4-Quick-Recall.md
```

---

## 4. Acceptance Criteria

1. **Obsidian Compatibility**: Complete bidirectional Wikilinks (`[[...]]`), valid YAML frontmatter, and Mermaid diagrams.
2. **Exhaustive Domain 4 Coverage**: All 4 Task Statements and 2026 FinOps nuances fully articulated.
3. **6 Decision Matrices**: Crisp comparison tables and decision trees.
4. **10 Detailed Scenario Walkthroughs**: Complete with question, 4 choices, correct answer explanation, and distractor traps analysis.
5. **30 Active Recall Flashcards**: Interactive `<details><summary>` toggle cards.
6. **Full 100% Blueprint Master Integration**: All 4 MOCs and blueprints interconnected.
