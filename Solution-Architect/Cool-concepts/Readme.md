# Google Cloud Data Services Overview

This document provides a quick comparison of key **Google Cloud Platform (GCP)** data services — both for **data processing/preparation** and **data storage/analytics** — from a solution architect’s point of view.

---

## 🧩 Processing / Preparation Tools

| **Service** | **Type** | **Main Use Case** | **Who Uses It Mostly** |
|--------------|-----------|-------------------|-------------------------|
| **Dataprep** | No‑code wrangler | Clean and shape data before loading into analytics systems. | Analysts / Data Stewards |
| **Datalab** | Notebook | Explore data, prototype models, and run machine learning experiments. | Data Scientists / Engineers |
| **Dataproc** | Managed Spark/Hadoop | Execute batch ETL, migrate legacy Hadoop/Spark jobs, or build custom data pipelines. | Data Engineers / Big‑Data Developers |
| **Dataflow**	| Batch + Streaming| 	Pipeline (Beam)|	Medium	ETL pipelines |
---

## 🗄️ Storage / Analytics Tools

| **Service** | **Type** | **Optimized For** | **Typical Workloads** |
|--------------|-----------|-------------------|------------------------|
| **Datastore** | NoSQL document database | Application data, transactions, and indexed queries. | Web/Mobile App Backends |
| **Bigtable** | Wide‑column NoSQL | Low‑latency, high‑throughput data access at scale. | Time‑Series, IoT, Personalization |
| **BigQuery** | Serverless data warehouse | Large‑scale SQL analytics and business intelligence. | Reporting, Ad‑hoc Queries, OLAP Workloads |

---

## 💡 Architectural Insight

- **Dataprep** simplifies pre‑analytics cleaning for non‑engineers.  
- **Dataproc** handles customizable, large‑scale ETL and ML workloads.  
- **BigQuery** is the de‑facto choice for high‑scale, SQL‑based analytics.  
- **Datastore** and **Bigtable** support transactional or high‑speed serving layers before data flows into BigQuery.

---

# Google Cloud Filestore vs Persistent Disk

Google Cloud **Filestore** provides managed, NFS-based file storage, while **Persistent Disk** offers block storage for individual VMs.  

- **Use Filestore** for shared access across multiple instances.  
- **Use Persistent Disk** for low-latency, single-VM workloads like databases.

## Disaster Recovery (DR)

Disaster Recovery protects your application from the **failure of an entire region**.  
It ensures that operations can resume quickly even if a full regional outage occurs.

### Hot DR Pattern

A **Hot DR** setup maintains a **fully active and synchronized** copy of your system in another region.  
Since the backup system is live, it can **instantly take over traffic** with minimal or no downtime.

**Key points:**
- Data is continuously replicated between regions.
- Traffic can be redirected immediately in case of primary region failure.
- Useful for mission-critical applications requiring near-zero downtime.
## Cloud Armor Purpose

**Cloud Armor** protects applications from **DDoS attacks** and serves as a **Web Application Firewall (WAF)**.

- Can defend against threats like **SQL injection** and **Cross-Site Scripting (XSS)**.
- Operates at the **network edge** to block malicious traffic before it reaches backend services.

---

## Key Differences

| **Aspect**       | **Filestore** (NFS Shared)                | **Persistent Disk** (VM-Attached)             |
|------------------|-------------------------------------------|----------------------------------------------|
| **Storage Type** | File storage (NFS)                        | Block storage                                |
| **Access**       | Multiple VMs / instances                  | Single VM (multi-attach limited)             |
| **Performance**  | ~5 ms latency, good throughput            | ~1 ms latency, high IOPS                     |
| **Scalability**  | Up to 320 TB (high-scale tiers)           | Up to 64 TB per disk                         |
| **Management**   | Fully managed                             | Self-managed                                 |
| **Cost (approx.)** | ~$0.20 per GB                           | ~$0.17 per GB                                 |

---

## Use Cases

- **Filestore** is ideal for:
  - Shared file systems
  - Media streaming platforms
  - HPC or ML datasets requiring concurrent access  
  - Multi-instance workloads

- **Persistent Disk** is best for:
  - Databases or transactional systems
  - Virtual machines and containers
  - High-performance or latency-sensitive applications
  - Single-instance storage needs

---

## Tip for GCP Kubernetes Workloads

For workloads like **Kubernetes pods** in GCP labs:

- **Persistent Disk (with CSI driver)** works better for **single-pod volumes**.
- **Filestore** is ideal for **persistent, multi-pod file shares**.

---

# Quick Summary: Read Replica vs Failover Replica

| **Feature** | **Read Replica** | **Failover Replica** |
|--------------|------------------|----------------------|
| **Purpose** | Scale read queries | Ensure high availability |
| **Replication** | Asynchronous | Synchronous |
| **Read Access** | Yes (read-only) | No (standby only) |
| **Failover** | Manual (optional) | Automatic |
| **Data Lag** | Possible | Minimal |
| **Use Case** | Performance | Reliability & uptime |

# App Engine Standard vs Flexible Environment

App Engine offers two environments: **Standard** (sandboxed, managed) and **Flexible** (container-based, customizable). Standard suits simple apps; Flexible handles complex needs. [web:11][web:12]

## Quick Comparison Table

