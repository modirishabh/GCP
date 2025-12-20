# Google Cloud Professional Cloud Architect Exam Questions 81-92

## Overview
This README covers core concepts, explanations, and best answers for Google Cloud Professional Cloud Architect certification exam questions 81-92. Each section breaks down the fundamental networking, migration, security, and architecture principles tested.[web:40][web:42]

## Question 81: Hybrid Cloud VPN Networking
**Core Concept**: VPC networks connected via Cloud VPN must use non-overlapping IP ranges. Overlapping ranges cause routing ambiguity where routers cannot distinguish on-premises vs. cloud destinations.[web:1][web:3][web:50]

- **A**: Complete overlap → routing failure
- **B/D**: Partial overlap (primary/secondary) → still conflicts
- **C**: ✅ Non-overlapping ranges → unique routing

**Best Answer**: `C` - Use IP range on Google Cloud that does not overlap with on-premises.[web:3]

## Question 82: Cloud Datastore Indexes Deployment
**Core Concept**: Composite indexes for App Engine queries defined in `index.yaml`; deploy via `gcloud datastore create-indexes` command, not console or app code.[web:6][web:33]

- **B**: No auto-detection from Storage
- **C**: No direct console upload; avoid deleting indexes
- **D**: Apps cannot self-deploy indexes
- **A**: ✅ CLI deploys YAML safely

**Best Answer**: `A` - `gcloud datastore create-indexes <config-file>`[web:6]

## Question 83: Compute Engine Disaster Recovery
**Core Concept**: Regional outage protection requires Managed Instance Groups (MIGs) in different regions + global HTTP(S) Load Balancer for health-check failover. Same project simplifies management.[web:7][web:15][web:46]

| Option | Issue |
|--------|-------|
| A/B | Single instances (no self-healing) |
| D | Separate projects (complex) |
| C | ✅ MIGs + global LB |

**Best Answer**: `C` - Two MIGs (different regions, same project) + HTTP Load Balancing.[web:7]

## Question 84: App Engine to On-Premises DB
**Core Concept**: Standard environment sandboxed (no VPC); Flexible runs in VPC → supports Cloud VPN for private on-premises connectivity.[web:8][web:26][web:34]

- **A/B**: Standard → no VPC/VPN
- **C**: Flexible + firewall → exposes DB publicly
- **D**: ✅ Flexible + VPN → private tunnel

**Best Answer**: `D` - App Engine Flexible + Cloud VPN.[web:8]

## Question 85: Private VM Software Install
**Core Concept**: Private Google Access enables internal-IP-only VMs to reach Google APIs (Cloud Storage) via Google's backbone. Use `gsutil cp` (with `sudo`).[web:9][web:17][web:53]

- **B/D**: Firewall whitelisting outdated
- **C/D**: Source Repos not for binaries
- **A**: ✅ Storage + Private Google Access + `gsutil`

**Best Answer**: `A` - Cloud Storage + Private Google Access subnet + internal IP + `gsutil`.[web:9]

## Question 86: 75TB Data Migration
**Core Concept**: >10TB datasets use Transfer Appliance (shippable hardware). Google rehydrates (unpacks/encrypts) to Cloud Storage offline.[web:10][web:18][web:21][web:25]

- **C/D**: `gsutil` too slow/unreliable
- **B**: Data Prep ≠ ingestion
- **A**: ✅ Appliance + rehydrator

**Best Answer**: `A` - Transfer Appliance + rehydrator to Cloud Storage.[web:18]

## Question 87: Kubernetes Zero-Downtime Update
**Core Concept**: `kubectl set image deployment/<name> <new-image>` triggers rolling update: gradual pod replacement with health checks.[web:11][web:19]

- **B**: Instance groups bypass Kubernetes
- **C/D**: Delete/recreate → outage
- **A**: ✅ Rolling update via Deployment

**Best Answer**: `A` - `kubectl set image deployment/echodeployment <new-image>`[web:11]

## Question 88: BigQuery Cross-Project Billing
**Core Concept**: `bigquery.jobUser` (billing project) = run queries (billed there); `bigquery.dataViewer` (data projects) = read-only access.[web:12][web:20][web:28]

| Role | Billing Project | Data Projects |
|-----|----------------|---------------|
| A/B | `user` (too permissive) | ❌ |
| D | ❌ `jobUser` | `dataViewer` |
| C | ✅ `jobUser` | `dataViewer` |

**Best Answer**: `C` - `jobUser` (billing) + `dataViewer` (data projects).[web:20]

## Question 89: Temporary Uploads (No Google Accounts)
**Core Concept**: Signed URLs provide time-limited (24h) upload access without authentication—perfect for external users.[web:29][web:37]

- **A**: No bucket passwords
- **C/D**: Cloud Identity requires accounts
- **B**: ✅ Signed URL (expires 24h)

**Best Answer**: `B` - Cloud Storage signed URL (24h expiration).[web:37]

## Question 90: GDPR Compliance
**Core Concept**: Shared responsibility—Google secures infrastructure; customer designs app (encryption, access, retention) for GDPR.[web:42]

- **A/B/C**: No passthrough/compliance toggles/scanner sufficiency
- **D**: ✅ Architect for compliance

**Best Answer**: `D` - Design data security meeting GDPR.[web:42]

