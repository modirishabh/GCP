# Google Cloud Professional Cloud Architect - Mountkirk Games Case Study (Questions 211-223)

## Overview
These questions test core Google Cloud concepts for the Mountkirk Games case study, focusing on security, networking, governance, SRE practices, and cloud-native architectures. Emphasis is on least privilege, preventive controls, user-centric metrics, and Google-recommended managed services.[web:2][web:4][web:21]

## Question 211: Firestore Cross-Project Access
**Core Concept**: Secure programmatic access between projects requires a shared service account (SA) with minimal roles like Firebase Admin SDK Admin, avoiding broad roles like Organization Admin that violate least privilege principle.[web:4][web:28]

**Best Answer**: **C**. Create SA in legacy project, add to new project's IAM, grant Firebase Admin role in both projects. Enables restricted read/write access without migration or excessive permissions.[web:4]

**Why Others Fail**:
- A/B: Organization Admin is overly permissive
- D: Migration disrupts architecture

## Question 212: Restrict Resource Locations
**Core Concept**: Organization Policy Service enforces `constraints/gcp.resourceLocations` to prevent resource creation in unapproved regions, providing centralized, proactive governance over reactive quotas or IAM conditions.[web:6][web:14][web:29]

**Best Answer**: **A**. Configure organizational policy constraining resource locations. Uses allowed/denied region lists applied at organization/folder level.[web:6]

**Why Others Fail**:
- B: IAM conditions not for location restrictions
- C: Quota zeroing is unmanageable at scale
- D: Alerts are reactive, not preventive

## Question 213: Multi-Region GKE Ingress
**Core Concept**: Ingress for Anthos (Multi-Cluster Ingress) provides centralized controller with global HTTP(S) Load Balancer for routing across GKE clusters in multiple regions using Network Endpoint Groups (NEGs).[web:15][web:30]

**Best Answer**: **D**. Configure Ingress for Anthos with global load balancer and GKE. Single entry point routes to nearest regional cluster.[web:15]

**Why Others Fail**:
- A: Compute Engine, not container-native
- B: Cloud Armor (typo for "cabm") + legacy setup
- C: Standard GKE Ingress cluster-scoped only

## Question 214: User-Centric SLIs
**Core Concept**: SRE Golden Signals measure user experience: Latency (response time), Errors (failure rate). Internal metrics (CPU/memory/uptime) don't reflect end-user quality.[web:8][web:16][web:31]

**Best Answer**: **C**. Request latency and error rate. Directly measures user-perceived speed and reliability.[web:8]

**Why Others Fail**:
- A/B: CPU utilization is internal metric
- D: Server uptime ignores resilient architectures

## Question 215: GKE Authentication to GCP Services
**Core Concept**: Workload Identity Federation links Kubernetes Service Accounts (KSAs) directly to GCP Service Accounts via short-lived OIDC tokens, eliminating static key management.[web:9][web:25][web:32]

**Best Answer**: **A**. Configure Workload Identity and service accounts. Google's keyless, streamlined method for API access.[web:32]

**Why Others Fail**:
- B: Kubernetes secrets are base64-encoded only
- C/D: Complex encryption/Vault management unnecessary

## Question 216: Mobile App Testing
**Core Concept**: Firebase Test Lab provides cloud-based real/virtual Android/iOS devices for parallel automated testing across configurations, eliminating physical device labs.[web:10][web:26][web:33]

**Best Answer**: **A**. Upload app to Firebase Test Lab for Android/iOS testing. Cost-effective, scalable validation.[web:10]

**Why Others Fail**:
- B/C: No native Android/iOS VMs/containers in GCP
- D: Firebase Hosting is for web apps only

## Question 217: On-Premises Database Replication
**Core Concept**: Dedicated Interconnect provides private, high-bandwidth (10Gbps+), low-latency connections bypassing public internet for large/frequent data transfers.[web:19][web:34]

**Best Answer**: **A**. Google Cloud Dedicated Interconnect. Meets private IP and performance requirements for 4TB DB syncs.[web:19]

**Why Others Fail**:
- B/D: VPN limited by public internet bandwidth
- C: Custom NAT/TLS not standard solution

## Question 218: IAM Audit Log Analysis
**Core Concept**: Export Admin Activity audit logs to BigQuery enables fast SQL analysis of IAM changes; use authorized views for secure auditor access without full dataset exposure.[web:12][web:20]

