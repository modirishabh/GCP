# Google Cloud Storage and Database Services

## Overview
Every application needs data storage—be it business records, media, or sensor data. Google Cloud offers various storage and database solutions to meet these needs. The key is selecting the service that best matches your application’s data type, access pattern, and scalability requirements.

---

## Choosing the Right Service

| Data Type | Use Case | Recommended Service |
|------------|-----------|---------------------|
| Unstructured, needs shared file system | File sharing or media rendering | **Filestore** |
| Unstructured, no shared file system | Object storage | **Cloud Storage** |
| Structured, analytical workloads | Big data analysis | **BigQuery / Bigtable** |
| Structured, non-analytical workloads | Depends on relational type |
| → Relational, HTAP | **AlloyDB** |
| → Relational, no HTAP, local scale | **Cloud SQL** |
| → Relational, no HTAP, global scale | **Cloud Spanner** |
| Non-relational, caching needed | **Memorystore** |
| Non-relational, no caching | **Firestore** |

---

## Cloud Storage Overview
- **Object storage** for worldwide data access.
- **Scalable** to exabytes; **millisecond latency**.
- Used for websites, backups, archives, and data distribution.
- **Organized in buckets** (globally unique names) containing **objects** (files).

### Storage Classes
| Class | Best For | Notes |
|--------|-----------|-------|
| **Standard** | Frequently accessed “hot” data | No minimum duration or retrieval cost |
| **Nearline** | Accessed < once per month | Lower cost, 30-day minimum storage |
| **Coldline** | Accessed < once per 90 days | Very low cost, 90-day minimum |
| **Archive** | Rarely accessed (< once per year) | Lowest cost, 365-day minimum |

All have **11 nines (99.999999999%) durability**.

### Location Types
- **Region** – low latency within one location.  
- **Dual-region** – two paired regions for redundancy.  
- **Multi-region** – highest availability, global access.

### Access Control
- **IAM roles** control access at project, bucket, and object levels.  
- **ACLs** give fine-grained access.  
- **Signed URLs** grant time-limited access without authentication.  
- **Signed Policies** restrict upload parameters for more control.

---

## Cloud Storage Features

### Security & Encryption
- **Customer-supplied encryption keys (CSEK)** supported.

### Data Protection
- **Soft Delete (default)**: keeps deleted objects up to 7 days (configurable to 90 days).
- **Object Versioning**: maintains archived versions of overwritten or deleted objects.
- **Retention Lock**: ensures objects are kept for compliance reasons.

### Lifecycle Management
Automatically apply rules to:
- Delete old objects.
- Downgrade older objects to cheaper storage classes.
- Keep limited recent versions.

### Autoclass
Automatically transitions objects between storage classes based on usage patterns to minimize cost (no early deletion or retrieval fees).

---

## Data Transfer Options
For large dataset migrations:
- **Transfer Appliance**: physical device (hundreds of TBs–PBs).  
- **Storage Transfer Service**: online transfers (Cloud Storage, S3, HTTP).  
- **Offline Media Import**: third-party physical media service.

All Cloud Storage operations (create, update, delete, list) are **strongly consistent**.

---

## Filestore Overview
A **managed, high-performance file storage service** for applications needing:
- A traditional file system interface and shared access.  
- Integration with Compute Engine or Kubernetes Engine.

### Key Features
- Fully managed NFSv3-compatible network storage.  
- Predictable performance with independent tuning.  
- Scale to hundreds of TBs with low-latency access.  
- Common use cases: enterprise app migration, media rendering, EDA, data analytics, genome sequencing, and web hosting.

---

**Next Steps:**  
Explore each service’s documentation and labs in the Google Cloud course to practice configuring and connecting these services efficiently.
