# Resilient System Design Guidelines

These guidelines explain how to make your system resilient when it calls other services such as APIs, databases, or microservices. Each section describes a best practice and how to apply it effectively.

---

## Exponential Backoff Retries

When a call to another service fails (e.g., due to a timeout or HTTP 503 error), retrying can help—but it must be done carefully. **Exponential backoff** increases wait time between retries to give the downstream service time to recover and to avoid “retry storms” that worsen overload.

**Example retry schedule:**
- 1st retry after 100 ms  
- 2nd retry after 200 ms  
- 3rd retry after 400 ms  
- 4th retry after 800 ms

**Key points:**
- Retry only on **transient errors** (timeouts, 429, 503), not on **business logic errors** (like 400 validation failures).  
- Set a **maximum number of retries** and **total timeout** to prevent indefinite hanging.  
- Add **jitter** (randomness) to delay intervals to prevent synchronized retry spikes.

---

## Gate Incoming Requests

When your dependencies experience trouble, it’s often better to reduce acceptance of new traffic. **Gating incoming requests** helps protect both your service and its dependencies.

**Common strategies:**
- **Circuit Breaker:** When too many calls to a dependency fail, “open the circuit” and fail fast until it stabilizes.  
- **Rate Limiting / Load Shedding:** Temporarily reject or shed low-priority requests when latency, error rate, or CPU usage is high.  
- **Backpressure:** If queues or thread pools are full, slow down or reject requests to avoid cascading failure.

The goal is to **protect the dependency and your own service** so the system remains stable under pressure.

---

## Prevent Duplicate Side Effects

Retries can sometimes cause unintended repeated actions—like recharging a credit card or sending duplicate emails. You need safeguards to make retries safe.

**Common strategies:**
- **Idempotent operations:** Repeating the same request produces the same end result.  
- **Idempotency keys:** The client provides a unique key; the server records the outcome and returns the same result for repeated requests with that key.  
- **Transactional guarantees:** Use data stores or message brokers that ensure “exactly-once” or “at-least-once” processing without duplication.

**Goal:** Ensure retries don’t cause repeated real-world side effects.

---

## Using Istio / Anthos Service Mesh

**Istio** and **Anthos Service Mesh** are service mesh platforms that let you configure reliability options (retries, timeouts, circuit breakers) centrally—without modifying application code.

**What they offer:**
- Centralized configuration (e.g., “Retry up to N times with exponential backoff and per-try timeout Y”).  
- Built-in support for **timeouts**, **retries**, **circuit breakers**, and **traffic shaping** (e.g., traffic splits, redirects, or fault injection).  
- Language-agnostic resilience: the mesh handles these behaviors at the network layer.

**Important cautions:**
- Avoid blindly retrying all HTTP methods—especially **non-idempotent** ones (like `POST` for payments).  
- Tune mesh-level retries carefully to prevent **retry amplification** (multiple layers of retries causing high traffic).  

---


# Kubernetes Pod Lifecycle & GKE Optimizations

Kubernetes ensures reliable pod management through graceful termination, health probes, workload-specific scaling, and networking tweaks. These concepts minimize disruptions while optimizing costs and performance.

## Graceful Pod Termination

When Kubernetes removes a pod (scaling, updates), it follows a sequenced shutdown to avoid data loss or outages:

- **preStop Hook**: Optional script runs first for cleanup (e.g., drain queues, close DB connections).
- **SIGTERM Signal**: Sent to main process—app stops new requests, finishes ongoing ones.
- **Grace Period**: Default 30s (`terminationGracePeriodSeconds`); extend for slow apps. SIGKILL follows if unfinished.

This pairs with readiness probes to remove pods from traffic early, ensuring smooth handovers.[page:1]

## Health Probes

Probes monitor pod states independently:

| Probe Type | Purpose | Example | Best Practices |
|------------|---------|---------|----------------|
| **Readiness** | "Ready for traffic?" Removes from load balancer if failing (e.g., cache warmup). | HTTP `/healthz` after startup. | Fast local checks, 5-10s period. |
| **Liveness** | "Still alive?" Restarts if stuck (e.g., deadlock). | TCP port check or metric endpoint. | Avoid false restarts; no side effects. |


### initialDelaySeconds
The `initialDelaySeconds` value represents how long Kubernetes waits **before performing the first probe** after the container starts up.

### periodSeconds
The `periodSeconds` value indicates **how often the probe will be performed** once it starts.

## Startup Probe
Pods can also be configured with a `startupProbe`, which determines whether the **application inside the container has successfully started**.  
If a `startupProbe` is configured, **no other probes (liveness or readiness)** will run until the `startupProbe` returns a `Success` state.

Keep probes simple—no remote calls—to prevent cascade failures.[page:2]

## Workload Optimization Strategies

Tailor GKE configs to workload type:

### Batch Jobs (e.g., Data Processing)
- **Node Autoprovisioning**: Auto-creates optimized pools for pending pods.
- **Preemptible VMs**: 80% cheaper for fault-tolerant jobs; dedicated pools via taints/labels.
- **optimize-utilization**: Autoscaler aggressively downsizes idle nodes.[web:7][web:12]

### Serving Workloads (e.g., APIs)
- Lean containers for rapid scaling.
- **Pre-warming**: Overprovision GPUs/CPUs or use pause pods to reserve capacity.
- Horizontal Pod Autoscaler + readiness for zero-downtime spikes.[web:8]

## Networking & DNS Efficiency

- **NodeLocal DNSCache**: Per-node DNS caching cuts lookup latency 50-90%, reduces kube-dns load.[web:9]
- **Container-Native Load Balancing**: GKE Ingress + Network Endpoint Groups route directly to pods, skipping node proxies.
- **Integration**: Probes + SIGTERM ensure traffic shifts seamlessly during terminations.

# Container-Native Load Balancing

## Overview
Container-native load balancing allows load balancers to directly target Kubernetes Pods, ensuring even distribution of traffic across them.

## Traditional Load Balancing vs Container-Native
Without container-native load balancing, load balancer traffic is routed to node instance groups and then forwarded via `iptables` rules to pods. These pods may or may not reside on the same node, leading to potential inefficiencies.

## Benefits
Container-native load balancing makes pods the central objects for load balancing. This approach provides several key advantages:
- Reduces the number of network hops.
- Enables more efficient routing.
- Decreases overall network utilization.
- Improves application performance.
- Ensures even traffic distribution across pods.
- Supports application-level health checks.

## Prerequisites
To use container-native load balancing, ensure that the **VPC-native** setting is enabled on your Kubernetes cluster.  
You can enable this by including the `--enable-ip-alias` flag when creating the cluster.


## Summary

Building resilient systems requires carefully managing retries, throttling overload, and preventing duplicate actions. Tools like Istio and Anthos Service Mesh can simplify this by handling key patterns declaratively, but always review settings to prevent unintended amplification or side effects.
