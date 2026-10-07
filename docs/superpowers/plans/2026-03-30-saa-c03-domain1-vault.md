# SAA-C03 Domain 1 (Design Secure Architectures) Vault Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a comprehensive, bilingual, Obsidian-compatible study vault and knowledge hub for AWS SAA-C03 Domain 1 (Design Secure Architectures) covering all 21 in-scope security services, decision matrices, scenarios, and flashcards.

**Architecture:** Build a 5-tier knowledge vault featuring Maps of Content (MOC), official Task Statements, deep dive technical guides, comparative decision matrices, and exam scenario/flashcard sets with bidirectional wikilinks (`[[...]]`) and Mermaid diagrams.

**Tech Stack:** Markdown (CommonMark + Obsidian Flavored Markdown), YAML Frontmatter, Mermaid.js diagrams.

**Spec:** [docs/superpowers/specs/2026-03-30-saa-c03-domain1-vault-design.md](docs/superpowers/specs/2026-03-30-saa-c03-domain1-vault-design.md)

## Global Constraints

- Root workspace is `D:\xbrain\saa`.
- Every note MUST contain valid YAML frontmatter (`title`, `domain`, `task_statement`, `exam_weight`, `services`, `tags`, `last_updated`).
- Bilingual standard: English for AWS official terms, architecture names, CLI/API parameters, and exam scenarios; Vietnamese for architectural explanations, trade-off analysis, decision rationale, and exam traps.
- All inter-note references must use Obsidian Wikilinks syntax `[[Target-Note-Name]]`.
- Diagrams must use standard Mermaid.js syntax (`graph TD`, `sequenceDiagram`, `flowchart LR`).

## Review Focus

- Broken or dangling Wikilinks across notes.
- Inconsistent YAML frontmatter field naming.
- Omission of any of the 21 in-scope services.
- Distractor traps lack concrete explanation of *why* they are wrong.
- Ephemeral port ranges, stateful vs stateless mechanics accurately stated for VPC security.

---

### Task 1: Vault Foundation, Index & Exam Blueprint

**Files:**
- Create: `README.md`
- Create: `00-Index/00-Domain-1-MOC.md`
- Create: `00-Index/SAA-C03-Exam-Blueprint.md`

**Interfaces:**
- Consumes: Spec folder hierarchy and exam domain weights.
- Produces: Master index, progress tracker table, and navigation links referenced by all sub-notes.

- [ ] **Step 1: Create `README.md`**
  - Document the purpose of the vault, how to open in Obsidian or view on GitHub, domain weight breakdown, and folder guide.
- [ ] **Step 2: Create `00-Index/00-Domain-1-MOC.md`**
  - Construct the Map of Content (MOC) with Dataview-ready table, status indicators (🔴 Not Started, 🟡 In Progress, 🟢 Mastered), and direct links to all tasks, deep dives, and matrices.
- [ ] **Step 3: Create `00-Index/SAA-C03-Exam-Blueprint.md`**
  - Document the SAA-C03 exam structure: 65 questions (15 unscored), 130 minutes, 720/1000 passing score, domain percentage weights (Domain 1: 30%, Domain 2: 26%, Domain 3: 24%, Domain 4: 20%), scoring strategy and time allocation tactics.
- [ ] **Step 4: Verify links and formatting**
  - Ensure all paths and links resolve correctly.

---

### Task 2: Official Task Statements Mapping

**Files:**
- Create: `01-Task-Statements/Task-1.1-Design-Secure-Access-Resources.md`
- Create: `01-Task-Statements/Task-1.2-Design-Secure-Workloads-Apps.md`
- Create: `01-Task-Statements/Task-1.3-Determine-Data-Security-Controls.md`

**Interfaces:**
- Consumes: Official SAA-C03 Examination Guide Task Statements.
- Produces: High-level task statement breakdown notes linking to corresponding Deep Dives and Decision Matrices.

- [ ] **Step 1: Author `Task-1.1-Design-Secure-Access-Resources.md`**
  - Cover authentication, authorization, least privilege, identity federation, multi-account governance, and temporary credentials.
  - Link to IAM, STS, Organizations, Directory Service, Cognito, and RAM.