## Question 91: SQL Server HA (Zonal Outages)
**Core Concept**: Always On Availability Groups + Windows Failover Clustering across **zones** (data centers), not subnets.[web:30][web:38]

- **A/B**: Managed services (not self-managed SQL)
- **C**: Subnets ≠ zone HA
- **D**: ✅ Zones for zonal DR

**Best Answer**: `D` - Always On + clustering across zones.[web:38]

## Question 92: GKE Deployment Workflow
**Core Concept**: `gcloud container clusters create` (GCP infra); `kubectl apply -f deployment.yaml` (Kubernetes workloads).[web:31][web:39]

- **C/D**: `kubectl` cannot create clusters
- **A**: Deployment Manager ≠ Kubernetes apps
- **B**: ✅ `gcloud` + `kubectl`

**Best Answer**: `B` - `gcloud` (cluster) + `kubectl` (deployment).[web:39]

## Key Takeaways
- **Networking**: Non-overlapping CIDRs essential for hybrid[web:1]
- **Security**: Private Google Access + signed URLs[web:53][web:37]
- **Migration**: Transfer Appliance for large data[web:18]
- **HA/DR**: Zones (not subnets); MIGs + global LB[web:7]
- **Billing**: Separate jobUser/dataViewer roles[web:20]
- **Compliance**: Customer responsibility (shared model)[web:42]
# Google Cloud Certification Questions (93-100) - Core Concepts & Best Answers

## Overview
These questions test key Google Cloud Platform (GCP) concepts for compute, storage, monitoring, identity, and scaling. Each section breaks down the core concepts, why options fail, and the optimal solution with citations.[web:21][web:25]

## Q93: Scale-to-Zero Compute
**Core Concept**: Serverless services like Cloud Functions scale instances to zero when idle, charging only for executions—ideal for business-hours apps to eliminate costs.[web:2][web:21]

**Why Others Fail**:
- Compute Engine: Always bills for VMs, even idle.[web:21]
- GKE: Control plane incurs constant charges.[web:1]
- App Engine Flexible: Maintains minimum 1 instance.[web:21]

**Best Answer**: **A. Cloud Functions** [web:2]

## Q94: Datastore Retrieval Efficiency
**Core Concepts**: 
- **Get**: Direct key lookup (fastest).[web:25]
- **Query**: Index scan (slower).[web:25]
- **Batch**: Multiple ops in one call (low overhead).[web:32]

**Why Others Fail**:
- Queries (C/D): Unnecessary scans when keys known.[web:6]
- Multiple gets (B): High network overhead.[web:25]

**Best Answer**: **A. Batch get with keys** [web:25]

## Q95: gsutil Customer-Supplied Encryption Keys (CSEK)
**Core Concept**: CSEK encrypts data with user-provided keys via `.boto` config file; gsutil reads it automatically—no command-line key exposure.[web:15][web:26]

**Why Others Fail**:
- No `--encryption-key` flag (C/D).[web:15]
- `gcloud config` for tool settings, not gsutil (B).[web:33]

**Best Answer**: **A. Key in .boto file + gsutil** [web:26]

## Q96: Real-Time KPI Monitoring
**Core Concepts**: Custom metrics to Cloud Monitoring (Stackdriver) enable low-latency ingestion/dashboarding for streaming KPIs from game servers.[web:8][web:34]

**Why Others Fail**:
- Bigtable/Data Studio: Analytics, not real-time.[web:8]
- BigQuery batches: 10-min delays.[web:8]
- Datastore: Not time-series optimized.[web:8]

**Best Answer**: **B. Custom metrics to Stackdriver dashboard** [web:8]

## Q97: On-Prem AD Integration
**Core Concepts**: GCDS syncs AD users/groups to Cloud Identity (one-way, no passwords); SAML SSO delegates auth to AD for single sign-on.[web:17][web:27]

**Why Others Fail**:
- Admin API: Manages Google users only (A).[web:27]
- IAP: App-specific, not domain-wide (C).[web:17]
- GCDS doesn't replicate DCs (D).[web:17]

**Best Answer**: **B. GCDS + SAML SSO** [web:27]

## Q98: GKE Crash Loop Debugging
**Core Concept**: Stdout logs auto-collected/persisted in Cloud Logging across restarts; filter by container for crash analysis.[web:18][web:28]

**Why Others Fail**:
- Node logs miss app errors (A/D).[web:18]
- Direct connect impossible in 2s crash loops (C).[web:28]

**Best Answer**: **B. Stackdriver logs for GKE container** [web:18]

## Q99: Cloud SQL High Availability
**Core Concepts**: 
- **HA**: Failover replica in same region/different zone for auto-promotion on zone failure.[web:19]
- Read replicas: Read scaling only, no failover.[web:19]
- Cross-region: Disaster recovery.[web:29]

**Best Answer**: **D. Failover replica (same region, different zone)** [web:19]

## Q100: MIG Scaling Sequence
**Core Concepts**: Correct order for autoscaled MIG: **Disk → Custom Image → Instance Template → MIG** for peak scaling.[web:12][web:30]

**Why Others Fail**:
- No template from snapshot (A).[web:30]
- No MIG from image directly (B).[web:12]
- Wrong order (D).[web:30]

**Best Answer**: **C. Image → Template → Autoscaled MIG** [web:12]