**Best Answer**: **B**. Enable logging export to BigQuery with ACLs/views. Streamlines yearly IAM change analysis securely.[web:20]

**Why Others Fail**:
- A: Alerts for real-time, not historical analysis
- C: Cloud SQL not optimized for log analytics
- D: GCS good for storage, poor for querying

## Question 219: Microservices Credentials Storage
**Core Concept**: Secret Manager provides encrypted storage, IAM access control, automatic rotation for distributed systems, superior to code/env vars/files.[web:13]

**Best Answer**: **C**. Secret management system (Secret Manager). Secure runtime API access for 30+ microservices.[web:13]

**Why Others Fail**:
- A: Source code exposes secrets in repos
- B: Env vars leak via logs/processes
- D: Files vulnerable despite ACLs

## Question 220: Deployment Manager Business Risks (Select 2)
**Core Concepts**: Business adoption risks: team learning curve (productivity loss) and vendor lock-in (Google Cloud only), not technical implementation details.[web:13]

**Best Answers**: **C** (Unfamiliar to engineers), **F** (Only Google Cloud resources). Impact training costs and hybrid strategy flexibility.[web:13]

**Why Others Fail**:
- A/D/E/B: Technical features, not business risks

## Question 221: Cloud-Native Application Stack
**Core Concepts**: GKE (Kubernetes) provides open-source portability, autoscaling, multi-tenancy, service routing; Jenkins (CI/CD); Helm (templated deployments).[web:13][web:24]

**Best Answer**: **A**. Google Kubernetes Engine, Jenkins, and Helm. Complete stack meeting all requirements.[web:13]

**Why Others Incomplete**:
- B/C/D: Missing CI/CD or templating components

## Question 222: Preemptible VM Graceful Shutdown
**Core Concept**: `shutdown-script` metadata key executes user script during 30-second preemption grace period for state preservation/clean shutdown.[web:13]

**Best Answer**: **C**. Set `shutdown-script` metadata with shutdown script content. Native GCE mechanism.[web:13]

**Why Others Fail**:
- A/B/D: Incorrect triggers/systemd complexity

## Question 223: Multi-Tier VPC Security
**Core Concepts**: VPC Firewall Rules with network tags enforce hierarchical traffic flow (web→API→DB), blocking direct web-DB access; scales automatically.[web:13]

**Best Answer**: **D**. Add tags to tiers + firewall rules for desired flows. Centralized, autoscaling-friendly security.[web:13]

**Why Others Fail**:
- A: Subnets don't block intra-VPC traffic
- B: VM-level firewalls don't scale
- C: Routes direct, don't secure traffic

# Google Cloud Professional Cloud Architect Exam Questions 224-230

## Overview
This README provides detailed explanations of Google Cloud Professional Cloud Architect certification questions 224-230. Each section covers **core concepts**, **correct answers**, and **why alternatives fail**, with official GCP documentation citations. These questions test troubleshooting, architecture design, DevOps practices, IAM governance, networking, security, and CI/CD pipelines.[web:41][web:47]

## Question 224: GCE Kernel Module Failure Troubleshooting

**Scenario**: New Linux kernel module causes 50% batch server failures in GCE VMs after 2 days.

**Core Concepts**:
- Kernel modules extend Linux kernel functionality but can cause panics/crashes.
- GCE provides Cloud Logging (system logs), Serial Console (low-level kernel output), Cloud Monitoring (metrics).[web:1][web:11][web:18]

**Correct Actions (Choose 3)**:
- **A**: Cloud Logging searches module/kernel error entries.[web:4][web:7]
- **C**: Serial console shows boot panics/unresponsive VM logs.[web:1][web:6][web:11]
- **E**: Cloud Monitoring timeline reveals CPU/memory spikes at failure time.[web:13][web:18]

**Incorrect Options**:
| Option | Reason |
|--------|--------|
| B | Activity logs track admin actions only, not OS internals.[web:14][web:17] |
| D | Live migration unlikely for kernel issues. |
| F | Exporting VM locally is inefficient vs. native tools.[web:1] |

## Question 225: 100TB Log Archival + Analytics

**Scenario**: Archive 100TB logs for DR backup + test analytics with low risk.

**Core Concepts**:
- Cloud Storage: Cheap, durable object storage for archival.
- BigQuery: Serverless SQL analytics on petabyte-scale data.[web:23][web:24]