- [ ] **Step 2: Author `Task-1.2-Design-Secure-Workloads-Apps.md`**
  - Cover network boundaries, public/private subnets, VPC firewalls, edge protection, threat detection, and patch management.
  - Link to SG/NACL, WAF, Shield, Network Firewall, Systems Manager, GuardDuty, and Inspector.
- [ ] **Step 3: Author `Task-1.3-Determine-Data-Security-Controls.md`**
  - Cover data at rest and in transit, KMS key lifecycle, S3 compliance/WORM, Secrets management, and sensitive data discovery.
  - Link to KMS, CloudHSM, Secrets Manager, Parameter Store, S3 Security, ACM, Macie, and Audit Manager.
- [ ] **Step 4: Verify cross-links**
  - Confirm all 3 task files link back to `[[00-Domain-1-MOC]]`.

---

### Task 3: Identity & Multi-Account Deep Dives (Task 1.1)

**Files:**
- Create: `02-Deep-Dives/IAM-and-STS-Deep-Dive.md`
- Create: `02-Deep-Dives/AWS-Organizations-and-SCPs.md`
- Create: `02-Deep-Dives/Directory-Service-and-IAM-Identity-Center.md`
- Create: `02-Deep-Dives/Amazon-Cognito-Architecture.md`

**Interfaces:**
- Consumes: Task 1.1 requirements.
- Produces: Detailed architectural notes for IAM, STS, Organizations, SCP, Directory Service, Identity Center, and Cognito.

- [ ] **Step 1: Author `02-Deep-Dives/IAM-and-STS-Deep-Dive.md`**
  - Explicit Deny > Allow > Implicit Deny policy evaluation flowchart (Mermaid).
  - IAM Roles vs Instance Profiles, STS `AssumeRole`, External ID pattern (confused deputy defense), Permission Boundaries vs SCPs.
- [ ] **Step 2: Author `02-Deep-Dives/AWS-Organizations-and-SCPs.md`**
  - Root, OUs, member accounts hierarchy, SCP inheritance rules, SCP cannot grant permissions (only boundary filter).
  - AWS RAM (Resource Access Manager) cross-account sharing (VPC Subnets, Transit Gateway).
- [ ] **Step 3: Author `02-Deep-Dives/Directory-Service-and-IAM-Identity-Center.md`**
  - AWS Managed Microsoft AD vs AD Connector vs Simple AD.
  - IAM Identity Center (SSO) multi-account access, SAML 2.0 / OIDC federation, Permission Sets.
- [ ] **Step 4: Author `02-Deep-Dives/Amazon-Cognito-Architecture.md`**
  - Cognito User Pools (Authentication, JWT tokens) vs Cognito Identity Pools (Federated Identities, temporary AWS credentials).
  - Web Identity Federation with Google/Apple/Facebook/SAML.

---

### Task 4: Workload & Network Security Deep Dives (Task 1.2)

**Files:**
- Create: `02-Deep-Dives/VPC-Network-Security-Layers.md`
- Create: `02-Deep-Dives/Edge-and-App-Defense.md`
- Create: `02-Deep-Dives/Threat-Detection-and-Auditing.md`

**Interfaces:**
- Consumes: Task 1.2 requirements.
- Produces: Technical guides for VPC Security, Edge Defense, and Threat Monitoring services.

- [ ] **Step 1: Author `02-Deep-Dives/VPC-Network-Security-Layers.md`**
  - Security Groups (stateful, ENI level, security group reference as source) vs NACLs (stateless, subnet level, numbered rule order, ephemeral ports).
  - VPC Endpoints: Gateway Endpoints (S3, DynamoDB, route table routing, free) vs Interface Endpoints / AWS PrivateLink (ENI, DNS, hourly/data charges).
  - AWS Systems Manager Session Manager (access EC2 in private subnet without port 22/3389, no bastion host).
