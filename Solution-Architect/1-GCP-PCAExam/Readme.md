# Google Cloud Professional Cloud Architect Exam Practice Questions

**Reviewed 20 sources**

---

## Overview
These 20 practice questions from a Google Cloud Professional Cloud Architect exam preparation resource test core concepts such as autoscaling, logging, Linux tuning, App Engine state management, hybrid modernization, migration practices, security workflows, data storage, and load balancing. Each question includes the problem, options, correct answer, and key GCP/Linux concepts with explanations.

---

## Question 1: MIG Autoscaling for CPU Crashes

**Problem:** Web app crashes from high CPU; network admin suggests using a Managed Instance Group (MIG). What is the best configuration?

**Options:**
- A. Autoscaling on load balancing capacity  
- B. Cluster autoscaler with fixed min/max  
- C. Autohealing policy  
- D. Autoscaling on CPU utilization  

**Answer:** D  

**Core Concepts:**  
MIG autoscaling proactively adds instances based on metrics like CPU (target e.g. 60%) to distribute load before crashes. Autohealing reacts post-failure, and cluster autoscaler is specific to GKE, not standard MIGs.

---

## Question 2: Logging Tool Needs

**Problem:** Team needs improved logging for errors and historical analysis. What's the best action?

**Options:**
- A. Install Stackdriver agent  
- B. Send logging best practices documentation  
- C. Define requirements, assess tools  
- D. Upgrade current tool  

**Answer:** C  

**Core Concepts:**  
Architects should first clarify requirements (retention, alerting, budget) before recommending tools like **Cloud Logging**. Premature tooling without needs assessment is discouraged.

---

## Question 3: Linux Send Buffer Limit

**Problem:** App hogs bandwidth via TCP window sizing; need to set max OS send buffer globally for all connections.

**Options:**
- A. `net.core.rmem_max`  
- B. `net.core.wmem_max`  
- C. `net.ipv4.tcp_rmem`  
- D. `net.ipv4.tcp_wmem`  

**Answer:** B  

**Core Concepts:**  
`net.core.wmem_max` caps the kernel send/write buffer size globally across all protocols.  
`rmem` limits receive buffers, and the `tcp_*` parameters are TCP-specific only.

---

## Question 4: App Engine Newsfeed Duplicates

**Problem:** Users see already-viewed articles again during peak load. What’s the likely cause?

**Options:**
- A. Session local to single instance  
- B. Session overwritten in Datastore  
- C. Modify API URL for caching  
- D. Set HTTP expires header to -1  

**Answer:** A  

**Core Concepts:**  
App Engine instances are stateless and ephemeral; local in-memory sessions (e.g., globals) don’t persist across instances. Load balancing between instances leads to user state inconsistency. Shared storage such as Datastore should be used for session data.

---

## Question 5: Legacy System Integration

**Problem:** Integrate on-prem legacy system from an acquired company in Spain into GCP while modernizing workloads.

**Options:**
- A. Containerize with GKE on-prem and use Anthos  
- B. Snapshot to Compute Engine  
- C. Containerize in GKE on GCP only  
- D. Use Transfer Appliance for data  

**Answer:** A  

**Core Concepts:**  
Anthos supports hybrid GKE cluster management across on-prem and cloud environments. Containerizing locally allows phased modernization without complete migration disruption.

---

## Question 6: J2EE Migration Practices

**Problem:** Recommend best practices for migrating J2EE applications to the cloud.

**Options:**
- A. Port to App Engine  
- B. Integrate Cloud Dataflow for metrics  
- C. Instrument with Stackdriver Debugger  
- D. Use IaC automation for infrastructure  
- E. Implement CI/CD with staging tests  
- F. Convert MySQL to NoSQL  

**Answers:** A, D, E  

**Core Concepts:**  
PaaS like App Engine abstracts infrastructure, **Infrastructure as Code (IaC)** ensures repeatable deployment (e.g., Terraform), and **CI/CD** pipelines automate testing and releases. Avoid unrelated tools or unnecessary database changes.

---

## Question 7: DeFi Private Keys Security

**Problem:** Securely handle temporary access to cold-stored private keys on-prem while using GCP.

**Options:**
- A. VM in separate VPC, peer to main  
- B. Copy keys via Cloud VPN to VM  
- C. Copy back keys and delete VM  
- D. Connect on-prem directly to main VPC  

