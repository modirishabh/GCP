# GCP Professional Cloud Architect Exam Questions 121–130

## Overview
This README covers core concepts from **Google Cloud Professional Cloud Architect** practice questions (121–130). Each section explains essential GCP services, best practices, and reasoning for correct answers — focusing on disaster recovery, data management, caching, migrations, and compliance.

---

### Question 121: Disaster Plan Testing
**Core Concept:** Automate disaster recovery (DR) testing using Infrastructure as Code (IaC) and monitoring tools.  
- **Deployment Manager:** Declarative IaC for reproducible service provisioning in test environments.  
- **Stackdriver (Operations Suite):** Comprehensive monitoring and debugging tools.  

**Avoid:** Imperative scripts or relying only on admin activity logs.  
**Why B?** Combines recommended IaC + monitoring approach.  
**✅ Correct Answer:** **B. Deployment Manager + Stackdriver**

---

### Question 122: Medical Data Retention
**Core Concept:** Enforce compliance using automated lifecycle management for data retention/deletion.  
- **Cloud Storage Lifecycle Rules:** Delete objects based on age or conditions automatically.  

**Avoid:** Google Drive, manual scripts, or indefinite anonymization.  
**Why C?** Scalable, automatic, and compliant.  
**✅ Correct Answer:** **C. Cloud Storage + Lifecycle Management**

---

### Question 123: PHP App Caching
**Core Concept:** Reduce Cloud SQL load using Memcache with a cache-aside pattern.  
- **Dedicated Memcache:** Reliable and high-capacity option.  
- **Query Hash Keys:** Unique keys for query results.  

**Avoid:** Shared Memcache, overwriting keys, or cron-based prepopulation.  
**Why A?** Implements standard on-demand caching design.  
**✅ Correct Answer:** **A. Dedicated Memcache + Query Hash**

---

### Question 124: Reliable Task Scheduling
**Core Concept:** Separate scheduling from task processing using durable messaging.  
- **App Engine Cron:** Reliable task scheduling.  
- **Pub/Sub:** Ensures durable, retryable message delivery.  

**Avoid:** Direct function calls or relying on GKE “cron” alternatives.  
**Why B?** Builds fault-tolerant architecture per GCP best practices.  
**✅ Correct Answer:** **B. App Engine Cron + Pub/Sub**

---

### Question 125: Large Data Migration
**Core Concept:** Use hybrid transfer strategies for petabyte-scale data.  
- **Transfer Appliance:** Physical data ingestion (offline).  
- **Dedicated Interconnect:** Private, high-bandwidth link for ongoing transfers.  

**Avoid:** gsutil or VPN for large-scale initial migrations.  
**Why B?** Efficiently handles bulk and incremental data transfer.  
**✅ Correct Answer:** **B. Transfer Appliance + Dedicated Connection**

---

### Question 126: VM Migration Process
**Core Concept:** Use Google’s native tools with proper assessment and planning.  
- **Migrate for Compute Engine:** Automated migration solution.  
- **Workflow:** Assess → Plan/Runbook → Execute → Test/Cutover.  

**Avoid:** Manual imaging or skipping assessment.  
**Why C?** Aligns with official Google migration lifecycle.  
**✅ Correct Answer:** **C. Assess, Plan, and Migrate for Compute Engine**

---

### Question 127: Application HA Architecture
**Core Concept:** High-availability (HA) design for non-scalable, file-system-based TCP applications.  
- **Unmanaged Instance Group:** Active/standby model without auto-scaling.  
- **Regional Persistent Disk:** Shared, synchronously replicated storage.  
- **Network Load Balancer:** Layer 4 TCP load balancing and failover.  

**Avoid:** Firestore (NoSQL) or HTTP(S) Load Balancers (Layer 7 only).  
**Why D?** Meets HA and non-scaling requirements precisely.  
**✅ Correct Answer:** **D. Unmanaged Group + Regional Disk + Network LB**

