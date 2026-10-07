# SAA-C03 Domain 4 (Design Cost-Optimized Architectures) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a comprehensive, bilingual, Obsidian-compatible study vault and knowledge hub for AWS SAA-C03 Domain 4 (Design Cost-Optimized Architectures) covering all 4 task statements, 2026 FinOps nuances (Aurora I/O-Optimized, S3 Intelligent-Tiering, Compute Savings Plans, NAT vs Gateway Endpoints, Compute Optimizer), 9 deep dives, 6 decision matrices, 10 exam scenarios, and 30 flashcards, completing 100% of the SAA-C03 exam syllabus.

**Architecture:** Build a 5-tier knowledge vault module featuring Map of Content (MOC), official Task Statements, deep dive technical guides, comparative decision matrices, and exam scenario/flashcard sets with bidirectional wikilinks (`[[...]]`) and Mermaid diagrams.

**Tech Stack:** Markdown (CommonMark + Obsidian Flavored Markdown), YAML Frontmatter, Mermaid.js diagrams.

**Spec:** [docs/superpowers/specs/2026-03-30-saa-c03-domain4-vault-design.md](docs/superpowers/specs/2026-03-30-saa-c03-domain4-vault-design.md)

## Global Constraints

- Root workspace is `D:\xbrain\saa`.
- Every note MUST contain valid YAML frontmatter (`title`, `domain`, `task_statement`, `exam_weight`, `services`, `tags`, `last_updated`).
- Bilingual standard: English for AWS official terms, architecture names, CLI/API parameters, and exam scenarios; Vietnamese for architectural explanations, trade-off analysis, decision rationale, and exam traps.
- All inter-note references must use Obsidian Wikilinks syntax `[[Target-Note-Name]]`.
- Diagrams must use standard Mermaid.js syntax (`graph TD`, `sequenceDiagram`, `flowchart LR`, `mindmap`).

## Review Focus

- Correct calculations and tipping points: Aurora I/O-Optimized is cost-effective when I/O costs exceed 25%-33% of total database spend.
- Data transfer costs: NAT Gateway ($0.045/hr + $0.045/GB) vs VPC Gateway Endpoints ($0/hr + $0/GB) for S3 and DynamoDB.
- Storage billing constraints: S3 Standard-IA (30-day min duration, 128 KB min size), Glacier Instant/Flexible (90-day min), Glacier Deep Archive (180-day min).
- Compute pricing distinctions: Compute Savings Plans (flexible across EC2/Fargate/Lambda) vs EC2 Instance Savings Plans (single family/region) vs Spot Instances (up to 90% off, 2-minute interruption notice).
- FinOps governance: AWS Budgets with action targets (SNS, SCP, Stop EC2) vs AWS Cost Anomaly Detection vs AWS Cost Explorer.

---

### Task 1: Domain 4 Foundation & 100% Exam Blueprint Completion

**Files:**
- Create: `00-Index/00-Domain-4-MOC.md`
- Modify: `README.md`
- Modify: `00-Index/SAA-C03-Exam-Blueprint.md`

- [ ] **Step 1: Create `00-Index/00-Domain-4-MOC.md`**
  - Author the Map of Content (MOC) with Mermaid mindmap, Dataview-ready table, status indicators (🟢 Completed), and direct links to all tasks, deep dives, matrices, and practice materials for Domain 4.
- [ ] **Step 2: Update `README.md`**
  - Update master README to announce 100% complete coverage across all 4 Domains (100% of SAA-C03 blueprint).
- [ ] **Step 3: Update `00-Index/SAA-C03-Exam-Blueprint.md`**
  - Link directly to `[[00-Domain-4-MOC]]`.

---

### Task 2: Official Task Statements Mapping (Task 4.1 to Task 4.4)

**Files:**
- Create: `01-Task-Statements/Task-4.1-Cost-Optimized-Storage.md`
- Create: `01-Task-Statements/Task-4.2-Cost-Optimized-Compute.md`
- Create: `01-Task-Statements/Task-4.3-Cost-Optimized-Databases.md`
- Create: `01-Task-Statements/Task-4.4-Cost-Optimized-Networking.md`

- [ ] **Step 1: Author `Task-4.1-Cost-Optimized-Storage.md`**
- [ ] **Step 2: Author `Task-4.2-Cost-Optimized-Compute.md`**
- [ ] **Step 3: Author `Task-4.3-Cost-Optimized-Databases.md`**
- [ ] **Step 4: Author `Task-4.4-Cost-Optimized-Networking.md`**

