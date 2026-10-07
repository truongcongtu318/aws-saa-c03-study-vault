# SAA-C03 Domain 2 (Design Resilient Architectures) Vault Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a comprehensive, bilingual, Obsidian-compatible study vault and knowledge hub for AWS SAA-C03 Domain 2 (Design Resilient Architectures) covering all 7 technical nuance pillars, 2 official task statements, 9 deep dives, 6 decision matrices, 10 exam scenarios, and 30 flashcards.

**Architecture:** Build a 5-tier knowledge vault module featuring Map of Content (MOC), official Task Statements, deep dive technical guides, comparative decision matrices, and exam scenario/flashcard sets with bidirectional wikilinks (`[[...]]`) and Mermaid diagrams.

**Tech Stack:** Markdown (CommonMark + Obsidian Flavored Markdown), YAML Frontmatter, Mermaid.js diagrams.

**Spec:** [docs/superpowers/specs/2026-03-30-saa-c03-domain2-vault-design.md](docs/superpowers/specs/2026-03-30-saa-c03-domain2-vault-design.md)

## Global Constraints

- Root workspace is `D:\xbrain\saa`.
- Every note MUST contain valid YAML frontmatter (`title`, `domain`, `task_statement`, `exam_weight`, `services`, `tags`, `last_updated`).
- Bilingual standard: English for AWS official terms, architecture names, CLI/API parameters, and exam scenarios; Vietnamese for architectural explanations, trade-off analysis, decision rationale, and exam traps.
- All inter-note references must use Obsidian Wikilinks syntax `[[Target-Note-Name]]`.
- Diagrams must use standard Mermaid.js syntax (`graph TD`, `sequenceDiagram`, `flowchart LR`, `mindmap`).

## Review Focus

- Correctness of technical details: SQS Visibility Timeout (30s default, max 12h), Long Polling (1-20s), FIFO deduplication (`MessageDeduplicationId`, `MessageGroupId`).
- Distinction between RDS Multi-AZ (synchronous, standby non-readable) vs Read Replicas (asynchronous, read-scaling, cross-region).
- Distinction between ALB (L7, path/host routing, SNI) and NLB (L4, static/Elastic IP per AZ, million req/s, low latency).
- RTO vs RPO exact definitions and clear tiering across Backup & Restore, Pilot Light, Warm Standby, and Multi-Site Active-Active.
- Route 53: Alias Record (Zone Apex, free) vs CNAME (subdomain only).

---

### Task 1: Domain 2 Foundation & Index

**Files:**
- Create: `00-Index/00-Domain-2-MOC.md`
- Modify: `README.md`
- Modify: `00-Index/SAA-C03-Exam-Blueprint.md`

- [ ] **Step 1: Create `00-Index/00-Domain-2-MOC.md`**
  - Author the Map of Content (MOC) with Mermaid mindmap, Dataview-ready table, status indicators (🟢 Completed), and direct links to all tasks, deep dives, matrices, and practice materials for Domain 2.
- [ ] **Step 2: Update `README.md`**
  - Add Domain 2 to the master directory guide and overview.
- [ ] **Step 3: Update `00-Index/SAA-C03-Exam-Blueprint.md`**
  - Link directly to `[[00-Domain-2-MOC]]`.

---

### Task 2: Official Task Statements Mapping (Task 2.1 & 2.2)

**Files:**
- Create: `01-Task-Statements/Task-2.1-Scalable-and-Loosely-Coupled.md`
- Create: `01-Task-Statements/Task-2.2-Highly-Available-and-Fault-Tolerant.md`

- [ ] **Step 1: Author `Task-2.1-Scalable-and-Loosely-Coupled.md`**
  - Cover horizontal vs vertical scaling, stateless vs stateful workloads, SQS, SNS, EventBridge, Step Functions, API Gateway throttling/caching.
- [ ] **Step 2: Author `Task-2.2-Highly-Available-and-Fault-Tolerant.md`**
  - Cover multi-AZ/multi-Region strategies, Elastic Load Balancing, compute fault tolerance, RDS/Aurora/DynamoDB resiliency, Route 53 health check failover, and DR models.

