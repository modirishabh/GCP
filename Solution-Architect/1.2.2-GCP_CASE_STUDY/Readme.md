# Google Cloud Professional Architect Case Study Guide

## Helicopter Racing League (HRL) Case Study

### Question 188: Card Tokenization Storage
**Core Concept**: Deterministic encryption produces the same ciphertext for identical plaintext, enabling duplicate credit card detection without storing plain text. Firestore in Datastore mode offers scalable, low-latency NoSQL storage for high-volume token vaults with customer-managed encryption key (CMEK) rotation support.[web:1][web:17][web:41]

**Best Answer**: **B**. Encrypt card data with deterministic algorithm and store in Firestore (Datastore mode).
- Secret Manager lacks query scalability for 10,000+ records [web:1]
- Memorystore is expensive, non-persistent [web:1]
- Cloud SQL encryption isn't deterministic by default [web:1]

### Question 189: Fastly CDN Firewall Rules
**Core Concept**: Cloud Armor protects external HTTPS load balancers (not VPC firewalls). `evaluatePreconfiguredExpr('sourceIpList-fastly')` uses Google's managed Fastly IP list for dynamic whitelisting.[web:43][web:49]

**Best Answer**: **D**. `gcloud compute security-policies rules create ... evaluatePreconfiguredExpr('sourceIpList-fastly')`
- VPC firewall rules target backend VMs, not load balancers [web:43]
- `src-ip-ranges=*` allows all internet traffic [web:43]

### Question 190: Automated Penetration Testing
**Core Concept**: Event-driven architecture using Pub/Sub decouples deployment from security testing. Deployment pipeline publishes to Pub/Sub topic, instantly triggering Cloud Function.[web:21]

**Best Answer**: **C**. Configure deployment job to notify Pub/Sub that triggers Cloud Function.
- Cloud Tasks/Storage don't capture repository deployments [web:21]
- Logging sync routes logs, not deployment events [web:21]

### Question 191: ML Model Interpretability
**Core Concept**: Vertex Explainable AI provides feature attributions showing input contributions to predictions, enabling trust in race outcome predictions.[web:46]

**Best Answer**: **A**. Use Explainable AI.
- Vision AI is for image analysis [web:46]
- Operations Suite monitors infrastructure [web:46]
- Jupyter aids development, not deployed explanations [web:46]

### Question 192: Race Telemetry Storage
**Core Concept**: BigQuery partitioning by season enables cost-effective ML training on specific years. Schema evolution handles growing telemetry fields.[web:27][web:47]

**Best Answer**: **C**. Use BigQuery, partition by season.
-- Train on previous season only (scans 1 partition)
SELECT * FROM race_telemetry
WHERE season = '2024' -- Only scans 2024 partition

text

### Question 193: Idle VM Detection
**Core Concept**: Recommender's `IdleResourceRecommender` uses ML on 14-day CPU/network metrics to identify zombie VMs.[web:19]

**Best Answer**: **C**. `gcloud recommender recommendations list --recommender=google.compute.instance.IdleResourceRecommender`[web:19]
gcloud recommender recommendations list
--recommender=google.compute.instance.IdleResourceRecommender
--format=yaml

text

## EHR Healthcare Case Study

### Question 194: HIPAA Compliance Audit
**Core Concepts**: 
1. **BAA required** - Legal contract defining PHI responsibilities [web:8][web:14]
2. **Compliant products list** - Only BAA-covered services handle PHI [web:8]

**Best Answers**: **A + B**
Execute BAA with Google Cloud​

Verify usage against compliant products list​

text

### Question 195: Secure Container Deployment
**Core Concept**: Binary Authorization enforces signed images + Container Registry vulnerability scanning creates complete supply chain security.[web:44][web:50]

**Best Answers**: **A + D**
CI/CD Pipeline: Scan → Sign → Deploy (only signed pass Binary Authz)

text

### Question 196: Business-Critical Connection
**Core Concept**: High availability requires redundant paths. Second Dedicated Interconnect enables failover.[web:45]

**Best Answer**: **A**. Add new Dedicated Interconnect connection.

### Question 197: Production Hybrid Connectivity
**Core Concept**: Google's 99.99% topology = 2× Dedicated Interconnect per metro × 2 metros (4 total connections).[web:45]

**Best Answer**: **D**. 2× Dedicated Interconnect in metro1 + 2× in metro2 (different zones).

# Google Cloud Professional Architect Exam Questions (198-210)

## Overview
These questions from Google Cloud Professional Cloud Architect certification focus on **performance optimization**, **security hardening**, **cost-effective storage**, **scalable architectures**, and **case study scenarios** (EHR Healthcare, Mountkirk Games). Core concepts emphasize **service selection**, **best practices**, and **trade-offs** between latency, cost, reliability, and scale.[web:1][web:26]

## Pub/Sub Publishing Latency (Q198)
**Scenario**: EHR portal timeouts after migration; no publishing errors but high latency.

**Core Concepts**:
- **Publishing latency**: Time from `publish()` call to acknowledgment.
- **Message batching**: Default client behavior groups messages (up to 10ms wait or batch size) for throughput efficiency, adding latency.[web:1][web:36]
- Pull/push affects *subscription delivery*, not publishing.
- Retries/backups irrelevant without errors.