---

### Task 3: Storage & Compute Cost Optimization Deep Dives

**Files:**
- Create: `02-Deep-Dives/S3-Lifecycle-and-Storage-Classes-Cost-Tuning.md`
- Create: `02-Deep-Dives/EC2-Pricing-Models-Spot-Savings-Plans-and-RIs.md`
- Create: `02-Deep-Dives/EFS-and-EBS-Storage-Cost-Optimization.md`
- Create: `02-Deep-Dives/AWS-Compute-Optimizer-and-Right-Sizing.md`

- [ ] **Step 1: Author `S3-Lifecycle-and-Storage-Classes-Cost-Tuning.md`**
- [ ] **Step 2: Author `EC2-Pricing-Models-Spot-Savings-Plans-and-RIs.md`**
- [ ] **Step 3: Author `EFS-and-EBS-Storage-Cost-Optimization.md`**
- [ ] **Step 4: Author `AWS-Compute-Optimizer-and-Right-Sizing.md`**

---

### Task 4: Database, Network & FinOps Governance Deep Dives

**Files:**
- Create: `02-Deep-Dives/Aurora-Storage-Configurations-Standard-vs-IO-Optimized.md`
- Create: `02-Deep-Dives/Data-Transfer-Costs-and-VPC-Endpoints-vs-NAT.md`
- Create: `02-Deep-Dives/DynamoDB-Cost-Models-On-Demand-vs-Provisioned.md`
- Create: `02-Deep-Dives/CloudFront-Cost-Optimization-and-Price-Classes.md`
- Create: `02-Deep-Dives/AWS-Cost-Governance-Budgets-Anomaly-and-Tags.md`

- [ ] **Step 1: Author `Aurora-Storage-Configurations-Standard-vs-IO-Optimized.md`**
- [ ] **Step 2: Author `Data-Transfer-Costs-and-VPC-Endpoints-vs-NAT.md`**
- [ ] **Step 3: Author `DynamoDB-Cost-Models-On-Demand-vs-Provisioned.md`**
- [ ] **Step 4: Author `CloudFront-Cost-Optimization-and-Price-Classes.md`**
- [ ] **Step 5: Author `AWS-Cost-Governance-Budgets-Anomaly-and-Tags.md`**

---

### Task 5: Decision Matrices for Domain 4

**Files:**
- Create: `03-Decision-Matrices/Matrix-EC2-Pricing-Models.md`
- Create: `03-Decision-Matrices/Matrix-S3-Storage-Classes-Cost-and-Retrieval.md`
- Create: `03-Decision-Matrices/Matrix-Aurora-Standard-vs-IO-Optimized.md`
- Create: `03-Decision-Matrices/Matrix-NAT-Gateway-vs-VPC-Endpoints.md`
- Create: `03-Decision-Matrices/Matrix-DynamoDB-On-Demand-vs-Provisioned.md`
- Create: `03-Decision-Matrices/Matrix-AWS-Cost-Management-Tools.md`

- [ ] **Step 1: Author `Matrix-EC2-Pricing-Models.md`**
- [ ] **Step 2: Author `Matrix-S3-Storage-Classes-Cost-and-Retrieval.md`**
- [ ] **Step 3: Author `Matrix-Aurora-Standard-vs-IO-Optimized.md`**
- [ ] **Step 4: Author `Matrix-NAT-Gateway-vs-VPC-Endpoints.md`**
- [ ] **Step 5: Author `Matrix-DynamoDB-On-Demand-vs-Provisioned.md`**
- [ ] **Step 6: Author `Matrix-AWS-Cost-Management-Tools.md`**

---

### Task 6: Practice Materials & 100% Vault Master Verification

**Files:**
- Create: `04-Exam-Scenarios-and-Traps/Domain-4-Scenario-Walkthroughs.md`
- Create: `04-Exam-Scenarios-and-Traps/Domain-4-Keyword-Triggers.md`
- Create: `05-Flashcards-and-Review/Domain-4-Quick-Recall.md`

- [ ] **Step 1: Author `Domain-4-Scenario-Walkthroughs.md`** (10 scenarios)
- [ ] **Step 2: Author `Domain-4-Keyword-Triggers.md`** (Rapid lookup table)
- [ ] **Step 3: Author `Domain-4-Quick-Recall.md`** (30 flashcards)
- [ ] **Step 4: Audit & Verify All Links Across All 4 Domains**
