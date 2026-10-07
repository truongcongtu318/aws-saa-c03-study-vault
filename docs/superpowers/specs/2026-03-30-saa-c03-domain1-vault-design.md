# Spec: AWS SAA-C03 Domain 1 (Design Secure Architectures) Study Vault

- **Date**: 2026-03-30
- **Target**: AWS Certified Solutions Architect – Associate (SAA-C03 / 2026)
- **Primary Focus**: Domain 1: Design Secure Architectures (30% Exam Score Weight)
- **Format**: Obsidian-Compatible Markdown Vault & GitHub-Friendly Knowledge Base
- **Language Policy**: Bilingual (Official AWS English terminology & exam questions; Vietnamese architectural explanations, decision logic, and exam trap analysis)

---

## 1. Executive Summary & Intent

Domain 1 constitutes the highest-weighted domain in the SAA-C03 exam (30%). A strong mastery of security concepts guarantees a solid score buffer for passing the examination. 

This repository is constructed as an **Obsidian-compatible Knowledge Vault** rooted at `D:\xbrain\saa`. It emphasizes:
- Interlinked notes using bi-directional Wikilinks (`[[...]]`).
- Standardized YAML frontmatter for searchability and automated metadata queries (e.g. Dataview plugin).
- Mermaid.js sequence and flowchart diagrams representing multi-tier architectures, token exchange flows, and decision trees.
- Concrete exam traps ("Distractor traps") and keyword-to-solution mappings.

---

## 2. Vault Information Architecture & Directory Hierarchy

```text
D:\xbrain\saa/
├── 00-Index/
│   ├── 00-Domain-1-MOC.md                       # Master Map of Content & Study Progress Tracker
│   └── SAA-C03-Exam-Blueprint.md                # 4-Domain weighting, scoring rules, passing criteria
├── 01-Task-Statements/
│   ├── Task-1.1-Design-Secure-Access-Resources.md # Task 1.1 official exam requirements & service map
│   ├── Task-1.2-Design-Secure-Workloads-Apps.md   # Task 1.2 official exam requirements & service map
│   └── Task-1.3-Determine-Data-Security-Controls.md # Task 1.3 official exam requirements & service map
├── 02-Deep-Dives/
│   ├── IAM-and-STS-Deep-Dive.md                 # Evaluation logic, AssumeRole, Instance Profiles, Cross-Account
│   ├── AWS-Organizations-and-SCPs.md            # Multi-account governance, OU hierarchy, SCP guardrails
│   ├── Directory-Service-and-IAM-Identity-Center.md # SSO, Active Directory integration (AD Connector vs Managed)
│   ├── Amazon-Cognito-Architecture.md           # User Pools vs Identity Pools, Social & SAML federation
│   ├── VPC-Network-Security-Layers.md           # Security Groups vs NACLs, Flow Logs, Endpoints (Gateway vs Interface)
│   ├── Edge-and-App-Defense.md                  # WAF, Shield Standard vs Advanced, Firewall Manager, Network Firewall
│   ├── AWS-KMS-and-CloudHSM.md                  # CMK vs AWS-managed, Key rotation, Key Policies, Envelope Encryption
│   ├── Secrets-Manager-and-Parameter-Store.md   # Secrets rotation vs Parameter Store tiers, caching strategies
│   ├── S3-Security-and-Compliance.md            # Bucket policies, Block Public Access, SSE options, Object Lock (WORM)
│   └── Threat-Detection-and-Auditing.md         # GuardDuty, Inspector, Macie, Security Hub, Detective, CloudTrail
├── 03-Decision-Matrices/
│   ├── Matrix-Secrets-vs-Parameter-Store.md     # Side-by-side comparison table & selection tree
│   ├── Matrix-SG-vs-NACL.md                     # Stateful vs Stateless rules, ephemeral ports, evaluation order
│   ├── Matrix-KMS-Key-Types.md                  # AWS Owned vs Managed vs Customer Managed; Symmetric vs Asymmetric
│   ├── Matrix-WAF-vs-Shield-vs-Firewall.md      # Layer 7 vs Layer 3/4 DDoS vs Stateful VPC Inspection
│   ├── Matrix-Security-Detection-Tools.md       # GuardDuty vs Inspector vs Macie vs Security Hub vs Detective
│   └── Matrix-AD-Integration-Modes.md           # AWS Managed AD vs AD Connector vs Simple AD vs Identity Center
├── 04-Exam-Scenarios-and-Traps/
│   ├── Domain-1-Scenario-Walkthroughs.md        # 10 classic SAA-C03 scenarios with detailed answer & distractor breakdown
│   └── Domain-1-Keyword-Triggers.md             # Direct lookup table: Exam prompt keywords -> Correct architectural answer
└── 05-Flashcards-and-Review/
    └── Domain-1-Quick-Recall.md                 # 30 high-frequency flashcard questions for rapid active recall
```