**Correct Steps (Choose 2)**:
- **E**: Upload to Cloud Storage for low-cost DR archival.[web:23][web:25]
- **A**: Load into BigQuery for SQL analytics testing.[web:21][web:31]

**Incorrect Options**:
| Option | Issue |
|--------|-------|
| B | Cloud SQL unsuitable for 100TB analytics scale.[web:22] |
| C | Cloud Logging for operational, not archival storage. |
| D | Bigtable for real-time NoSQL, not analytics. |

## Question 226: Instance Groups Bad Deployment Recovery

**Scenario**: Bad code impacts KPIs in self-healing instance groups; fix takes 1 week.

**Core Concepts**:
- Immutable infrastructure: Revert source code, let pipeline roll out safely.
- Avoid manual server edits (drift risk).[web:30]

**Correct Action**:
- **B**: Revert git change + rerun pipeline for rolling update.[web:30]

**Incorrect Options**:
- A/C: Manual server changes bypass CI/CD controls.
- D: Deleting instances causes outage (use rolling updates).[web:30]

## Question 227: Departmental IAM Governance

**Scenario**: Independent IAM per department but central control.

**Core Concepts**:
- Resource hierarchy: Organization → Folders → Projects.
- Folder-level IAM inherits to projects for delegation.[web:26][web:32]

**Correct Approach**:
- **C**: Single organization + folders per department.[web:32][web:38]

**Incorrect Options**:
| Option | Issue |
|--------|-------|
| A/B | Multiple orgs break central governance. |
| D | Project-level IAM unscalable for departments. |

## Question 228: MySQL Replication VPN Issues

**Scenario**: On-prem MySQL → GCP replication via VPN has latency/packet loss.

**Core Concepts**:
- VPN uses public internet (unreliable).
- Dedicated Interconnect: Private, low-latency link.[web:29]

**Correct Action**:
- **B**: Configure Dedicated Interconnect.[web:29]

**Incorrect Options**:
- A: UDP drops data (needs TCP reliability).
- C: Daily restores ≠ continuous replication.
- D: More VPNs still internet-dependent.
- E: Pub/Sub irrelevant for DB replication.

## Question 229: PII Sanitization for Bigtable Logs

**Scenario**: Sanitize customer support logs (email/chat) before Bigtable storage.

**Core Concepts**:
- DLP API: ML-based PII/PCI detection + redaction/tokenization.
- Preserves analyzable text post-sanitization.[web:28][web:40]

**Correct Approach**:
- **C**: Cloud DLP API to identify/redact sensitive data.[web:28][web:34]

**Incorrect Options**:
| Option | Issue |
|--------|-------|
| A | Hashing destroys all analysis value. |
| B | Encryption requires decryption (exposes PII). |
| D | Regex brittle, misses PII variants. |

## Question 230: Safe CI/CD Pipeline Verification

**Scenario**: Git pipeline verifies changes before production deployment.

**Core Concepts**:
- Staging environment testing via git tags.
- Jenkins triggers on tags: staging → test → prod tag.[web:27][web:33]

**Correct Approach**:
- **D**: Jenkins monitors tags → staging deploy/test → prod tag deploy.[web:27][web:33]

**Incorrect Options**:
- A/B: No pre-prod verification.
- C: Canary skips dedicated staging tests.

## Key Takeaways
- **Troubleshooting**: Logging + Serial Console + Monitoring trio.[web:1]
- **Big Data**: Storage (archive) + BigQuery (analyze).[web:23]
- **DevOps**: Revert code, never manual prod edits.[web:30]
- **Governance**: Folders for delegation.[web:32]
- **Networking**: Interconnect > VPN for reliability.[web:29]
- **Security**: DLP for intelligent sanitization.[web:40]
- **CI/CD**: Staging + tags = safe releases.[web:33]

**Study Tip**: Focus on GCP resource hierarchy, native troubleshooting tools, and DevOps best practices. Practice with official sample questions.[web:56]


## Key Takeaways
- **Security**: Least privilege, Workload Identity, Secret Manager, VPC firewalls
- **Governance**: Organization Policies > reactive controls
- **Networking**: Anthos Ingress, Dedicated Interconnect, tag-based firewalls
- **SRE**: User-facing Golden Signals (latency + errors)
- **Operations**: Native managed services over custom solutions[web:2][web:22]
