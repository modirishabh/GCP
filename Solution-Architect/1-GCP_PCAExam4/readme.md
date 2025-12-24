# Google Cloud Professional Architect Exam Questions 61-72

## Overview
Google Cloud certification questions testing disaster recovery, IAM hierarchy, security, networking, IaC, monitoring, and automation. Each includes core concepts, why answers are correct/incorrect, and GCP best practices. [web:2][web:27]

## Question 61: GCE Disaster Recovery Snapshots
**Core Concept**: GCE persistent disk snapshots are global resources, instantly available across regions (e.g., us-central1 to europe-west1) without cross-region copy delays. Enables low RPO/RTO by creating DR disks immediately. Persistent disks are regional only. [web:2]

**Best Answer**: **A** - Snapshots are global resources in GCE. Available across all regions by default.
- **Why Wrong**:
  - C: Persistent disks regional, can't mount cross-region [web:5]
  - B: No DRaaS snapshot feature
  - D: No console scheduler; copy time still issue

## Question 62: IAM Policy Management
**Core Concept**: Single organization with department folders enables central governance + inherited IAM policies per folder. Projects inherit folder policies scalably. [web:6]

**Best Answer**: **C** - Single organization with folders for each department.
- **Why Wrong**:
  - A/B: Multiple orgs fragment central control
  - D: Project-level IAM unscalable for departments

## Question 63: App Engine Java Deployment Error
**Core Concept**: `SHA1 digest error` from invalid/missing JAR digital signatures. App Engine verifies JAR integrity; fix by signing all JARs with valid certs. [web:7]

**Best Answer**: **B** - Digitally sign all JAR files and redeploy.
- **Why Wrong**:
  - A: Error not about missing files
  - C: MD5 downgrade; issue is signature, not hash

## Question 64: Cloud Storage Performance & Security
**Core Concepts**: 
- Cloud CDN caches files at global edges for low-latency downloads.
- Signed URLs grant time-limited access post-form submission. [web:16]

**Best Answers**: **C + D** - Signed URLs + Cloud CDN.
- **Why Wrong**: A/B - Multi-regional for durability, not end-user latency.

## Question 65: Chat App Message Authentication
**Core Concept**: PKI digital signatures: sender signs with private key (client-side); receiver verifies with public key. Proves origin + integrity, prevents spoofing. [web:9]

**Best Answer**: **C** - PKI to encrypt message client-side using originating user's private key (digital signing).
- **Why Wrong**:
  - A: Client tags fakeable
  - B: Shared key no authenticity
  - D: SSL secures transit only

## Question 66: Hybrid MySQL Replication
**Core Concept**: Cloud VPN (public internet) unreliable for DB replication due to latency/loss. Dedicated Interconnect provides private, low-latency, high-bandwidth link. [web:1 ]

**Best Answer**: **B** - Configure Google Cloud Dedicated Interconnect.
- **Why Wrong**:
  - A: UDP unreliable for DB
  - C: Daily restore ≠ replication
  - D: More VPNs still public internet
  - E: Pub/Sub not DB replication

## Question 67: Infrastructure Automation
**Core Concept**: Cloud Deployment Manager (IaC) deploys repeatable resource stacks (VMs, networks) via YAML templates. Native GCP, ideal for limited staff. [web:45]

**Best Answer**: **B** - Cloud Deployment Manager.
- **Why Wrong**:
  - A: Forseti = security scanning
  - C: Manual creation unscalable
  - D: Asset history = audit only

## Question 68: PII(personally identifiable information or payment card information) Sanitization for Bigtable
**Core Concept**: **The cloud data loss prevention API or DLP** is the purpose-built service for this exact problem. 
First, its powerful detectors can automatically find over 150 different types of sensitive information. 
Second, after it finds the data, it can deidentify it using various techniques like redaction or tokenization. Using the DLP API
provides a scalable, reliable, and automated way to ensure sensitive data is sanitized before it's stored for analysiS

**Best Answer**: **C** - Identify data with Cloud DLP API.
- **Why Wrong**:
  - A: Hashing destroys analysis value
  - B: Encryption ≠ sanitization
  - D: Regex unreliable globally

## Question 69: Cloud Shell Persistence
**Core Concept**: `$HOME` (5GB persistent disk) includes `~/bin` in PATH. Store executables there for cross-session availability. [web:21][web:23]

**Best Answer**: **A** - `/bin` (~/bin in persistent $HOME).
- **Why Wrong**:
  - B: Cloud Storage not in PATH
  - C/D: Non-persistent locations

## Question 7 : Secure Remote Desktop VDI
**Core Concept**: Google Cloud Virtual Desktop Infrastructure (VDI) provides managed, scalable, encrypted desktop streaming for WFH. [web:33]

**Best Answer**: **B** - Google Cloud Virtual Desktop.
- **Why Wrong**:
  - A: Workspace = productivity apps
  - C: VPN tunnel only
  - D: RDP protocol lacks management

## Question 71: High-Bandwidth Hybrid Connectivity
**Core Concept**: Dedicated Interconnect bundles 1 Gbps+ private links via VPC for 2 +Gbps private connectivity. [web:18]

**Best Answer**: **A** - VPC + Dedicated Interconnect.
- **Why Wrong**:
  - B: Single VPN <2 Gbps guaranteed
  - C/D: CDN for public web content

