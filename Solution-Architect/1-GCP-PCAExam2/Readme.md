# Google Cloud Solutions Summary

This document summarizes solutions to questions 21–31 related to Google Cloud Platform services, deployment, compliance, and monitoring strategies.

---

## Question 21: Model Performance Degradation Solution

**Correct Answers:** A and D

### AI Platform Approach (Option A)
AI Platform supports continuous evaluation jobs that compare predictions to ground truth labels, detecting performance drops for retraining.  
This feature fits the sales forecast scenario by tracking accuracy over time.

### Vertex AI Approach (Option D)
Vertex AI Model Monitoring detects data skew (production vs. training data) and drift (distribution changes), common causes of degradation.  
It triggers alerts and retraining automatically—this is the recommended modern solution.

---

## Question 22: Restrict to US-Central1 Region

**Correct Solution:** Option A

Use the `constraints/gcp.resourceLocations` organization policy to enforce region restrictions.  
Allow only `us-central1` to ensure all resources remain compliant at the organization level.

---

## Question 23: Mixed Batch/Stream Processing

**Best Choice:** Option B

Google Cloud **Dataflow** uses Apache Beam to unify batch and streaming pipelines.  
Ideal for hourly data jobs and real-time processing, unlike DataProc which lacks a unified model and requires more management.

---

## Question 24: Shielded VM Post-Update

Here’s your content formatted properly as a README.md file with Markdown headers, sections, and code formatting for commands:

text
# Google Cloud Concepts Overview

This document provides key insights into several Google Cloud Platform (GCP) services and security features, covering Shielded VMs, Dataflow, App Engine incident response, Cloud Armor, and more.

---

## Shielded VMs

Shielded VMs in Google Cloud enhance virtual machine security through features such as **Secure Boot**, **vTPM**, and **Integrity Monitoring**.

- **Integrity Monitoring**  It works by taking a snapshot of the systems boot sequence called a baseline when it's in a known good trusted state. Every time the VM boots, it compares the new boot sequence to this baseline. If they don't match,it raises an alert, signaling a potential integrity violation.  
- **Secure boot** is a feature of the Unified Extensible Firmware Interface (UEFI) that ensures only trusted software (signed by authorized publishers) can load during the boot process.
- **vTPM (Virtual Trusted Platform Module**) A primary role of the vTPM is to perform "measured boot," where it records cryptographic hashes (measurements) of the entire boot chain (UEFI, OS, drivers). These measurements can be used for remote attestation, allowing the cloud provider or a monitoring system to cryptographically verify the VM's health and ensure it has not been compromised.


---

## Dataflow and Unified Processing

**Google Cloud Dataflow**, powered by **Apache Beam**, offers a serverless data processing platform for both **batch** and **streaming** data using a unified programming model.

- Developers write pipelines once using the **Beam SDK**, and the same code handles:
  - **Bounded datasets** (batch jobs)
  - **Unbounded datasets** (real-time streams)
- This design makes Dataflow ideal for new data pipeline projects, while **DataProc** focuses on existing **Hadoop/Spark** workloads.

---
## High Availability (HA)

High Availability typically means protecting your application from failures **within a single region**.  
This is achieved by deploying multiple copies of your application across **different availability zones (AZs)** in that region.

**Goal:**  
Ensure minimal downtime and continuous service even if one zone experiences an outage.

**Example setup:**
- Deploy application instances across 2–3 availability zones.
- Use load balancers to distribute traffic automatically.
- Store data in replicated databases across zones.

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

## App Engine Incident Response

Performance issues like slow post-update load times (e.g., 30-second delays) may result from code-level problems such as **cold starts**.

**Recommended recovery steps:**
1. **Rollback** to the last stable version to restore service quickly.
2. Use **Cloud Trace** to analyze latency issues.
3. Use **Cloud Logging** in a **staging** environment to diagnose safely.
4. Avoid redeploying broken code or blaming external ISPs before confirming the root cause.

---

## Cloud Armor Purpose

**Cloud Armor** protects applications from **DDoS attacks** and serves as a **Web Application Firewall (WAF)**.

- Can defend against threats like **SQL injection** and **Cross-Site Scripting (XSS)**.
- Operates at the **network edge** to block malicious traffic before it reaches backend services.
- Differs from:
  - **IAM** (Identity and Access Management) – handles permissions.
  - **KMS** (Key Management Service) – manages encryption keys.
  - **Secret Manager** – stores secrets securely.

