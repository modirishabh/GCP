# Google Cloud Professional Cloud Architect - Core Concepts (Questions 231-243)

## 📋 Overview
This README covers core concepts from Google Cloud Professional Cloud Architect exam questions 231-243. Each section explains the **fundamental principle**, **why other options fail**, and the **best practice solution** with citations.[web:1][web:6][web:10]

## 🛠️ Question 231: Troubleshooting Slow Applications
**Core Concept**: Always **diagnose before changing** infrastructure. First step is data collection via logs/metrics, not resource guessing.[web:1]

**Why Others Fail**:
- B: Upgrading instances assumes capacity issues (expensive guess)
- C: Backup restore risks data loss 
- D: Major rearchitecture delays diagnosis

**Best Answer**: **A** - Inspect Cloud Logging (errors) + Cloud Monitoring (CPU/memory/disk I/O)[web:1][web:4]

## 🌐 Question 232: VPC Connectivity (Overlapping Subnets)
**Core Concept**: **VPC Peering requires non-overlapping IPs**; use Cloud VPN for overlaps with custom routing.[web:6]

**Why Others Fail**:
- A: Peering blocks on subnet overlaps
- B: Massive migration/relaunch = high re-engineering
- D: SSH forwarding = temporary, not persistent

**Best Answer**: **C** - Cloud VPN gateways + targeted subnet routing[web:6]

## 💻 Question 233: Multi-Workstation gcloud Management
**Core Concept**: **Cloud Shell** = pre-configured, persistent, auto-updating environment (no local installs).[web:7]

**Why Others Fail**:
- B: VM costs + manual updates
- C/D: Manual installs across machines = maintenance nightmare

**Best Answer**: **A** - Google Cloud Shell (browser-based, persistent home dir)[web:7]

## 💾 Question 234: 10TB Cloud-to-Cloud Migration
**Core Concept**: **Storage Transfer Service (STS)** = direct cloud-to-cloud via high-speed backbone (fastest/cheapest).[web:8][web:21]

**Why Others Fail**:
- A: gsutil doesn't support 3rd-party providers
- C: Transfer Appliance = offline/physical (wrong use case)
- D: Download+upload = double bandwidth costs

**Best Answer**: **B** - Storage Transfer Service[web:8]

## 🔥 Question 235: Bigtable Hotspots
**Core Concept**: **Hotspots** = poor row keys concentrating writes (e.g., timestamps). Fix with **even key distribution** (hashing).[web:9]

**Why Others Fail**:
- A: API choice irrelevant to storage
- B: Old data deletion ≠ write distribution
- D: More nodes won't help uneven traffic

**Best Answer**: **C** - Review row key strategy (alphabetical spread)[web:9]

## 🔒 Question 236: Restrict IAM to Domain Users
**Core Concept**: **Organization Policy** (`iam.allowedPolicyMemberDomains`) = proactive prevention at org level.[web:10]

**Why Others Fail**:
- B: Service accounts ≠ external users
- C/D: Reactive scripts allow temporary unauthorized access

**Best Answer**: **A** - Domain restricted sharing policy[web:10]

## 📊 Question 237: 2-Year Log Retention + 30-Day Query
**Core Concept**: **Logging Agent → Sink → Storage** + lifecycle (Standard→Coldline) + **Bucket Lock** (compliance).[web:19]

**Why Others Fail**:
- B/D: Cron jobs unreliable vs agent
- C/D: BigQuery expiration deletes after 30 days (violates 2yr req)

**Best Answer**: **A** - Agent + sink + lifecycle + bucket lock[web:19]

## 🌉 Question 238: Hybrid VPN Migration Networking
**Core Concept**: **Non-overlapping IP ranges** prevent routing conflicts in VPN-connected networks.[web:20]

**Why Others Fail**:
- A/B/D: Any primary/secondary overlap = routing breaks

**Best Answer**: **C** - Unique GCP IP ranges[web:20]

## ✅ Question 239: Verify Log Authenticity
**Core Concept**: **Digital signatures** = cryptographic proof of integrity (public key verifies hash).[web:27]

**Why Others Fail**:
- A: Dual-write = redundancy, not verification
- B: DB access limits ≠ tamper-proofing
- D: JSON dumps = no integrity mechanism

**Best Answer**: **C** - Sign timestamp + log entry[web:27]

## 🚀 Question 240: No-Ops Autoscaling (Choose 2)
**Core Concept**: **PaaS** (App Engine) + **managed Kubernetes** (GKE) = minimal ops + auto-scaling.[web:29]

**Correct**: **B** (GKE) + **C** (App Engine Standard)
**Why Others Fail**: A/D/E = IaaS (manual OS/patching)[web:34]

## 🔐 Question 241: DevSecOps Speed + Security (Choose 2)
**Core Concept**: **Automated CI/CD scans** catch issues early without slowing releases.

**Correct**: **B** (source analyzers) + **E** (vuln scanners)
**Why Others Fail**: A = manual bottleneck; C = functional ≠ security; D = post-build only[web:28]