---

### Task 3: Decoupling & Event-Driven Deep Dives

**Files:**
- Create: `02-Deep-Dives/Decoupling-with-SQS-and-SNS.md`
- Create: `02-Deep-Dives/Event-Driven-Architectures-EventBridge-StepFunctions.md`

- [ ] **Step 1: Author `02-Deep-Dives/Decoupling-with-SQS-and-SNS.md`**
  - Detail SQS Visibility Timeout (`ChangeMessageVisibility`), Long Polling vs Short Polling, Dead Letter Queues (`maxReceiveCount`), SQS Extended Client (S3 pointer for >256KB), FIFO deduplication & message groups.
  - Detail SNS Fan-out to SQS, SNS Message Filtering (Filter Policy by JSON attributes), Amazon MQ (RabbitMQ/ActiveMQ lift-and-shift).
- [ ] **Step 2: Author `02-Deep-Dives/Event-Driven-Architectures-EventBridge-StepFunctions.md`**
  - Detail EventBridge Event Buses, rules pattern matching, schema registry, Archive and Replay.
  - Detail AWS Step Functions Standard vs Express workflows, Task/Choice/Wait/Fail states, error handling (`Retry` and `Catch`).

---

### Task 4: Load Balancing & Compute Resilience Deep Dives

**Files:**
- Create: `02-Deep-Dives/Elastic-Load-Balancing-Deep-Dive.md`
- Create: `02-Deep-Dives/EC2-Auto-Scaling-and-Compute-Resilience.md`

- [ ] **Step 1: Author `02-Deep-Dives/Elastic-Load-Balancing-Deep-Dive.md`**
  - Detail ALB (Layer 7, path/host/query routing, SNI, Cognito/OIDC authentication).
  - Detail NLB (Layer 4, static/Elastic IP per AZ, millions req/s, client IP preservation).
  - Detail Gateway Load Balancer (GWLB, GENEVE protocol).
  - Detail Cross-Zone Load Balancing mechanics and costs (ALB enabled free vs NLB disabled by default).
  - Detail Deregistration Delay / Connection Draining (default 300s).
- [ ] **Step 2: Author `02-Deep-Dives/EC2-Auto-Scaling-and-Compute-Resilience.md`**
  - Launch Templates vs Launch Configurations.
  - Scaling Policies: Target Tracking, Step Scaling, Simple Scaling, Scheduled, Predictive.
  - Termination Policies (AZ with most instances -> oldest template/config -> closest to billing hour).
  - Lifecycle Hooks: `autoscaling:EC2_INSTANCE_LAUNCHING` and `autoscaling:EC2_INSTANCE_TERMINATING`.

---

### Task 5: Database & DNS High Availability Deep Dives

**Files:**
- Create: `02-Deep-Dives/RDS-and-Aurora-High-Availability.md`
- Create: `02-Deep-Dives/DynamoDB-Resilience-and-Global-Tables.md`
- Create: `02-Deep-Dives/Route-53-Routing-Policies-and-Failover.md`

- [ ] **Step 1: Author `02-Deep-Dives/RDS-and-Aurora-High-Availability.md`**
  - RDS Multi-AZ (synchronous, standby failover, standby cannot serve reads) vs Read Replicas (asynchronous, read-scaling, cross-region).
  - Aurora 6 copies across 3 AZs (4/6 write, 3/6 read), up to 15 Aurora Replicas, Aurora Global Database (<1s replication, <1min failover).
- [ ] **Step 2: Author `02-Deep-Dives/DynamoDB-Resilience-and-Global-Tables.md`**
  - DynamoDB multi-AZ native design, DynamoDB Streams, Global Tables (active-active multi-region), Point-In-Time Recovery (PITR).
- [ ] **Step 3: Author `02-Deep-Dives/Route-53-Routing-Policies-and-Failover.md`**
  - Alias Record vs CNAME (Zone Apex, cost).
  - 7 Routing Policies: Simple, Weighted, Latency, Failover (Active-Passive with Health Checks), Geolocation, Geoproximity (Traffic Flow + Bias), Multi-Value Answer.