---

## Persistent Disk Expansion

When an **ext4** disk approaches capacity:

1. Resize it through the **Console** (VM can stay online).
2. Extend the filesystem using:

# Google Cloud Concepts and Exam Review

## Question 32: Hot Disaster Recovery Pattern

A **hot disaster recovery pattern** requires identical, live copies of the application across different regions for near-zero downtime failover.

- **Correct Option:** B  
- **Explanation:** Deploying **Compute Engine instances** in two zones across different regions achieves both:
  - **High availability (zone redundancy)**
  - **Disaster recovery (region redundancy)**  
  Same-region setups (C, D) only handle zonal failures, while single-zone setups (A) lack failover capability.

---

## Question 33: Scaling Spark/Hadoop Jobs

- **Correct Option:** B — **Cloud Dataproc**  
- **Explanation:** Cloud Dataproc is the managed service for running existing **Apache Spark** and **Hadoop** jobs with minimal code changes and operations overhead.  
  - Supports **lift-and-shift migrations**
  - Reduces maintenance and cluster management needs  
  - Other options:
    - **A (Dataflow):** Requires rewriting jobs in Apache Beam  
    - **C (Compute Engine):** Needs manual cluster management  
    - **D (Kubernetes Engine):** Adds unnecessary containerization complexity

---

## Question 34: vCPU Calculation

- **Calculation:**  
  - 4 servers × 2 dual-core CPUs = 16 physical cores  
  - **1 physical core = 2 vCPUs** (due to hyperthreading)  
  - Total = **16 × 2 = 32 vCPUs**

- **Correct Option:** C — **32 vCPUs**

---

## Question 35: Database Performance

For an I/O-intensive **MySQL import/normalization** on an 80 GB SSD persistent disk:

- **Correct Option:** C — **Resize to 500 GB**
- **Explanation:**  
  - Disk performance (IOPS and throughput) scales with disk size  
  - Increasing to 500 GB boosts IOPS **without downtime**
  - Outperforms:
    - **A:** Adding memory  
    - **B:** Changing engine type  
    - **D:** Migrating to BigQuery  
    - **E:** Modifying app code

---

## Question 36: Policy Enforcement (Transcript Error)

- **Correct Option:** B — **Policy Controller**
- **Explanation:**  
  The transcript incorrectly describes functionality.  
  - **Policy Controller** enforces policies via **admission control** on Kubernetes API requests using **YAML configurations**, similar to **OPA Gatekeeper**.  
  - Other tools:
    - **A (Config Connector):** Manages GCP resources as K8s objects  
    - **C (Service Mesh):** Manages traffic and observability  
    - **D:** Nonexistent "Config Knowledge Base"

---

## Question 37: High-Throughput Storage

- **Correct Option:** C — **Bigtable**
- **Explanation:**  
  - Supports **500,000 writes/second** with **low latency** and **massive scalability** for time-series sensor data.
  - **Not suitable alternatives:**
    - **A (BigQuery):** Great for analytics, not real-time ingestion  
    - **B (Cloud SQL):** Cannot scale to this throughput  
    - **D (Cloud Storage):** Designed for object storage, not databases

---

## Question 38: Cloud SQL Storage

- **Correct Options:** B and C  
- **Explanation:**  
  - **B:** Cloud SQL automatically increases storage when space is low.  
  - **C:** You can set a limit on automatic increases to avoid runaway growth.  
  - **A:** Manual increases are not required (false).  
  - **D:** Size limits do exist (false).

---

## Question 39: Resiliency Testing

- **Correct Option:** B — **Synthetic random load with chaos engineering**
- **Explanation:**  
  - Generates **synthetic random load** to validate **autoscaling behavior**.  
  - Uses **chaos engineering** by randomly terminating resources across zones in a **staging environment**.  
  - Other options:
    - **A:** Full zone failures test rare events only  
    - **C:** Exposes real users to risk  
    - **D:** Static provisioning ignores autoscaling

---

## Question 40: Managed Containers

- **Correct Option:** D — **Cloud Run**
- **Explanation:**  
  Cloud Run is **serverless**, handling both **container deployment and scaling** automatically.

  - **Alternatives:**
    - **A (Service Mesh):** Manages service communication  
    - **B (Config Connector):** Handles resource provisioning  
    - **C (Cloud Build):** Builds container images

---
