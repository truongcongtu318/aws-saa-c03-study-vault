# SAA-C03 Domain 3 (Design High-Performing Architectures) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a comprehensive, bilingual, Obsidian-compatible study vault and knowledge hub for AWS SAA-C03 Domain 3 (Design High-Performing Architectures) covering all 5 task statements, modern 2026 features (S3 Express One Zone, Aurora Serverless v2, ElastiCache Serverless, Parquet partition pruning, io2 Block Express, EFA), 10 deep dives, 7 decision matrices, 10 exam scenarios, and 30 flashcards.

**Architecture:** Build a 5-tier knowledge vault module featuring Map of Content (MOC), official Task Statements, deep dive technical guides, comparative decision matrices, and exam scenario/flashcard sets with bidirectional wikilinks (`[[...]]`) and Mermaid diagrams.

**Tech Stack:** Markdown (CommonMark + Obsidian Flavored Markdown), YAML Frontmatter, Mermaid.js diagrams.

**Spec:** [docs/superpowers/specs/2026-03-30-saa-c03-domain3-vault-design.md](docs/superpowers/specs/2026-03-30-saa-c03-domain3-vault-design.md)

## Global Constraints

- Root workspace is `D:\xbrain\saa`.
- Every note MUST contain valid YAML frontmatter (`title`, `domain`, `task_statement`, `exam_weight`, `services`, `tags`, `last_updated`).
- Bilingual standard: English for AWS official terms, architecture names, CLI/API parameters, and exam scenarios; Vietnamese for architectural explanations, trade-off analysis, decision rationale, and exam traps.
- All inter-note references must use Obsidian Wikilinks syntax `[[Target-Note-Name]]`.
- Diagrams must use standard Mermaid.js syntax (`graph TD`, `sequenceDiagram`, `flowchart LR`, `mindmap`).

## Review Focus

- Accurate differentiation between S3 Express One Zone (Directory Buckets, single-digit ms latency, single AZ) vs S3 One Zone-IA (infrequent access cost savings, not for high throughput).
- Correct technical specifications for EBS `gp3` (3,000 baseline IOPS & 125 MB/s free) vs `io2 Block Express` (up to 256,000 IOPS and 4,000 MB/s).
- Placement Groups: Cluster (single AZ, low latency/high bandwidth) vs Spread (max 7 instances/AZ, separate racks) vs Partition (Hadoop/Cassandra partitions across racks).
- Athena query tuning: Apache Parquet columnar storage, partition pruning vs "Small Files Trap", Partition Projection.
- Edge performance: CloudFront (CDN caching) vs Global Accelerator (Static Anycast IPs for TCP/UDP without caching); CloudFront Functions (JS, sub-ms) vs Lambda@Edge (Node/Python, 10s runtime, origin request/response).

---

### Task 1: Domain 3 Foundation & Index

**Files:**
- Create: `00-Index/00-Domain-3-MOC.md`
- Modify: `README.md`
- Modify: `00-Index/SAA-C03-Exam-Blueprint.md`

- [ ] **Step 1: Create `00-Index/00-Domain-3-MOC.md`**
  - Author the Map of Content (MOC) with Mermaid mindmap, Dataview-ready table, status indicators (🟢 Completed), and direct links to all tasks, deep dives, matrices, and practice materials for Domain 3.
- [ ] **Step 2: Update `README.md`**
  - Add Domain 3 to the master directory guide and update total exam coverage to 80% (Domain 1 + 2 + 3).
- [ ] **Step 3: Update `00-Index/SAA-C03-Exam-Blueprint.md`**
  - Link directly to `[[00-Domain-3-MOC]]`.

---

### Task 2: Official Task Statements Mapping (Task 3.1 to Task 3.5)

**Files:**
- Create: `01-Task-Statements/Task-3.1-High-Performing-Storage.md`
- Create: `01-Task-Statements/Task-3.2-High-Performing-Compute.md`
- Create: `01-Task-Statements/Task-3.3-High-Performing-Databases.md`
- Create: `01-Task-Statements/Task-3.4-High-Performing-Networking.md`
- Create: `01-Task-Statements/Task-3.5-High-Performing-Data-Ingestion.md`

- [ ] **Step 1: Author `Task-3.1-High-Performing-Storage.md`**
- [ ] **Step 2: Author `Task-3.2-High-Performing-Compute.md`**
- [ ] **Step 3: Author `Task-3.3-High-Performing-Databases.md`**
- [ ] **Step 4: Author `Task-3.4-High-Performing-Networking.md`**
- [ ] **Step 5: Author `Task-3.5-High-Performing-Data-Ingestion.md`**