---

### Task 6: Storage Durability & Disaster Recovery Deep Dives

**Files:**
- Create: `02-Deep-Dives/Storage-Durability-S3-EBS-EFS-FSx.md`
- Create: `02-Deep-Dives/Disaster-Recovery-Strategies-RTO-RPO.md`

- [ ] **Step 1: Author `02-Deep-Dives/Storage-Durability-S3-EBS-EFS-FSx.md`**
  - S3 11 9's durability, Cross-Region Replication (CRR), S3 Replication Time Control (S3 RTC - 15 mins).
  - EBS Snapshots (incremental, stored in S3), Fast Snapshot Restore (FSR), EBS Multi-Attach (`io1`/`io2`, single AZ only).
  - EFS Multi-AZ POSIX shared storage.
  - FSx for Windows (Multi-AZ SMB) vs FSx for Lustre (scratch vs persistent).
- [ ] **Step 2: Author `02-Deep-Dives/Disaster-Recovery-Strategies-RTO-RPO.md`**
  - Detailed RTO/RPO definitions and comparison of the 4 tiers: Backup & Restore, Pilot Light, Warm Standby, Multi-Site Active-Active.
  - AWS Elastic Disaster Recovery (AWS DRS) continuous replication.

---

### Task 7: Decision Matrices for Domain 2

**Files:**
- Create: `03-Decision-Matrices/Matrix-ALB-vs-NLB-vs-GWLB.md`
- Create: `03-Decision-Matrices/Matrix-SQS-vs-SNS-vs-EventBridge-vs-Kinesis.md`
- Create: `03-Decision-Matrices/Matrix-RDS-Multi-AZ-vs-Read-Replica.md`
- Create: `03-Decision-Matrices/Matrix-Disaster-Recovery-Tiers.md`
- Create: `03-Decision-Matrices/Matrix-Route-53-Routing-Policies.md`
- Create: `03-Decision-Matrices/Matrix-Storage-Resilience-EBS-vs-EFS-vs-S3.md`

- [ ] **Step 1: Author `Matrix-ALB-vs-NLB-vs-GWLB.md`**
- [ ] **Step 2: Author `Matrix-SQS-vs-SNS-vs-EventBridge-vs-Kinesis.md`**
- [ ] **Step 3: Author `Matrix-RDS-Multi-AZ-vs-Read-Replica.md`**
- [ ] **Step 4: Author `Matrix-Disaster-Recovery-Tiers.md`**
- [ ] **Step 5: Author `Matrix-Route-53-Routing-Policies.md`**
- [ ] **Step 6: Author `Matrix-Storage-Resilience-EBS-vs-EFS-vs-S3.md`**

---

### Task 8: Practice Materials & Vault Verification

**Files:**
- Create: `04-Exam-Scenarios-and-Traps/Domain-2-Scenario-Walkthroughs.md`
- Create: `04-Exam-Scenarios-and-Traps/Domain-2-Keyword-Triggers.md`
- Create: `05-Flashcards-and-Review/Domain-2-Quick-Recall.md`

- [ ] **Step 1: Author `Domain-2-Scenario-Walkthroughs.md`**
  - 10 realistic SAA-C03 exam scenarios covering: SQS visibility timeout/dead letter, ALB path-based with SNI, NLB static IP requirement, ASG target tracking with lifecycle hook, RDS Multi-AZ automatic failover vs Read Replica read scaling, Aurora Global Database disaster recovery, Route 53 latency vs geolocation routing, Pilot Light vs Warm Standby RTO/RPO selection, S3 Cross-Region Replication with RTC, EBS Multi-Attach io2 constraints.
- [ ] **Step 2: Author `Domain-2-Keyword-Triggers.md`**
  - Rapid lookup table: Exam prompt keywords -> Correct Domain 2 architectural solution.
- [ ] **Step 3: Author `Domain-2-Quick-Recall.md`**
  - 30 active-recall flashcard Q&A with `<details><summary>` tags.
- [ ] **Step 4: Audit & Verify All Links**
  - Verify every `[[...]]` wikilink in Domain 2 resolves properly.
