# TerramEarth Google Cloud Case Study - Questions 159-169

## Overview
TerramEarth (Terramirth) manufactures heavy equipment for mining (80%) and agriculture (20%), with 20M vehicles collecting 120 data fields/second. Current FTP-based pipeline creates 3-week stale data delays, causing up to 4-week customer downtimes. Business needs: reduce unplanned downtime <1 week, support 500+ dealers in 100 countries, enable real-time analytics and ML for predictive maintenance without surplus inventory.[web:1][web:5][page:1]

## Core Business Challenges
- **Stale Data**: 3-week aggregated reports limit proactive parts stocking
- **Limited Connectivity**: Only 200K/20M vehicles cellular-connected (9TB/day)
- **Scale Requirements**: Plan for 20,600 records/sec → 40TB/hour when all connected
- **API Needs**: Dealer/partner access to usage data for joint offerings
- **ML Goals**: Predict failures, optimize operations across connected/unconnected fleet

## Question 159: Real-Time Analytics Pipeline
**Core Concept**: Replace batch FTP with streaming IoT pipeline for central querying of raw vehicle data.

**Best Answer**: **A** IoT device → Google Cloud Load Balancing → GKE → Pub/Sub → Dataflow → BigQuery

**Why**: GKE + Load Balancer handles high-volume custom FTP ingestion scalably. Pub/Sub/Dataflow/BigQuery is GCP's standard real-time pattern vs App Engine's poor FTP fit.[web:5][page:1]

## Question 160: Low-Overhead API Development
**Core Concept**: Managed platform + API gateway for dealer/partner APIs, minimizing ops overhead.

**Best Answer**: **A** Google App Engine + Cloud Endpoints (focus: dealers/partners)

**Why**: App Engine serverless eliminates infrastructure management. Cloud Endpoints provides auth/monitoring. GKE requires more ops. Public-facing options wrong audience.[web:5]

## Question 161: Delegated Third-Party Access
**Core Concept**: OAuth 2.0 for revocable, scoped third-party data access without credential sharing.

**Best Answer**: **A** OAuth 2.0 compatible access control

**Why**: Enables dealerships to grant limited vehicle data access to partner tools. SAML=auth only. IP restrictions inflexible. Secondary creds = bad security practice.[web:5]

## Question 162: Massive IoT Ingestion (20M Vehicles)
**Core Concept**: Global messaging service as resilient ingestion buffer for extreme throughput.

**Best Answer**: **B** Vehicles write directly to Google Cloud Pub/Sub

**Why**: Pub/Sub built for millions events/sec globally. Decouples devices from processing. GCS poor for tiny writes. BigQuery streaming costly at scale. FTP unscalable.[web:5][page:1]

## Question 163: Fix 3-Week Reporting Delay
**Core Concept**: Holistic solution addressing data source + transport + analytics.

**Best Answer**: **C** Increase cellular connectivity to 80% + streaming transport + ML metrics analysis

**Why**: Must solve "only 200K connected" first. Streaming useless without data source. SFTP still batch/slow. Avoids inventory increase per requirements.[web:5]

## Question 164: Cloud Migration Process Impacts
**Core Concept**: Fundamental shift from CapEx hardware model to OpEx elastic services.

**Best Answer**: **B** Capacity planning, TCO calculations, OPEX/CAPEX allocation

**Why**: Cloud eliminates data center expansion. Transforms long-term capacity guessing → on-demand scaling, asset-based TCO → pay-per-use, CapEx → OpEx.[web:5][web:19]

## Question 165: Reliable Cellular Data Transfer
**Core Concept**: Resumable uploads + regional buckets minimize latency/restarts.

**Best Answer**: **D** HTTPS Google APIs to regional buckets (US/EU/Asia) + ETL from each

**Why**: HTTPS resumable uploads fix FTP restart problem. Regional buckets = lowest latency per geography vs multi-regional or FTP clusters.[web:5]

## Question 166: Cost-Effective Global Data Analysis
**Core Concept**: "Bring compute to data" - preprocess locally to minimize inter-region egress.

**Best Answer**: **D** Regional Dataproc clusters preprocess/compress → single regional bucket → final job

**Why**: Egress fees dominate multi-region costs. Local compression reduces transfer volume 10x+. Multi-regional storage more expensive than regional.[web:5]

## Question 167: Unconnected Vehicle Storage
**Core Concept**: Storage class matching access pattern + offline collection method.

**Best Answer**: **D** Hourly compression → GCS Coldline bucket

**Why**: Coldline = lowest cost for <1/year access (ML training next year). Nearline for monthly access. Real-time streaming impossible for unconnected trucks.[web:5]

## Question 168: Fleet-Wide ML Optimization
**Core Concept**: Edge ML - cloud training + local inference for connected/unconnected fleet.

**Best Answer**: **B** Capture data → train ML models → run locally on vehicles

**Why**: Works for entire 20M fleet. Cloud-only solutions fail unconnected vehicles. Manual rules can't capture complex patterns. Maintenance delivers model updates.[web:5]

## Question 169: Autonomous Vehicle Security
**Core Concepts**: Zero Trust + Secure Boot for complex vehicle systems.

**Best Answers**: **A + C**
- **A**: Treat microservice calls as untrusted (Zero Trust)
- **C**: TPM + verify firmware/binaries on boot (Secure Boot)

**Why**: Industry standards for autonomous systems. IPv6/functional langs/redundancy/Faraday irrelevant to software security.[web:5]

# Google Cloud Professional Architect Exam - Jenko Mart & Dress4Win Case Studies