| **Feature**          | **Standard Environment**                  | **Flexible Environment**                  |
|----------------------|-------------------------------------------|-------------------------------------------|
| **Runtime**          | Predefined (Python, Java, Node.js, Go, PHP) | Any language via Docker containers        |
| **Scaling**          | Automatic (to zero instances)             | Manual/automatic (runs on GCE VMs)        |
| **Cold Start**       | Very fast                                 | Slower (VM spin-up)                       |
| **Customization**    | Limited (sandbox restrictions)            | Full control (custom runtime, libraries)  |
| **Pricing**          | Free tier + usage-based (cheaper)         | VM-based (more expensive, no free tier)   |
| **SSH Access**       | No                                        | Yes (direct VM access)                    |
| **Built-in Services**| Full App Engine APIs (task queues, memcache) | Limited (runs on Compute Engine)       |
| **Deployment**       | `gcloud app deploy` (simple)              | Docker + `gcloud app deploy`              |
| **Use Case**         | Simple web apps, quick prototyping        | Complex apps, custom stacks, legacy code  |

## Key Differences Explained

### **Standard Environment**
- **Sandbox**: Runs in secure sandbox; no direct OS access.
- **Zero-downtime scaling**: Scales from 0 instances when idle.
- **Exam favorite**: "Deploy a simple Python web app with automatic scaling."

### **Flexible Environment**
- **Containerized**: Uses Docker on GCE VMs (Kubernetes underneath).
- **More control**: Install any library, access full Linux environment.
- **Exam gotcha**: "Need custom Node.js version or Ruby? Use Flexible."

## When to Choose What
- **Standard**: Simple apps, supported languages, cost-sensitive, fast scaling.
- **Flexible**: Unsupported languages, custom dependencies, need VM-level access.
- **Migration path**: Standard apps often run in Flexible with minor changes.

**Pro tip**: Start with Standard for speed/simplicity; switch to Flexible only if blocked by runtime limits.


# Google Cloud Monitoring — Exam-Focused Summary

Cloud Monitoring is tested primarily on **concepts**, not UI clicks. Focus on **what to monitor**, **how to alert**, and **how it ties into SRE/operations**.

---

## Core Concepts

### Metrics & Resources
- **Types:** system metrics (CPU, memory), GKE metrics, load balancer metrics, uptime checks, custom metrics.  
- Understand key terms:
  - **Metric type:** defines what is measured (e.g., `compute.googleapis.com/instance/cpu/utilization`).
  - **Time series:** sequence of measurements across time.
  - **Labels:** metadata providing dimensions for filtering/aggregation.
  - **Metrics scope:** view metrics from multiple projects (for central monitoring).

---

## Dashboards & Views
- **Built‑in dashboards:** available for GCP services (e.g., Compute Engine, GKE, Cloud SQL).
- **Custom dashboards:** visualize key metrics, combine multiple charts.
- **Usage tip:** correlate spikes (CPU, latency, errors) with incidents to identify causes.

---

## Alerting & Uptime Checks

### Alerting Policies
- **Conditions:** threshold-based, rate-of-change, or **MQL-based** conditions.
- **Notification channels:** email, SMS, Slack, PagerDuty, etc.
- **Alert types:**
  - **Metric-based alerts:** trigger on metric thresholds.
  - **Log-based alerts:** trigger on specific log patterns or frequency.
  - **SLO-based alerts:** trigger when error budget burn exceeds thresholds.

### Uptime Checks
- Perform **external health checks** for endpoints, load balancers, VMs, or APIs.
- Integrate with alerting to detect outages.
- Typical exam question: detect if a **public endpoint is down across locations**.

---

## SLO & SRE Topics

### SLI / SLO / Error Budget
- **SLI:** measurable service indicator (e.g., latency, availability).
- **SLO:** target performance (e.g., 99.9% uptime over 30 days).
- **Error budget:** \( 1 - \text{SLO} \).
- **Best practice:** use **SLO monitoring and burn rate alerts** instead of naive metrics (e.g., average latency > X).

### Service Monitoring
- Provides a **service‑centric view** for GKE, Cloud Run, App Engine, etc.
- SLOs are **auto‑detected** where possible.
- Exam tip: choose this when asked about "**monitoring microservices health following SRE best practices**."

---

## Integration & Agents

### Ops Suite Components
- **Cloud Monitoring**, **Cloud Logging**, **Cloud Trace**, **Cloud Profiler**, and **Cloud Debugger** form the **Operations Suite**.
- Typical pattern:  
  `Logs → Log‑based metrics → Alerts`  
  Use **Trace/Profiler** to debug latency and CPU issues.

### Agents & Hybrid Monitoring
- **Ops or Monitoring agent** for detailed OS/app metrics on GCE VMs.
- **Hybrid/External monitoring** with:
  - Managed Service for Prometheus  
  - Bindplane  
  - External endpoints

---

## Cloud Trace
Core Concepts:cloud trace captures the full path of each request. It then provides a detailed breakdown often visualized as a waterfall chart showing exactly how much time was spent in each micros service. This allows you to immediately pinpoint the service that is taking the longest and causing the high latency.This is done by adding the trace library to your code

## Exam-Style Gotchas

- Centralize monitoring in a **dedicated monitoring project / metrics scope** for multi-project setups.
- Use **custom metrics**, **OpenTelemetry**, or **Managed Prometheus** for **application-level monitoring**.
- For latency/communication issues between services:  
  → Prefer **Cloud Trace + Monitoring dashboards/alerts** over logs alone.

---
