# GCP Professional Cloud Architect Exam Questions 101-112: Core Concepts & Best Answers

## Overview
This README covers Google Cloud Professional Cloud Architect exam questions 101-112 from the provided transcript. Each section explains **core concepts**, **why the best answer works**, and **why others fail**. All explanations use current GCP best practices with inline citations.[web:1][web:21][web:36]

## Table of Contents
- [Q101: VPC Autoscaling Firewall Rules](#q101-vpc-autoscaling-firewall-rules)
- [Q102: Cloud SQL Scaling](#q102-cloud-sql-scaling)
- [Q103: OLAP Analytics](#q103-olap-analytics)
- [Q104: GKE-Cloud SQL Troubleshooting](#q104-gke-cloud-sql-troubleshooting)
- [Q105: VM-to-Pub/Sub Authentication](#q105-vm-to-pubsub-authentication)
- [Q106: Multi-Region VPN](#q106-multi-region-vpn)
- [Q107: BigQuery Log Retention](#q107-bigquery-log-retention)
- [Q108: GKE CPU Autoscaling](#q108-gke-cpu-autoscaling)
- [Q109: DR Hybrid Connectivity](#q109-dr-hybrid-connectivity)
- [Q110: HIPAA Batch Workloads](#q110-hipaa-batch-workloads)
- [Q111: MIG Startup Optimization](#q111-mig-startup-optimization)
- [Q112: BigQuery IAM Groups](#q112-bigquery-iam-groups)

## Q101: VPC Autoscaling Firewall Rules
**Scenario**: Restrict VM communications in autoscaling VPC without static IPs/subnets.

**Core Concept**: Network tags enable dynamic firewall rules. Tags attach to instance templates, so autoscaler-created VMs inherit them automatically.[web:1][web:2]

**Best Answer: B** - Firewall rules based on network tags.
Rule: allow tcp:80 from web-tier tag → api-tier tag
New VMs auto-tagged → rules apply instantly

 

**Why Others Fail**:
| Option | Why Incorrect |
|--------|---------------|
| A: Separate VPCs | Overkill; needs peering/VPN[web:1] |
| C: Cloud DNS | DNS resolves names, doesn't enforce network rules[web:4] |
| D: Service accounts | API-level auth, not L3/L4 network control[web:17] |

## Q102: Cloud SQL Scaling
**Scenario**: Auto-scale storage, keep CPU <75%, replication lag <60s.

**Core Concept**: Cloud SQL auto-storage + Monitoring alerts + sharding for lag.[web:21][web:6]

**Best Answer: A**
1. Enable automatic storage increase ✅
2. CPU alert → scale instance type ✅  
3. Lag alert → shard database ✅

**Why Others Fail**:
| Option | Storage | CPU | Lag |
|--------|---------|-----|-----|
| B | Auto ✅ | 32-core overkill | Memcached (reads only)[web:21] |
| C/D | Manual alerts ❌ | Memcached/32-core wrong | Wrong lag fixes[web:6] |

## Q103: OLAP Analytics
**Scenario**: Relational DB for 100s TB marketing analytics.

**Core Concept**: BigQuery = serverless data warehouse for OLAP (analytics queries).[web:7][web:15]

**Best Answer: D** - BigQuery for large-scale tabular processing.

**Workload Matrix**:
| Database | OLTP | OLAP | Scale |
|----------|------|------|-------|
| Spanner | ✅ | ❌ | Global[web:7] |
| Cloud SQL | ✅ | ❌ | TBs[web:15] |
| Firestore | ❌ | ❌ | NoSQL |
| **BigQuery** | ❌ | **✅** | PBs ✅ |

## Q104: GKE-Cloud SQL Troubleshooting
**Scenario**: Connection issues; need postmortem (root cause).

**Core Concept**: Cloud Logging captures GKE + Cloud SQL errors without data loss.[web:16]

**Best Answer: C** - Check Stackdriver logs for both services.

**Troubleshooting Priority**:
1. **Logs first** ✅ (preserves evidence)
2. ❌ Restarts destroy logs
3. ❌ Backups = data loss risk
4. ❌ Wrong IAM role (Cloud Build Editor irrelevant)

## Q105: VM-to-Pub/Sub Authentication
**Scenario**: Secure VM → Pub/Sub for sensitive data.

**Core Concept**: VM service accounts + IAM roles = automatic, no manual creds.[web:17][web:9]

**Best Answer: A** - Grant VM service account Pub/Sub roles.

**Auth Methods Ranked**:
| Method | Secure | Automated | Google Rec |
|--------|--------|-----------|------------|
| **Service Acct** | ✅ | ✅ | ✅[web:17] |
| Scopes | ❌ Legacy | ✅ | ❌ |
| Stored Tokens | ❌ Risky | ❌ | ❌ |
| Cloud Function | ✅ | ✅ | Overkill |

## Q106: Multi-Region VPN
**Scenario**: 75TB data → Cloud Storage across regions to on-prem.

**Core Concept**: Cloud VPN gateways are **regional** resources.[web:18]

**Best Answer: D** - Deploy VPN gateway per region + tunnels.

Region-US → On-Prem VPN Tunnel
Region-EU → On-Prem VPN Tunnel (separate gateway)

 

**Why Others Fail**:
- A: Peering = GCP↔GCP only[web:30]
- B: IAM/VPC sharing ≠ connectivity
- C: No "global VPN gateway"

## Q107: BigQuery Log Retention
**Scenario**: Per-app tables, delete logs >45 days, optimize storage.

**Core Concept**: Time-partitioned tables + partition expiration = auto-delete old daily partitions.[web:11][web:19]

**Best Answer: B**
CREATE TABLE logs TIMESTAMP PARTITIONED BY date
ALTER TABLE SET PARTITION EXPIRATION = 45 days

 

**Why Others Fail**:
| Method | Granular | Automated | Cost |
|--------|----------|-----------|------|
| **Partition Exp** | Daily ✅ | ✅ | Lowest ✅ |
| Table Exp | All data ❌ | ✅ | - |
| Scripts | ✅ | ❌ | High |

## Q108: GKE CPU Autoscaling
**Scenario**: Auto-add/remove nodes based on CPU load.

**Core Concept**: **Two-level scaling** - HPA (pods) + Cluster Autoscaler (nodes).[web:12][web:38]

**Best Answer: A**
HPA: targetCPUUtilizationPercentage: 75%
Cluster Autoscaler: gcloud container clusters update --enable-autoscaling

 

**GKE Scaling Flow**:
High CPU → HPA adds pods → Pending pods → Cluster Autoscaler adds nodes

 

## Q109: DR Hybrid Connectivity
**Scenario**: On-prem prod → GCP DR, secure + redundant.

**Core Concept**: Dedicated Interconnect (primary) + Cloud VPN (failover).[web:39]

**Best Answer: B**
Primary: Dedicated Interconnect (private, high BW)
Backup: Cloud VPN (encrypted, public internet)

 

**Hybrid Options**:
| Connection | Secure | Redundant | Use Case |
|------------|--------|-----------|----------|
| **Interconnect + VPN** | ✅ | ✅ | DR ✅ |
| Transfer Appliance | ❌ Offline | ❌ | One-time |
| Direct Peering | ❌ Public | ❌ | Internet |

## Q110: HIPAA Batch Workloads
**Scenario**: National batch jobs, non-time-critical, HIPAA, manage costs.

**Core Concept**: Preemptible VMs (80% savings) + disable non-HIPAA services.[web:27][web:34]

**Best Answer: B**
Provision: Preemptible VMs (fault-tolerant batch)
Compliance: Organization Policy → disable non-HIPAA APIs

 

**Cost vs Compliance**:
| VM Type | Cost | HIPAA | Batch Fit |
|---------|------|-------|-----------|
| **Preemptible** | 80% less ✅ | Covered | ✅ Fault-tolerant |
| Standard | Full price | Covered | Overkill |

## Q111: MIG Startup Optimization
**Scenario**: Automate MIG with many OS packages, minimize startup.

**Core Concept**: **Custom images** = pre-bake dependencies.[web:35]

**Best Answer: B**
Create custom image w/ all packages

Deployment Manager → MIG using custom image

 

**Startup Time**:
| Method | Boot Time | Scale Speed |
|--------|-----------|-------------|
| **Custom Image** | Seconds ✅ | Instant |
| Startup Script | Minutes | Slow |
| Puppet/Ansible | Minutes | Slow |

## Q112: BigQuery IAM Groups
**Scenario**: Country-specific analyst access to log datasets.

**Core Concept**: **Dual permissions** - JobUser (run queries) + Dataset View (read data).[web:36][web:29]

**Best Answers: A**
all_analysts@group: BigQuery JobUser (project-wide)
country_us@group: VIEW on us_dataset
country_eu@group: VIEW on eu_dataset

 

**IAM Matrix**:
| Role | Run Jobs | Read Data | Needs Both |
|-----|----------|-----------|------------|
| **JobUser + Dataset View** | ✅ | ✅ | ✅ Analysis |
| DataViewer only | ❌ | ✅ | Fails |
| Table-level | ✅ | ✅ | More work |

## Key Takeaways
- **Always use built-in automation** (auto-storage, partition exp, service accounts)
- **Understand service boundaries** (network vs API, regional vs global)
- **Two-level patterns recur** (HPA+Cluster Autoscaler, JobUser+Dataset View)
- **HIPAA = disable non-compliant** via org policy, not just "don't use"

## References
- [GCP VPC Firewalls][web:1]
- [Cloud SQL Auto Storage][web:21]
- [BigQuery Partitioning][web:11]
- Full citations in sections above