**Best Answer**: **C. Turn off Pub/Sub message batching**  
Disables batching for immediate single-message sends (latency <1ms).[web:1]

## External IP Restrictions (Q199)
**Scenario**: Prevent backend Compute Engine public IPs; allow frontend only.

**Core Concepts**:
- **IAM**: Controls "who" (permissions); can't enforce resource configs.
- **Org Policies**: Platform-level constraints like `constraints/compute.vmExternalIpAccess` deny external IPs with tag/project exceptions.[web:6][web:29]

**Best Answer**: **A. Organizational policy constraint**  
Enforces at creation time, user-agnostic.[web:6]

## GKE Secure Networking (Q200)
**Scenario**: Minimize EHR GKE attack surface.

**Core Concepts**:
| Cluster Type | Nodes | Control Plane | Best For |
|--------------|--------|---------------|----------|
| Public | Public IPs | Public/Private | Quick dev |
| Private + Public Endpoint | Private IPs | Public (authorized nets) | Balanced |
| **Private + Private Endpoint** | Private IPs | Private (authorized nets) | **Max security** |[web:38][web:42]

**Best Answer**: **A. Private cluster + private endpoint + master authorized networks**  
No public exposure.[web:38]

## Cost-Effective Storage Migration (Q201)
**Scenario**: SAN workloads → GCP (logs, VMs, thumbnails, session state).

**Core Concepts**:
| Workload | Needs | Best Service |
|----------|--------|--------------|
| Logs (20TB) | Infrequent | Cloud Storage + Lifecycle (→Archive) |
| VM vols/templates (500GB) | Durable files | Cloud Storage Lifecycle |
| Thumbnails (500GB) | Infrequent | Cloud Storage Lifecycle |
| **Session state (200GB)** | Fast read/write, days-long durable | **Memcached + Datastore** |

Local SSD ephemeral (lost on stop).[web:39]

**Best Answer**: **B. Memcached + Cloud Datastore; Lifecycle Cloud Storage**.[web:39]

## BigQuery Audit Queries (Q202)
**Scenario**: Count user queries last month.

**Core Concepts**:
- **INFORMATION_SCHEMA.JOBS**: Recent jobs only (~recent).
- **Cloud Audit Logs**: Complete, authoritative API records (`bigquery.jobs.create`).[web:17][web:44]
- Filter Logs Explorer: `protoPayload.methodName="google.cloud.bigquery.v2.JobService.QueryJobs"`.

**Best Answer**: **D. Cloud Audit Logs filter**.[web:44]

## Mountkirk Games Case Study (Q203-210)
**Business**: Global game → GCP migration for scale, analytics, ML future.
**Key Needs**: Time-series ingestion, global low-latency, spikes, analytics.[web:26][web:32]

### Q203: Analytics Migration
**Best**: **A+B**. ETL → Dataflow eval; denormalize for BigQuery.[web:26]

### Q204: Compute Architecture
**Core**: Global L7 LB + multi-Z MIG + autoscaling + non-preemptible VMs.
**Best**: **D**.[web:12]

### Q205: Future-Proofing
**Core**: Data hoarding for ML; containerize (GKE portability).
**Best**: **A+B**.[web:26]

### Q206: Latency Resilience
**Core**: Chaos engineering (inject latency on analytics path).
**Best**: **A**.[web:26]

### Q207: Database Architecture
**Core**:
| Data Type | Service |
|-----------|---------|
| Time-series | Bigtable |
| Transactions | Spanner |
| Historical | BigQuery |

**Best**: **D**.[web:40]

### Q208: Time-Series DB
**Core**: Bigtable optimized for high-write time-series (gaming metrics).[web:40]
**Best**: **A**.

### Q209: REST Backend
**Core**: Multi-Z MIG (HA); L7 LB (HTTP-aware for REST).[web:12]
**Best**: **C**.

### Q210: Batch Transfers
**Core**: `gsutil -m cp` (parallel multi-threaded) for speed on many files.[web:25][web:27]
**Best**: **B**.

## Key Takeaways
- **Match service to workload**: Purpose-built > generalist.
- **Trade-offs**: Batching (throughput) vs. no-batch (latency).
- **Security**: Policies > IAM for configs; private everything.
- **Scale**: Global LB + MIGs + autoscaling.
- **Cost**: Lifecycle rules + right-tier storage.

**Study Tip**: Memorize Mountkirk/EHR requirements; practice mapping to GCP services.[web:32]

## Key Architecture Patterns Summary

| Pattern | Use Case | Services |
|---------|----------|----------|
| Deterministic Encryption | Duplicate detection | Firestore + CMEK [web:17] |
| Cloud Armor | Load balancer security | `evaluatePreconfiguredExpr()` [web:43] |
| Event-Driven | Automation triggers | Pub/Sub → Cloud Functions [web:21] |
| Data Warehouse | ML training | BigQuery partitioning [web:27] |
| Recommender | Cost optimization | IdleResourceRecommender [web:19] |
| HIPAA Compliance | Healthcare | BAA + compliant products [web:14] |
| Software Supply Chain | Container security | Binary Authz + vuln scanning [web:44] |
| HA Networking | Production connectivity | 4× Dedicated Interconnect [web:45] |

**References**: All answers validated against current Google Cloud documentation (2025)[web:1][web:8][web:14][web:17][web:19][web:21][web:27][web:43][web:44][web:45][web:46][web:47]