- [ ] **Step 2: Author `02-Deep-Dives/Edge-and-App-Defense.md`**
  - AWS WAF (Layer 7 web exploits, SQLi, XSS, rate-based rules, IP sets on CloudFront/ALB/API Gateway).
  - AWS Shield Standard vs Shield Advanced (L3/L4 DDoS, DRT support, cost spike protection).
  - AWS Network Firewall (VPC inspection, stateful Suricata rules, outbound domain filtering) vs AWS Firewall Manager (centralized multi-account enforcement).
- [ ] **Step 3: Author `02-Deep-Dives/Threat-Detection-and-Auditing.md`**
  - Amazon GuardDuty (agentless ML detection on VPC Flow Logs, DNS Logs, CloudTrail, S3 data events, EKS).
  - Amazon Inspector (vulnerability CVE scanning on EC2 with SSM agent, ECR images, Lambda).
  - AWS Security Hub (centralized posture, CIS benchmarks, aggregation) vs Amazon Detective (graph-based root cause analysis).
  - CloudTrail (management vs data events, multi-region trails, log file integrity validation).

---

### Task 5: Data Security & Cryptography Deep Dives (Task 1.3)

**Files:**
- Create: `02-Deep-Dives/AWS-KMS-and-CloudHSM.md`
- Create: `02-Deep-Dives/Secrets-Manager-and-Parameter-Store.md`
- Create: `02-Deep-Dives/S3-Security-and-Compliance.md`

**Interfaces:**
- Consumes: Task 1.3 requirements.
- Produces: Detailed notes for encryption, key management, secrets rotation, and S3 compliance controls.

- [ ] **Step 1: Author `02-Deep-Dives/AWS-KMS-and-CloudHSM.md`**
  - KMS Key Types: AWS Owned Keys, AWS Managed Keys, Customer Managed Keys (CMKs).
  - Symmetric vs Asymmetric Keys, Multi-Region Keys.
  - Envelope Encryption mechanics (Customer Master Key -> Data Key plaintext/ciphertext).
  - KMS Key Policies (Default policy giving root access vs scoped key policy).
  - AWS CloudHSM: dedicated FIPS 140-2 Level 3 hardware module.
- [ ] **Step 2: Author `02-Deep-Dives/Secrets-Manager-and-Parameter-Store.md`**
  - Secrets Manager: Automatic rotation (built-in Lambda for RDS/Aurora), KMS encryption, multi-region replication.
  - SSM Parameter Store: Standard (free, 4KB) vs Advanced (paid, 8KB, notifications), String/StringList/SecureString.
  - When to pick Secrets Manager vs Parameter Store.
- [ ] **Step 3: Author `02-Deep-Dives/S3-Security-and-Compliance.md`**
  - S3 Block Public Access (4 settings).
  - Server-side encryption: SSE-S3, SSE-KMS, DSSE-KMS, SSE-C.
  - S3 Object Lock: WORM model, Governance Mode (bypassable with permission) vs Compliance Mode (unbreakable), Legal Hold.
  - AWS Certificate Manager (ACM): SSL/TLS public vs private, DNS validation auto-renewal.
  - Amazon Macie: Sensitive data & PII discovery in S3.
  - AWS Artifact & AWS Audit Manager.

---

### Task 6: Decision Matrices & Selection Trees

**Files:**
- Create: `03-Decision-Matrices/Matrix-Secrets-vs-Parameter-Store.md`
- Create: `03-Decision-Matrices/Matrix-SG-vs-NACL.md`
- Create: `03-Decision-Matrices/Matrix-KMS-Key-Types.md`
- Create: `03-Decision-Matrices/Matrix-WAF-vs-Shield-vs-Firewall.md`
- Create: `03-Decision-Matrices/Matrix-Security-Detection-Tools.md`
- Create: `03-Decision-Matrices/Matrix-AD-Integration-Modes.md`

**Interfaces:**
- Consumes: Deep dive notes.
- Produces: Comparative summary tables and Mermaid flowcharts for instant decision-making in the exam.

- [ ] **Step 1: Author `Matrix-Secrets-vs-Parameter-Store.md`**
  - Comparison table (rotation, price, key features) + Mermaid flowchart: "Do you need automatic rotation? -> Secrets Manager".
