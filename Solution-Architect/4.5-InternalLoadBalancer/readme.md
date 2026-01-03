# Understanding Google Cloud Internal Load Balancing

This document explains **Google Cloud’s internal load balancing options**, which help distribute network traffic *within* your Virtual Private Cloud (VPC) rather than to the public internet.

---

## 1. What Is Internal Load Balancing?

An **Internal Load Balancer (ILB)** manages traffic between services inside your private network.  
It uses **private IP addresses**, ensuring that traffic **stays within Google’s network**.

**Benefits:**
- Improved **security** (no public exposure)
- Lower **latency** (traffic doesn’t leave the private network)
- Simplified **network pricing**

---

## 2. Types of Internal Load Balancers

Google Cloud offers three main types of internal load balancers:

### a. Internal Application Load Balancer (Layer 7)
- Operates at **Layer 7 (HTTP/HTTPS)**.
- Based on **Envoy proxy**.
- Designed for **HTTP(S)** applications like internal APIs or web services.

**Modes:**
- **Regional:** Clients and backends are within the same region (useful for regulatory compliance).
- **Cross-region:** Traffic is shared across multiple regions for **global accessibility** and **automatic failover**.

**When to use:**  
For internal web traffic, microservices, or APIs that need rich HTTP(S) controls (URL routing, headers, etc.).

---

### b. Internal Passthrough Network Load Balancer (Layer 4)
- Operates at **Layer 4 (TCP/UDP)**.
- **No proxy** — traffic flows **directly** from client to backend.
- Powered by **Andromeda**, Google’s virtual networking stack.
- Fully **software-defined** and **private**.
- Scoped to a **region**.

**When to use:**  
For **low-latency**, high-performance internal services (e.g., databases, messaging systems).

**Key advantage:**  
Because there’s no proxy, it’s faster and simpler — but still managed and scalable.

---

### c. Internal Proxy Network Load Balancer (Layer 4 with proxy)
- Operates at **Layer 4 (TCP)**.
- Uses **Envoy proxy** to manage connections.
- Can be deployed in **regional** or **cross-region** mode.
- Handles **connection termination**, **health checks**, and **traffic distribution** with more advanced control.

**When to use:**  
For TCP-based services that need richer load-balancing features (SSL termination, detailed logging, etc.).

---

## 3. Example: 3-Tier Web Architecture

A typical setup for internal load balancing in a **3-tier web service** looks like this:

1. **Web tier:**  
   - Uses an **external Application Load Balancer** with a global IP.  
   - Handles public traffic from users around the world.

2. **Application tier:**  
   - Uses an **internal Network Load Balancer** to distribute traffic within the private network.  
   - Only accessible through internal IPs.

3. **Database tier:**  
   - Stays fully private, only communicating with the internal application tier.  

**Benefits:**
- Increases **security** (no external exposure for internal systems).
- Improves **performance** (traffic remains in-region).
- Simplifies **network design**.

---

## 4. Comparison Summary

| Load Balancer Type | Layer | Proxy-Based | Scope | Typical Use Case |
|--------------------|-------|--------------|--------|------------------|
| Internal Application LB | 7 (HTTP/S) | Yes (Envoy) | Regional / Cross-region | For internal web or API traffic |
| Internal Passthrough Network LB | 4 (TCP/UDP) | No | Regional | For private, low-latency internal services |
| Internal Proxy Network LB | 4 (TCP) | Yes (Envoy) | Regional / Cross-region | For managed TCP services with rich controls |

---

## 5. Key Takeaways
- All internal load balancers use **private IPs** within a VPC.
- **Regional mode** confines clients and backends to one region.
- **Cross-region mode** enables global distribution and **failover**.
- Google Cloud’s load balancing is **software-defined** and **managed** — no need to maintain physical devices or VMs.

---

## Additional Resources
- [Google Cloud Load Balancing Overview](https://cloud.google.com/load-balancing/docs)
- [Internal Application Load Balancer Docs](https://cloud.google.com/load-balancing/docs/internal)
- [Internal Network Load Balancer Docs](https://cloud.google.com/load-balancing/docs/internal#network)
