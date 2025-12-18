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

## Summary

Building resilient systems requires carefully managing retries, throttling overload, and preventing duplicate actions. Tools like Istio and Anthos Service Mesh can simplify this by handling key patterns declaratively, but always review settings to prevent unintended amplification or side effects.