---

## 3. Note Standards & Frontmatter Specification

Every note in the vault must adhere to a clean schema to ensure compatibility with Obsidian plugins, graph rendering, and plain GitHub viewers.

### Sample YAML Frontmatter
```markdown
---
title: "AWS KMS and Envelope Encryption"
domain: "Domain 1: Design Secure Architectures"
task_statement: "Task 1.3: Determine appropriate data security controls"
exam_weight: "High"
services:
  - AWS KMS
  - AWS CloudHSM
tags:
  - aws
  - saa-c03
  - domain1
  - encryption
  - security
last_updated: 2026-03-30
---
```

### Standardized Note Structure
1. **Frontmatter**: Standard YAML block.
2. **Concept Overview (Khái niệm cốt lõi)**: Concise English definition + Vietnamese architectural intuition.
3. **Architecture & Working Mechanism (Cơ chế hoạt động & Sơ đồ)**: Visual Mermaid flowchart or ASCII layout.
4. **Key Configuration Points (Cấu hình trọng tâm trong đề thi)**: Specific toggles, CLI parameters, and JSON policy blocks tested in SAA.
5. **Exam Tips & Distractor Traps (Bẫy đề thi & Mẹo ghi nhớ)**:
   - What wrong answers look like.
   - Decisive discriminator phrases.
6. **Related Notes (Liên kết hai chiều)**: Wikilinks `[[...]]` to connected topics and matrices.

---

## 4. In-Scope Services & Topic Coverage Matrix (21 Services)

