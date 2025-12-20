# Google Cloud Database Services Summary

This document summarizes key concepts of Google Cloud’s managed database services for structured, semi-structured, and unstructured data.

---

## Cloud SQL
- Fully managed relational database service supporting **MySQL, PostgreSQL, and SQL Server**.
- Google handles **patching, updates, and backups**; you manage DB users.
- **High availability (HA):** Primary and standby instances with synchronous replication for automatic failover.
- Supports **read replicas**, point-in-time recovery, and **on-demand backups**.
- Connection options:
  - **Private IP:** Best for security and performance within the same VPC/region.
  - **Cloud SQL Auth Proxy:** For secure external connection.
  - **Authorized external IP:** Least secure, for specific cases.
- Use **Cloud SQL** when you need a managed relational database but **don’t require horizontal scaling** or global replication.
- Cloud SQL is configured to automatically increase its storage capacity when it detects that available space is running low. This is a core feature of the managed service.While automatic increases are helpful, you might not want your database to grow indefinitely and generate a huge bill. To control this, Cloud SQL provides an option called the **automatic storage increase limit**. This allows you to set an upper boundary so the database will automatically grow, but only up to the limit you define.

---

## Cloud Spanner
- **Globally distributed, horizontally scalable relational database**.
- Combines relational schema and SQL with **NoSQL-like scale and high availability**.
- Offers **petabyte capacity**, **strong consistency**, and **automatic synchronous replication**.
- Ideal for **financial, inventory, and mission-critical apps** needing **global consistency**.
- SLAs vary by regional vs multi-regional configuration.
- Use **Cloud Spanner** when you outgrow traditional RDBMS or need **global transactions**.

---

## AlloyDB for PostgreSQL
- **Fully managed PostgreSQL-compatible** database for **transactional + analytical** workloads.
- Uses Google’s custom engine for **4× faster transactional** and **up to 100× faster analytical** performance compared to standard PostgreSQL.
- Automates **backups, replication, patching,** and **capacity management**.
- Integrates with **Vertex AI** for real-time ML predictions.
- Best for **enterprise-grade PostgreSQL workloads** needing **99.99% uptime** and advanced analytics.

---

## Cloud Bigtable
- **Fully managed NoSQL database** for **large, low-latency**, read/write workloads (petabyte scale).
- Powers Google services like Search and Maps.
- Ideal for **IoT, time-series, analytics, and ML applications**.
- Uses **HBase-compatible API** for easy integration.
- Data model:
  - Tables organized by **row keys**, **column families**, and **qualifiers**.
  - Supports versioned cells with timestamps.
- Scales linearly — adding nodes increases throughput proportionally.
- Use **Cloud Bigtable** for **high throughput, low latency**, and **structured or semi-structured** data.

---

## Memorystore (for Redis)
- **Managed in-memory data store** built on Redis.
- Provides **sub-millisecond latency**, **high availability**, and **99.9% SLA**.
- Handles **failover, patching, scaling**, and **monitoring** automatically.
- Scales up to **300 GB** and **12 Gbps throughput** per instance.
- **Protocol compatible with Redis**, so existing applications work without modification.
- Use **Memorystore** for **caching, gaming, session management, or real-time analytics**.

---

## Decision Overview

| Requirement | Recommended Service |
|--------------|--------------------|
| Relational DB (no horizontal scale) | **Cloud SQL** |
| Global relational consistency, horizontal scale | **Cloud Spanner** |
| Advanced PostgreSQL analytics and ML integration | **AlloyDB** |
| Large-scale NoSQL, very low latency | **Cloud Bigtable** |
| In-memory data caching | **Memorystore** |

---

For more details, visit: [Google Cloud SQL Documentation](https://cloud.google.com/sql/docs/)