## 📈 Question 242: Promo Site (100-500K traffic) (Choose 2)
**Core Concept**: **Serverless** or **managed scaling** for unpredictable loads.

**Correct**: **A** (App Engine + Datastore) + **C** (MIG + Bigtable)
**Why Others Fail**: B = PD not multi-writer; D = single VM no scale[web:29]

## ⚙️ Question 243: Enable Autoscaling on Running GKE
**Core Concept**: **Update existing cluster** (no recreation/disruption).[web:33]

**Why Others Fail**:
- A: Resize = fixed size
- B: Tags irrelevant
- D: New cluster = redeploy disruption

**Best Answer**: **C** - `gcloud container clusters update ... enable-autoscaling`[web:33]
# Google Cloud Professional Cloud Architect - Questions 244–250

## Overview
These questions test core GCP concepts in IAM, load testing, backups, VM management, policy inheritance, data preparation, and troubleshooting.  
Each explanation covers the key principle, why the best answer works, and why others fail.

---

## Question 244: IAM Roles for Security Visibility
**Core Concept:** Principle of least privilege in GCP IAM for organization-wide read-only auditing.  

**Best Answer:** **B. Org Viewer + Project Viewer**  
- **Explanation:** Org Viewer shows the full hierarchy (organization, folders, projects), and Project Viewer provides read access to project resources without modification rights.  

**Why Others Fail:**  
- A, C, D include admin/owner roles that allow changes, violating the least privilege principle.

---

## Question 245: Load Testing Requirements (Choose 3)
**Core Concept:** GCP best practices for safe, measurable load testing of Compute Engine and Bigtable.  

**Best Answers:** **A, B, F**  
- **A:** Validate Bigtable performance under stress.  
- **B:** Separate project isolates quotas and billing.  
- **F:** Instrument logging and metrics for analysis.  

**Why Others Fail:**  
- C: Risks production outages.  
- D: Not a testing requirement.  
- E: Complex, uncontrolled replay scenario.

---

## Question 246: MySQL Backup Storage
**Core Concept:** High-performance, non-disruptive backups avoiding I/O contention on live database disks.  

**Best Answer:** **B. Local SSD backup volume + scp/gsutil to Cloud Storage**  
- **Explanation:** Local SSD offers ultra-low latency and high IOPS on separate hardware from the persistent disk.  

**Why Others Fail:**  
- A: Snapshots compete for I/O on the same disk.  
- C: FUSE-mounted GCS is too slow for large dumps.  
- D: RAID still impacts main disk I/O performance.

---

## Question 247: Copy VM Across Projects/Regions
**Core Concept:** Cloud-native VM replication using snapshots → images for cross-project sharing.  

**Best Answer:** **D. Snapshot → Image in GCS → New VM**  
- **Explanation:** Images are reusable boot templates shareable across projects; snapshots are project-bound.  

**Why Others Fail:**  
- A, C: Manual `dd` or `netcat` methods are error-prone and non-repeatable.  
- B: Snapshots cannot be used across projects.

---

## Question 248: IAM Policy Inheritance
**Core Concept:** GCP resource hierarchy (Org → Folder → Project) uses additive union for effective policies.  

**Best Answer:** **C. Union of node policy + inherited ancestors**  
- **Explanation:** Permissions accumulate downward; GCP IAM has no deny or intersection feature.  

**Why Others Fail:**  
- A: Ignores inheritance.  
- B, D: Incorrectly assume IAM has restriction or deny capabilities.

---

## Question 249: Data Anomaly Detection
**Core Concept:** Data ingestion and visual preparation for degraded on-premises data.  

**Best Answer:** **B. Cloud Storage → Cloud Dataprep**  
- **Explanation:** Stage data in Cloud Storage first, then use Dataprep's profiling and cleaning features to detect anomalies.  

**Why Others Fail:**  
- A, C: DataLab (Vertex AI Notebooks) is for analysis, not visual preparation.  
- C, D: Direct on-premises connections are less reliable—always stage data in the cloud.

---

## Question 250: MIG Restart Troubleshooting
**Core Concept:** Managed Instance Groups (MIGs) autoheal by restarting unhealthy instances via health checks, even without autoscaling.  

**Best Answer:** **C. Disable health check + add project-wide SSH key**  
- **Explanation:** Disabling the health check stops restart loops, and project-wide SSH keys allow stable access.  

**Why Others Fail:**  
- A: Viewer role is read-only and prevents SSH access.  
- B: Restart won’t fix the root cause.  
- D: Autoscaling is already disabled in this case.

## 🎯 Key Takeaways
| **Pattern** | **First Principle** | **Common Traps** |
|-------------|-------------------|------------------|
| Troubleshooting | Diagnose → Act | Resource guessing |
| Networking | No IP overlap | Peering overlaps |
| Operations | Managed services | Manual installs |
| Data | Right tool per job | Wrong migration path |
| Security | Prevent > React | Scripts over policies |
| Architecture | Match reqs to service | IaaS for PaaS needs |

**Study Tip**: Focus on **"first step"**, **"minimal change"**, **"Google recommended"** keywords in questions.[web:39]