| Service Name | Task Statement | Key Exam Focus |
| :--- | :--- | :--- |
| **IAM** | Task 1.1 | Principle of least privilege, JSON evaluation order, permission boundaries, IAM roles for EC2/Lambda |
| **AWS STS** | Task 1.1 | Temporary credentials (`AssumeRole`, `GetSessionToken`), cross-account access, external ID against confused deputy |
| **AWS Organizations** | Task 1.1 | Multi-account consolidation, Organizational Units (OUs), SCP guardrails (Deny overrides Allow) |
| **IAM Identity Center** | Task 1.1 | Centralized multi-account login, SAML 2.0 / OIDC corporate identity federation, permission sets |
| **Directory Service** | Task 1.1 | AWS Managed Microsoft AD (two-way trust), AD Connector (proxy to on-prem), Simple AD |
| **Amazon Cognito** | Task 1.1 | User Pools (authentication, sign-up/login) vs Identity Pools (authorization, temporary AWS credentials) |
| **AWS RAM** | Task 1.1 | Safe cross-account resource sharing (VPC Subnets, Transit Gateway, Route 53 Resolver rules) without duplication |
| **Security Groups** | Task 1.2 | Stateful firewall, instance/ENI level, allow rules only, referencing other SGs by ID |
| **Network ACLs** | Task 1.2 | Stateless firewall, subnet level, numbered rules (lowest evaluated first), allow and deny, ephemeral ports (1024-65535) |
| **VPC Endpoints** | Task 1.2 | Gateway Endpoints (S3 & DynamoDB, route table update, free) vs Interface Endpoints / PrivateLink (all others, ENI-based, cost per hr/GB) |
| **AWS WAF** | Task 1.2 | Layer 7 filtering (SQLi, XSS, geo-blocking, rate limiting) attached to CloudFront, ALB, API Gateway, AppSync |
| **AWS Shield** | Task 1.2 | Shield Standard (free L3/L4 DDoS) vs Shield Advanced ($3000/mo, 24/7 DRT team, financial protection) |
| **AWS Firewall Manager** | Task 1.2 | Multi-account centralized policy rollout for WAF, Shield, Security Groups, and Network Firewall |
| **AWS Network Firewall** | Task 1.2 | Stateful VPC-level inspection, L3-L7 deep packet filtering, Suricata rules, egress domain filtering |
| **AWS Systems Manager (SSM)** | Task 1.2 | Session Manager (secure terminal access to private EC2 without inbound port 22/3389 or Bastion host) |
| **AWS KMS** | Task 1.3 | AWS Owned vs Managed vs Customer Managed Keys (CMKs), Envelope Encryption, automatic key rotation (1 yr), Multi-region keys |
| **AWS CloudHSM** | Task 1.3 | Dedicated single-tenant FIPS 140-2 Level 3 hardware security module for strict compliance |
| **AWS Secrets Manager** | Task 1.3 | Automatic rotation (built-in Lambda for RDS/Aurora/DocumentDB), KMS encryption, cross-account secret replication |
| **SSM Parameter Store** | Task 1.3 | Hierarchical key-value store, Standard (free, 4KB) vs Advanced (paid, 8KB, policies), no built-in auto rotation |
| **Amazon S3 Security** | Task 1.3 | Block Public Access (4 settings), Bucket Policies, SSE (SSE-S3, SSE-KMS, DSSE-KMS, SSE-C), Object Lock (Governance, Compliance, Legal Hold) |
| **AWS Certificate Manager (ACM)**| Task 1.3 | Public & private SSL/TLS certificates, free auto-renewal with Route 53 DNS validation, data in transit |
| **Amazon GuardDuty** | Task 1.2 / 1.3 | ML-based intelligent threat detection scanning VPC Flow Logs, DNS Logs, CloudTrail, S3 data events without agents |
| **Amazon Inspector** | Task 1.2 | Automated vulnerability assessment (CVE) for EC2 (via SSM Agent), ECR container images, and Lambda functions |
| **Amazon Macie** | Task 1.3 | Discovery and classification of sensitive data & PII in S3 buckets using NLP/ML |
| **AWS Security Hub** | Task 1.2 / 1.3 | Single-pane security posture management, CIS benchmarks, finding aggregation from GuardDuty/Inspector/Macie |
| **Amazon Detective** | Task 1.2 / 1.3 | Root cause graph analysis and visual investigation following GuardDuty / Security Hub findings |
| **AWS Artifact & Audit Manager** | Task 1.3 | Artifact (on-demand SOC/PCI compliance reports & BAA agreements); Audit Manager (automated evidence collection) |

---

## 5. Verification & Acceptance Criteria

1. **Obsidian Compatibility**: All `.md` files contain valid YAML frontmatter, standard Markdown tables, Mermaid charts, and relative `[[wikilinks]]`.
2. **Exhaustive Domain 1 Coverage**: All 21 in-scope security services from the official SAA-C03 exam guide are represented in depth.
3. **Actionable Decision Matrices**: 6 dedicated comparison matrices covering the most confusing exam choices.
4. **Scenario Walkthroughs**: 10 realistic examination scenario analyses showing how to spot distractors and select the optimal architectural answer.
5. **Keyword Triggers & Rapid Recall**: A dedicated lookup table and active-recall flashcard set for pre-exam review.

---

## 6. Execution Transition

Upon approval of this design spec, we proceed directly into the implementation phase using `writing-plans` to outline granular file-by-file generation steps.