## Overview
Detailed explanations of core concepts and best answers for Jenko Mart (Q170-175) and Dress4Win (Q176-179) case study questions from Google Cloud Professional Cloud Architect certification exam. Each question includes the **core concept** driving the solution and **why other options fail**.

## Jenko Mart Case Study Questions

### Q170: Domain/Project Structure for Least Privilege
**Core Concept**: Least privilege requires production isolation from dev/test/staging via separate GCP projects with IAM boundaries. Single Google Workspace account centralizes user management.[web:21][web:22][web:24]

**Best Answer**: **D** - Single G Suite account with one dev/test/staging project + one production project.

**Why Others Fail**:
- A/B: Multiple G Suite accounts = management nightmare
- C: One project per app stage = project explosion

### Q171: Migration Bottlenecks (Choose 3)
**Core Concept**: VPN migrations bottleneck at network (single tunnel, internet paths) and compute asymmetry (fewer target VMs).[web:6][web:14]

**Best Answers**: **A, D, F**
- A: Single VPN tunnel bandwidth cap
- D: Fewer GCP VMs than source = receive overload  
- F: Complex internet connectivity

**Why Others Fail**:
- B: Irrelevant (VMs, not Storage)
- C: Command rarely bottlenecks vs network
- E: Cloud storage designed for throughput

### Q172: SSH Troubleshooting (Database Working, Choose 3)
**Core Concept**: Non-disruptive diagnosis: firewall rules → serial console → disk snapshot analysis.[web:7][web:15]

**Best Answers**: **C, D, F**
- C: Snapshot disk for safe filesystem inspection
- D: Check inbound firewall (port 22 most common)
- F: Serial console bypasses SSH for boot/logs

**Why Others Fail**:
- A/B: Destructive (delete VM)
- E: Risky network change on live DB

### Q173: User Profiles Database
**Core Concept**: Flexible schemas → NoSQL (Datastore/Firestore). Profiles vary by user.[web:8][web:29][web:39]

**Best Answer**: **D** - Google Cloud Datastore

**Why Others Fail**:
- A: Spanner = overkill/expensive
- B: BigQuery = analytics, not transactional
- C: Cloud SQL = rigid relational schema

### Q174: Hybrid Service Account Strategy
**Core Concept**: On-premises = manual keys. GCE VMs = attach service account (GCP-managed, auto-rotated).[web:9][web:17]

**Best Answer**: **C** - Keys for on-premises + GCP-managed for VMs

**Why Others Fail**:
- A: VMs don't need manual keys
- B: Never use user accounts for apps
- D: Custom auth = unnecessary complexity

### Q175: Asia Expansion Success Metrics
**Core Concept**: Business (visits) + Technical (latency) directly track goals.[web:10]

**Best Answer**: **D** - Total visits + average latency (Asia)

**Why Others Fail**:
- A/B/C: Incomplete metric sets
- E: Implementation detail, not success measure

## Dress4Win Case Study Questions

### Q176: Low-Cost Backup Archival
**Core Concept**: Cron + gsutil → Coldline (infrequent access). Transfer Service = inter-cloud.[web:11]

**Best Answer**: **A** - Cron script + gsutil → Coldline bucket

**Why Others Fail**:
- B/D: Regional = expensive for archival
- C: Transfer Service overkill for server→bucket

### Q177: Application Server Machine Types
**Core Concept**: Right-sizing = start small → monitor → scale up. Avoid hardware mapping.[web:12][web:20]

**Best Answer**: **C** - Smallest instances → monitor → scale up

**Why Others Fail**:
- A: Physical mapping → overprovisioning
- B: RAM/CPU ratio blind guessing
- D: VM mapping still risks overprovisioning

### Q178: MySQL Migration (Min Downtime)
**Core Concept**: Async replication: on-prem master → cloud replica → brief cutover.[web:27]

**Best Answer**: **B** - Cloud MySQL replica + async replication

**Why Others Fail**:
- A: Dump + shutdown = long outage
- C: Dual writes = consistency nightmare
- D: MySQL→Datastore = full rearchitecture

### Q179: Stackdriver Uptime Check Failing
**Core Concept**: External checks blocked by firewall. Whitelist Google's uptime IPs.[web:33][web:38]

**Best Answer**: **B** - Download uptime IP list → inbound firewall rule

**Why Others Fail**:
- A: Agent irrelevant for external checks
- C/D: User-agent secondary to IP blocking

## Key Takeaways
- **Security**: Single Workspace + env-separated projects
- **Migration**: Network > compute > storage bottlenecks
- **Troubleshooting**: Non-destructive first (firewall → console → snapshot)
- **Database**: Match workload (NoSQL for profiles)
- **Auth**: Environment-specific (keys vs managed)
- **Sizing**: Right-size via monitoring
- **Monitoring**: Firewall IPs for uptime checks

## References
- Google Cloud Architecture Best Practices [web:21][web:22]
- Official Documentation: IAM, Monitoring, Compute [web:15][web:33]
- Case Study Analyses [web:26][web:27]

## Key Architecture Patterns Summary
| Pattern | Use Case | Services |
|---------|----------|----------|
| IoT Streaming | Real-time vehicle telemetry | Pub/Sub → Dataflow → BigQuery |
| Managed APIs | Dealer/partner access | App Engine + Cloud Endpoints |
| Edge ML | Fleet optimization | Cloud training → Local inference |
| Global Processing | Multi-region cost optimization | Regional Dataproc → Regional GCS |
| Secure Access | Third-party delegation | OAuth 2.0 |

**References**: [GCP TerramEarth Case Study](https://services.google.com/fh/files/blogs/master_case_study_terramearth.pdf)[web:34]