- [ ] **Step 2: Author `Matrix-SG-vs-NACL.md`**
  - Comparison table (stateful, level, allow/deny, rule numbering, return traffic, ephemeral ports) + Mermaid flowchart.
- [ ] **Step 3: Author `Matrix-KMS-Key-Types.md`**
  - AWS Owned vs AWS Managed vs Customer Managed vs CloudHSM vs External Key Store.
- [ ] **Step 4: Author `Matrix-WAF-vs-Shield-vs-Firewall.md`**
  - WAF (L7) vs Shield Standard (L3/4 free) vs Shield Advanced (L3/4 paid) vs Network Firewall (VPC inspection) vs Firewall Manager (governance).
- [ ] **Step 5: Author `Matrix-Security-Detection-Tools.md`**
  - GuardDuty (Threat) vs Inspector (Vulnerability CVE) vs Macie (PII in S3) vs Security Hub (Aggregator/CIS) vs Detective (Root cause).
- [ ] **Step 6: Author `Matrix-AD-Integration-Modes.md`**
  - AWS Managed Microsoft AD vs AD Connector vs Simple AD vs IAM Identity Center.

---

### Task 7: Exam Scenarios, Keyword Triggers & Flashcards

**Files:**
- Create: `04-Exam-Scenarios-and-Traps/Domain-1-Scenario-Walkthroughs.md`
- Create: `04-Exam-Scenarios-and-Traps/Domain-1-Keyword-Triggers.md`
- Create: `05-Flashcards-and-Review/Domain-1-Quick-Recall.md`

**Interfaces:**
- Consumes: Complete Domain 1 syllabus.
- Produces: Exam-ready scenario drills, rapid keyword lookup table, and active-recall test cards.

- [ ] **Step 1: Author `Domain-1-Scenario-Walkthroughs.md`**
  - 10 real-world exam scenario questions covering:
    1. EC2 accessing S3 without hardcoded keys (IAM Role Instance Profile).
    2. Multi-account database password rotation (Secrets Manager).
    3. Blocking specific malicious IP addresses at subnet level (NACL Deny rule).
    4. Guarding web application against SQL injection and cross-site scripting (AWS WAF).
    5. Enforcing company-wide rule that no S3 bucket can be created without encryption (SCP).
    6. Compliance requiring WORM model where even root user cannot delete objects (S3 Object Lock Compliance Mode).
    7. Detecting credit card numbers inadvertently uploaded to S3 (Amazon Macie).
    8. Connecting to EC2 in private subnet without bastion host or open port 22 (SSM Session Manager).
    9. Cross-account access with third-party SaaS avoiding confused deputy (AssumeRole + ExternalId).
    10. FIPS 140-2 Level 3 dedicated hardware regulatory requirement (CloudHSM).
  - Each scenario includes: Prompt, Question, Options A/B/C/D, Correct Answer, Why it's correct, and Distractor Traps analysis.
- [ ] **Step 2: Author `Domain-1-Keyword-Triggers.md`**
  - Rapid lookup table: Exam Trigger Keyword -> Correct AWS Service / Architectural Solution.
- [ ] **Step 3: Author `Domain-1-Quick-Recall.md`**
  - 30 active-recall flashcard Q&A with collapsible `<details><summary>` tags for self-testing.

---

### Task 8: Verification & Cross-Linking Audit

**Files:**
- Audit all files in `00-Index`, `01-Task-Statements`, `02-Deep-Dives`, `03-Decision-Matrices`, `04-Exam-Scenarios-and-Traps`, `05-Flashcards-and-Review`.

- [ ] **Step 1: Check all Wikilinks**
  - Confirm every `[[Note-Name]]` points to an actual existing file in the vault.
- [ ] **Step 2: Validate YAML Frontmatter**
  - Ensure all files have valid YAML headers with domain, tags, and service names.
- [ ] **Step 3: Check Mermaid syntax**
  - Verify diagrams parse cleanly.
- [ ] **Step 4: Update Master MOC Status**
  - Mark all sections in `00-Index/00-Domain-1-MOC.md` as 🟢 Completed.