---

### Task 3: Storage & Compute Performance Deep Dives

**Files:**
- Create: `02-Deep-Dives/S3-Express-One-Zone-and-Storage-Performance.md`
- Create: `02-Deep-Dives/EBS-Volume-Types-gp3-and-io2-Block-Express.md`
- Create: `02-Deep-Dives/EFS-and-FSx-High-Throughput-Modes.md`
- Create: `02-Deep-Dives/EC2-Placement-Groups-and-HPC-Accelerators.md`
- Create: `02-Deep-Dives/Lambda-Performance-and-Provisioned-Concurrency.md`

- [ ] **Step 1: Author `S3-Express-One-Zone-and-Storage-Performance.md`**
- [ ] **Step 2: Author `EBS-Volume-Types-gp3-and-io2-Block-Express.md`**
- [ ] **Step 3: Author `EFS-and-FSx-High-Throughput-Modes.md`**
- [ ] **Step 4: Author `EC2-Placement-Groups-and-HPC-Accelerators.md`**
- [ ] **Step 5: Author `Lambda-Performance-and-Provisioned-Concurrency.md`**

---

### Task 4: Database, Analytics & Network Performance Deep Dives

**Files:**
- Create: `02-Deep-Dives/Caching-Deep-Dive-ElastiCache-and-DAX.md`
- Create: `02-Deep-Dives/Aurora-Serverless-v2-and-RDS-Proxy.md`
- Create: `02-Deep-Dives/Data-Lake-Query-Tuning-Athena-Glue-Parquet.md`
- Create: `02-Deep-Dives/CloudFront-vs-Global-Accelerator-Edge-Performance.md`
- Create: `02-Deep-Dives/Real-Time-Streaming-Kinesis-and-MSK.md`

- [ ] **Step 1: Author `Caching-Deep-Dive-ElastiCache-and-DAX.md`**
- [ ] **Step 2: Author `Aurora-Serverless-v2-and-RDS-Proxy.md`**
- [ ] **Step 3: Author `Data-Lake-Query-Tuning-Athena-Glue-Parquet.md`**
- [ ] **Step 4: Author `CloudFront-vs-Global-Accelerator-Edge-Performance.md`**
- [ ] **Step 5: Author `Real-Time-Streaming-Kinesis-and-MSK.md`**

---

### Task 5: Decision Matrices for Domain 3

**Files:**
- Create: `03-Decision-Matrices/Matrix-EBS-Volume-Types.md`
- Create: `03-Decision-Matrices/Matrix-S3-Performance-Options.md`
- Create: `03-Decision-Matrices/Matrix-EC2-Placement-Groups.md`
- Create: `03-Decision-Matrices/Matrix-ElastiCache-Redis-vs-Memcached.md`
- Create: `03-Decision-Matrices/Matrix-Analytics-Athena-vs-Redshift-vs-OpenSearch.md`
- Create: `03-Decision-Matrices/Matrix-CloudFront-vs-Global-Accelerator.md`
- Create: `03-Decision-Matrices/Matrix-CloudFront-Functions-vs-Lambda-at-Edge.md`

- [ ] **Step 1: Author `Matrix-EBS-Volume-Types.md`**
- [ ] **Step 2: Author `Matrix-S3-Performance-Options.md`**
- [ ] **Step 3: Author `Matrix-EC2-Placement-Groups.md`**
- [ ] **Step 4: Author `Matrix-ElastiCache-Redis-vs-Memcached.md`**
- [ ] **Step 5: Author `Matrix-Analytics-Athena-vs-Redshift-vs-OpenSearch.md`**
- [ ] **Step 6: Author `Matrix-CloudFront-vs-Global-Accelerator.md`**
- [ ] **Step 7: Author `Matrix-CloudFront-Functions-vs-Lambda-at-Edge.md`**

---

### Task 6: Practice Materials & Vault Verification

**Files:**
- Create: `04-Exam-Scenarios-and-Traps/Domain-3-Scenario-Walkthroughs.md`
- Create: `04-Exam-Scenarios-and-Traps/Domain-3-Keyword-Triggers.md`
- Create: `05-Flashcards-and-Review/Domain-3-Quick-Recall.md`

- [ ] **Step 1: Author `Domain-3-Scenario-Walkthroughs.md`** (10 scenarios)
- [ ] **Step 2: Author `Domain-3-Keyword-Triggers.md`** (Rapid lookup table)
- [ ] **Step 3: Author `Domain-3-Quick-Recall.md`** (30 flashcards)
- [ ] **Step 4: Audit & Verify All Links** (Verify every wikilink in Domain 3)