**Answers:** A, B, C  

**Core Concepts:**  
Use an **isolated VPC** and **VPN** for secure transfer. Delete the VM post-operation to remove exposure. Avoid direct connection to the main VPC to reduce the attack surface.

---

## Question 8: Petabyte Dataset Storage

**Problem:** You have multi-petabyte datasets for analysts who use SQL and require high availability.

**Options:**
- A. BigQuery  
- B. Cloud SQL  
- C. Cloud Storage flat files  
- D. Datastore  

**Answer:** A  

**Core Concepts:**  
**BigQuery** is a serverless, scalable **data warehouse** designed for SQL-based OLAP on petabyte-scale datasets. Cloud SQL is transactional (OLTP), while Cloud Storage and Datastore lack SQL interfaces.

---

## Question 9: Global HTTP Load Balancer Backend

**Problem:** A global external HTTP(S) load balancer routes to regional MIGs. Which backend configuration is correct?

**Answer:** A  

**Core Concepts:**  
A global **HTTP(S) load balancer (Layer 7)** uses HTTP protocol to communicate with backends from regional MIGs. Backend services connect to health checks monitoring these groups. TCP is used for L4 load balancing, not HTTP(S).
# GCP Professional Cloud Architect Exam Guide

## Overview
This guide explains core concepts from 11 key Google Cloud Professional Cloud Architect exam questions (Q10-20). Each section covers the **problem scenario**, **core concepts tested**, **best answers**, and **why alternatives fail**. Focus is on GCP best practices for scalability, reliability, security, and cost optimization. All explanations reference official GCP documentation and exam patterns.[web:1][web:41][web:43]