---

### Question 128: Hybrid Connectivity
**Core Concept:** Enable private, low-latency communication with Google Cloud.  
- **Dedicated Interconnect:** Physical direct connection between on-prem and GCP.  

**Avoid:** VPN (public internet), OpenVPN (manual setup), or direct peering (public-only).  
**Why D?** Ensures highest performance and privacy.  
**✅ Correct Answer:** **D. Dedicated Interconnect**

---

### Question 129: Cloud SQL Durability
**Core Concept:** Achieve minimal data loss with point-in-time recovery (PITR).  
- **Automated Backups:** Daily snapshots for restore points.  
- **Binary Logging:** Continuous transaction tracking for PITR.  

**Avoid:** Sharding or read replicas (scaling, not recovery).  
**Why C+D?** Provides maximum resiliency against data loss.  
**✅ Correct Answers:** **C. Binary Logging** and **D. Automated Backups**

---

### Question 130: PII Deletion Compliance
**Core Concept:** Enforce “right to be forgotten” through proactive data governance.  
- **DLP API:** Detects and classifies PII during ingestion.  
- **Data Catalog:** Enables locating and managing PII datasets efficiently.  

**Avoid:** Manual deletions, views, or hashing only (data persists).  
**Why B?** Proactive scanning and cataloging streamline compliance.  
**✅ Correct Answer:** **B. DLP API + Data Catalog**

---

## Study Tips
- Focus on the **GCP Well-Architected Framework** pillars: reliability, operations, and cost optimization.  
- Review **official documentation and hands-on labs** (particularly IaC, migration, and Pub/Sub topics).  
- Watch for **common pitfalls**, such as confusing monitoring layers, migration methods, or HA components.

# Google Cloud Professional Cloud Architect Exam Questions 131–140

**Topic:** GCP Certification Practice — Core Concepts & Best Practices for Questions 131–140

This README explains the core concepts behind Google Cloud Professional Cloud Architect exam questions 131–140, focusing on DevOps workflows, cost optimization, migrations, security, and high availability. Each question includes the key requirement, why wrong answers fail, and why the best answer succeeds.

---

