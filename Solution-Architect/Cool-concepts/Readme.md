# Google Cloud Data Services Overview

This document provides a quick comparison of key **Google Cloud Platform (GCP)** data services — both for **data processing/preparation** and **data storage/analytics** — from a solution architect’s point of view.

---

## 🧩 Processing / Preparation Tools

| **Service** | **Type** | **Main Use Case** | **Who Uses It Mostly** |
|--------------|-----------|-------------------|-------------------------|
| **Dataprep** | No‑code wrangler | Clean and shape data before loading into analytics systems. | Analysts / Data Stewards |
| **Datalab** | Notebook | Explore data, prototype models, and run machine learning experiments. | Data Scientists / Engineers |
| **Dataproc** | Managed Spark/Hadoop | Execute batch ETL, migrate legacy Hadoop/Spark jobs, or build custom data pipelines. | Data Engineers / Big‑Data Developers |

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

## Exam-Style Gotchas

- Centralize monitoring in a **dedicated monitoring project / metrics scope** for multi-project setups.
- Use **custom metrics**, **OpenTelemetry**, or **Managed Prometheus** for **application-level monitoring**.
- For latency/communication issues between services:  
  → Prefer **Cloud Trace + Monitoring dashboards/alerts** over logs alone.

---