## Table of Contents
- [Q10: API Versioning](#q10-api-versioning)
- [Q11: Local Kubernetes Development](#q11-local-kubernetes-development)
- [Q12: Reducing Deployment Rollbacks](#q12-reducing-deployment-rollbacks)
- [Q13: Cloud SQL Disaster Recovery](#q13-cloud-sql-disaster-recovery)
- [Q14: Dev VM Cost Optimization](#q14-dev-vm-cost-optimization)
- [Q15: On-Prem to GCP Connectivity](#q15-on-prem-to-gcp-connectivity)
- [Q16: High-Throughput IoT Data](#q16-high-throughput-iot-data)
- [Q17: GKE Multi-Tier Security](#q17-gke-multi-tier-security)
- [Q18: Load Balancer Health Checks](#q18-load-balancer-health-checks)
- [Q19: PII Handling at Scale](#q19-pii-handling-at-scale)
- [Q20: GCE to BigQuery Authentication](#q20-gce-to-bigquery-authentication)

## Q10: API Versioning
**Scenario**: Run old/new API versions simultaneously using same domain/SSL/DNS.

**Core Concepts**: Path-based routing, load balancer backend pools, avoiding DNS changes.

**Best Answer**: `D. Use separate backend pools for each API path behind the load balancer`

| Why D Works | Why Others Fail |
|-------------|-----------------|
| Single LB → `/v1` (old pool), `/v2` (new pool). Same IP/SSL.[web:1][web:5] | A: New LB needs new DNS/IP<br>B: Breaks old clients<br>C: Inefficient app-layer proxy |

## Q11: Local Kubernetes Development
**Scenario**: Google Cloud Code + Anthos IDE integration for local K8s clusters.

**Core Concepts**: Minikube for single-node local clusters using local Docker.

**Best Answer**: `C. Minikube`

- Cloud Code auto-provisions Minikube (Docker driver) for local dev/debug.[web:6]
- **A**: Enterprise platform (not local)<br>**B**: Cloud IaaS (unrelated)<br>**D**: Skaffold builds/deploys (needs cluster).

## Q12: Reducing Deployment Rollbacks
**Scenario**: Post-QA improvements, further reduce prod rollbacks (choose 2).

**Core Concepts**: Blue-green deployments, microservices blast radius reduction.

**Best Answers**: `A. Blue-green deployment`, `C. Microservices`

| Strategy | Benefit |
|----------|---------|
| Blue-green | Dual envs; instant rollback via traffic switch [web:7] |
| Microservices | Independent deploys; failure isolated to one service |

**Wrong**: B (Canary ≠ QA replacement), D/E (DB changes don't fix deploys).

## Q13: Cloud SQL Disaster Recovery
**Scenario**: London region DR for financial app; EU-only data; near-zero RPO (choose 3).

**Core Concepts**: Cross-region read replicas + promotion, zone HA.

**Best Answers**: `A. Cross-region replica (Belgium) + failover`, `B. Zone standby`, `D. Cross-region replica (Frankfurt)`

- London (europe-west2) → Belgium (europe-west1), Frankfurt (europe-west3).[web:22][web:23]
- **C**: Violates EU residency.

## Q14: Dev VM Cost Optimization
**Scenario**: On-prem → GCP dev VMs; persist state on start/stop; cost visibility (choose 2).

**Core Concepts**: VM stop (not terminate), persistent disk flags, billing labels.

**Best Answers**: `A. --no-auto-delete + stop VM`, `D. BigQuery billing + labels`

- Stop = vCPU/RAM free, disk persists.<br>Labels group costs by team.[web:9][web:17]
- **Wrong**: B/E/F lose data; C (no CPU labels).

## Q15: On-Prem to GCP Connectivity
**Scenario**: Low bandwidth, no budget/private/SLA/custom routing, Google Workspace access.

**Core Concepts**: Direct Peering (free public peering).

**Best Answer**: `A. Direct Peering`

| Option | Cost | Private? | Fits? |
|--------|------|----------|-------|
| Direct Peering | Free* | No | ✅ All requirements [web:18][web:29] |
| B/C/D | Paid | Yes | ❌ Budget/private |

*You pay colocation; Google peering free.

## Q16: High-Throughput IoT Data
**Scenario**: 1K motion sensors/sec → simple time-series data + analytics.

**Core Concepts**: NoSQL for high-write IoT (horizontal scale, schema flex).

**Best Answer**: `B. NoSQL`

- Bigtable/Firestore: 1K+ writes/sec, auto-scale.[web:11]
- **Wrong**: A (no queries), C (write limits), D (no structure/queries).

## Q17: GKE Multi-Tier Security
**Scenario**: Web → App only (block DB) in single VPC.

**Core Concepts**: VPC firewall rules + network tags on GKE nodes.

**Best Answer**: `B. Custom tags + firewall rules`

- Tags target GKE nodes: `allow web → app-tag`.[web:12][web:20]
- **Wrong**: A (overkill VPCs), C (routes ≠ firewalls), D (labels ≠ tags).

## Q18: Load Balancer Health Checks
**Scenario**: MIG recreates instances/min despite working app (no public IPs).

**Core Concepts**: LB health probes from Google IP ranges (firewall allow).

**Best Answer**: `C. Firewall rule for LB health checks`

- Allow `35.191.0.0/16`, `130.211.0.0/22` → port.[web:3][web:36]
- **Wrong**: A (LB public), B (no public IPs), D (no LB name source).

## Q19: PII Handling at Scale
**Scenario**: Auto-discover/classify/anonymize PII across Storage/Datastore/BQ; low overhead (choose 2).

**Core Concepts**: DLP scanning + Data Catalog tagging; Dataflow + DLP templates.

**Best Answers**: `A. DLP scan + Data Catalog tags`, `C. DLP templates + Dataflow`

| Phase | Tools |
|-------|-------|
| Discover | DLP scans → Catalog tags [web:27] |
| De-ID | Templates + Dataflow batch [web:32] |

**Wrong**: B (Catalog can't scan), D (no scale).

## Q20: GCE to BigQuery Authentication
**Scenario**: Python script connection fails from GCE VM.

**Core Concepts**: Service accounts (least privilege) attached to VM.

**Best Answer**: `C. Service account + BigQuery role`

- Attach SA (roles/bigquery.user) → auto-auth.[web:28][web:38]
- **Wrong**: A (perms issue), B (scopes legacy), D (CLI ≠ Python).

## Key Takeaways
- **Always verify requirements** (e.g., budget, residency, scale).
- **GCP primitives**: Tags > labels (firewalls); stop > terminate (cost); SAs > scopes (auth).
- **Exam pattern**: Process of elimination + GCP docs knowledge.[web:41][web:43]

**Practice Tip**: Focus on "why NOT" other options. Reference: [GCP Architect Exam Guide](https://cloud.google.com/learn/certification/cloud-architect).

---
