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

*Author: GCP Solution Architect*  
*Last Updated: December 2025*