## Question 72: Compute Engine CPU Monitoring
**Core Concept**: Cloud Monitoring metric-threshold policies alert on CPU >9 % (e.g., 5min duration). Automated, native metrics collection. [web:27][web:53]

**Best Answer**: **B** - Alerting policy based on metric rate in Cloud Monitoring.
- **Why Wrong**:
  - A: Process health ≠ VM CPU
  - C: Uptime checks external only
  - D: Custom script = poor practice

## Key Takeaways
- **Global vs Regional**: Snapshots global; disks regional [web:2]
- **Hierarchy**: Org → Folders → Projects for IAM [web:6]
- **Security**: DLP for PII, PKI for authenticity, signed URLs [web:46]
- **Networking**: Interconnect > VPN for performance [web:18]
- **Automation**: Deployment Manager for GCP IaC [web:45]
- **Monitoring**: Metric policies over custom scripts [web:27]


# Question 73: GCP Cost Optimization for Startups

**Core Concepts:**
- **Sustained Use Discounts (SUDs)**: Automatic discounts for VMs running >25% of billing month; no commitment needed - perfect for unpredictable startup demand [web:1][web:2]
- **Committed Use Discounts (CUDs)**: Require 1-3 year resource commitments - unsuitable for trials
- **Cost Management**: Team training > dedicated staff for startups (builds cost awareness culture)

**Why B is Best:** Utilize free tier and sustained use discounts. Provide training to the team about service cost management.
- Combines flexible pricing + scalable management [web:2]

## Question 74: CI/CD Pipeline Verification

**Core Concepts:**
- **Pre-Production Testing**: Staging environment verification mandatory before production
- **Jenkins**: CI tool excels at git monitoring/tags → staging builds [web:22]
- **Spinnaker**: CD tool for deployment strategies (red/black/canary) but skips verification here

**Why D is Best:**
Use Jenkins to monitor tags → Deploy staging tags → Test → Production tag → Deploy

 
- Complete safe CI/CD workflow [web:22]

## Question 75: BigQuery Access Control

**Core Concepts:**
- **Granular IAM**: Table-level roles for country-specific access
- **roles/bigquery.dataViewer**: Read-only data access (correct) [web:7]
- **Avoid**: metadataViewer (schema only), dataEditor/Owner (modifies data) [web:15]

**Why B is Best:**
Single dataset + roles/bigquery.dataViewer at table level per partner

 
- Secure, read-only, granular access [web:7]

## Question 76: MIG Troubleshooting Access

**Core Concepts:**
- **MIG Auto-Healing**: Health check failures → constant restarts (even without autoscaling) [web:28]
- **Fix**: Disable health check → Add project-wide SSH key
- **Avoid**: Viewer role (read-only), rolling restart (ignores root cause)

**Why C is Best:**
Disable health check + Add SSH key to project metadata

 
- Stops restart loop, enables investigation [web:28]

## Question 77: PCI DSS Compliance

**Core Concepts:**
- **Shared Responsibility**: GCP secures infrastructure, customer configures services [web:9]
- **GKE**: Provides PCI tools/blueprints (not auto-compliant or "shared hosting") [web:17]
- **False**: App Engine only, all services auto-compliant

**Why C is Best:**
GKE/GCP provide tools to build PCI DSS compliant environment

 
- Reflects shared responsibility model [web:9]

## Question 78: Container Security Enforcement

**Core Concepts:**
- **Detection**: Vulnerability scanning (Container Analysis)
- **Enforcement**: Binary Authorization policies block insecure images pre-GKE [web:1 ]
- **Complete Solution**: Scanning + Binary Auth (detection + gatekeeping) [web:18]

**Why D is Best:**
Vulnerability scanning with binary authorization

 
- End-to-end security pipeline [web:1 ]

## Question 79: Data Cleaning Practices

**Core Concepts:**
- **Data Landing**: Stage on-premises data in Cloud Storage first
- **Cloud Dataprep**: Visual/ML-driven cleaning for degraded data [web:11]
- **DataLab/Notebooks**: Code-based analysis, not visual prep [web:19]

**Why B is Best:**
Upload to Cloud Storage → Use Cloud Dataprep

 
- Google-recommended data preparation workflow [web:11]

## Question 8 : IAM Policy Inheritance

**Core Concepts:**
- **Hierarchical IAM**: Organization → Folder → Project (additive inheritance) [web:12]
- **Effective Policy**: Union of direct + ancestor policies (no denies) [web:2 ]
- **Avoid**: Intersection/restriction (GCP doesn't work this way)

**Why C is Best:**
Effective policy = union of node policy + inherited ancestor policies

 
- Core GCP IAM principle [web:12]

## Quick Reference Table

| Question | Core Principle | Best Answer Letter |
|----------|----------------|-------------------|
| 73 | Flexible pricing + team training | **B** |
| 74 | Staging verification before prod | **D** |
| 75 | Table-level dataViewer role | **B** |
| 76 | Disable health check + SSH | **C** |
| 77 | Shared responsibility model | **C** |
| 78 | Scanning + Binary Auth | **D** |
| 79 | Storage → Dataprep workflow | **B** |
| 80  | IAM policy union inheritance | **C** |

**Study Tip**: Focus on **why wrong answers fail core requirements** - this pattern appears in 9 % of GCP exam questions.
**Study Tip**: Focus on GCP-native services vs generic approaches. Always verify resource scoping (global/regional/zonal).