## Table of Contents
- [Question 131: DevOps Staging/Promotion](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-131)
- [Question 132: VM Cost Optimization](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-132)
- [Question 133: DB Migration Minimal Downtime](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-133)
- [Question 134: External IP Governance](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-134)
- [Question 135: Firewall Insights Troubleshooting](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-135)
- [Question 136: Location-Based Storage Access](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-136)
- [Question 137: Non-Disruptive MIG Updates](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-137)
- [Question 138: Zonal Outage Recovery](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-138)
- [Question 139: Overlapping IP Resolution](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-139)
- [Question 140: Hadoop Migration](https://www.perplexity.ai/search/help-me-to-understand-this-bet-jlRYVA9YQj6CoiLh4A9hOg#question-140)

---

## Question 131: DevOps Staging/Promotion
**Core Concept:** Separate developer staging from ops promotion in cloud migration using managed container orchestration (e.g., Kubernetes Deployments) with minimal operations overhead. GKE handles cluster management; App Engine lacks pipeline flexibility.

| Option | Why Incorrect |
|---------|----------------|
| A. App Engine | Limited versioning for complex CI/CD pipelines |
| B. GKE on-prem | Not a cloud migration; high on-prem overhead |
| C. Compute Engine | Manual deployment platform build |

**✅ Best Answer: D. Google Kubernetes Engine**  
Developers stage via `kubectl apply`; ops promotes autonomously. Google manages the control plane.

---

## Question 132: VM Cost Optimization
**Core Concept:** Non-24/7 VMs save most by stopping (no compute charges) using serverless scheduling versus resizing/autoscaling (minimum running instances) or preemptibles (daytime interruptions).

| Option | Cost Savings Issue |
|---------|--------------------|
| A. Resize script | Still pays idle CPU |
| C. MIG autoscaling | Minimum 1 instance always running |
| D. Preemptibles | Unstable for development work |

**✅ Best Answer: B. Cloud Scheduler + Cloud Functions**  
Cron triggers automate start/stop; zero runtime cost.

---

## Question 133: DB Migration Minimal Downtime
**Core Concept:** For PCI-DSS compliance and minimum downtime, replication (not dump/restore) is needed. A VPN enables Cloud SQL replica sync for quick cutover.

**✅ Best Answer: C. Cloud VPN + Replication + Promote Replica**  
The app keeps reading/writing on-prem during replication; a brief promotion ensures consistent data.

---

## Question 134: External IP Governance
**Core Concept:** Organization-wide enforcement of IP policies uses **Organization Policy constraints** (preventive and scalable) versus VPC-level changes or NAT (reactive, limited).

**✅ Best Answer: D. Org Policy `constraints/compute.vmExternalIpAccess`**  
Denies external IPs by default and allowlists approved instances across the organization.

---

## Question 135: Firewall Insights Troubleshooting
**Core Concept:** Firewall Insights relies on firewall rule logs (opt-in); it does not use VPC flow logs or IAM audit logs.

**✅ Best Answer: B. Enable Firewall Rules Logging**  
Generates match data (IPs, protocols) to power firewall insights analysis.

---

## Question 136: Location-Based Storage Access
**Core Concept:** Combine IAM with contextual attributes (e.g., IP range) via **VPC Service Controls** to restrict API access from unauthorized networks.

**✅ Best Answer: A. VPC Service Controls Perimeter + Office IP Access Level**  
Perimeter enforces access even if the user has IAM read permissions.

---

## Question 137: Non-Disruptive MIG Updates
**Core Concept:** Opportunistic mode updates only new or healed instances, ensuring zero service disruption.  

| Mode | Behavior |
|-------|-----------|
| Proactive | Auto-replaces running VMs |
| Opportunistic | Applies only to new instances |

- **proactive update mode** is designed to actively and automatically replace your old instances with new ones. While this is a common way to update

- **opportunistic update mode.** This mode is designed for tells the instance group about the new version, but does not actively replace any running
instances. Instead, it waits for an opportunity to apply the update. This means any new instance created by the autoscaler or any instance recreated by the autohealer will be created using the new updated template.The existing instances are not affected and all new instances will have the update

**✅ Best Answer: D. Rolling Update (Opportunistic Mode)**  
Autoscaler and autohealer integrate non-disruptive updates.

---

## Question 138: Zonal Outage Recovery
**Core Concept:** Regional Persistent Disks replicate synchronously across zones, ensuring near-zero RPO and minimal downtime.

**✅ Best Answer: B. Instance Template + Regional PD (Same Region)**  
Instant failover to a healthy zone within the same region.

---

## Question 139: Overlapping IP Resolution
**Core Concept:** Cloud VPN requires unique RFC1918 address ranges; overlapping networks break routing consistency.  

**✅ Best Answer: A. Cloud VPN + Router + Re-IP (No Overlap)**  
Re-IP to eliminate fundamental routing conflicts.

---

## Question 140: Hadoop Migration
**Core Concept:** Dataproc offers managed Hadoop clusters with minimal operational burden; preemptible workers reduce batch job costs dramatically.

| Option | Management Overhead |
|---------|----------------------|
| C/D. Manual Compute | Full cluster operations required |
| A. Standard workers | Higher cost |

**✅ Best Answer: B. Dataproc + Preemptible Workers**  
Fully managed with low cost for tolerant batch workloads.

---

## Study Tips
- Focus on **managed services** (GKE, Dataproc, Cloud SQL) to reduce operations overhead.  
- Prioritize **serverless automation** (Cloud Scheduler, Cloud Functions) for efficiency.  
- Leverage **replication and org policies** for zero-downtime migrations and enterprise-scale governance.

