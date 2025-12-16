# Google Cloud Network Load Balancers (NLBs)

Google Cloud **Network Load Balancers (NLBs)** are **Layer 4 (Transport Layer)** load balancers.  
They distribute traffic based on IP address and port, rather than application-level information like URLs or headers.

NLBs can route traffic to backend instances that are located **within a single region** or **across multiple regions**.

---

## Types of Network Load Balancers

There are two types of Network Load Balancers:

1. **Proxy Network Load Balancer**
2. **Passthrough Network Load Balancer**

Each type is designed for different network traffic and use cases.

---

## 1. Proxy Network Load Balancer

A **Proxy NLB** acts as a **reverse proxy**:  
It terminates the client’s connection, then opens a new connection to a backend instance.

### Key Features
- Handles **TCP traffic only**, with or without SSL.
- Can be **external** (internet-facing) or **internal** (private).
- Built on **Google Front Ends (GFEs)** or **Envoy proxies**.
- Supports **global, regional, or classic** deployment modes.

### Use Cases
- You need **advanced traffic control features**.
- You have **hybrid or multi-cloud backends**.
- You want to offload SSL/TLS termination at the load balancer.

### Example Configurations
- **Target TCP Proxy** – for plain TCP traffic.
- **Target SSL Proxy** – for encrypted SSL/TLS traffic.

### How It Works
1. The client connects to the global proxy load balancer.
2. The load balancer **terminates** the connection.
3. It chooses the **nearest healthy backend** (e.g., `us-east` or `us-central`).
4. A **new connection** (TCP/SSL) is established to that backend.

> 🧠 **Tip:** For HTTP(S) traffic, use a Google Cloud **Application Load Balancer** instead,  
> since it operates at **Layer 7 (Application Layer)**.

---

## 2. Passthrough Network Load Balancer

A **Passthrough NLB** is a **non-proxy** load balancer —  
it forwards packets directly to backends **without changing the source IP or port**.

### Key Features
- Operates at **Layer 4** (network transport).
- **Regional** only (load balancer and backends must be in the same region).
- Supports multiple protocols:
  - **TCP, UDP, ESP, GRE, ICMP, ICMPv6**
- Built using **Google Andromeda** and **Maglev**.
- Supports **Direct Server Return (DSR)** — responses go directly to clients.

### Use Cases
- You need to **preserve the client’s source IP**.
- You want **low latency** and **high performance**.
- You need **support for non-TCP protocols**.

### Deployment Variants
- **External Passthrough NLB:** Internet-facing, built on Maglev.
- **Internal Passthrough NLB:** Used for private workloads inside a VPC.

---

## Backends: Target Pools vs. Backend Services

### Target Pool (Legacy)
- A group of VM instances that receive traffic.
- Uses hashing of source/destination IP and port to balance traffic.
- Supports **only one health check** and requires all instances to be in the **same region**.

### Backend Service (Recommended)
- Defines load balancer behavior and backend configuration.
- Supports **IPv4/IPv6**, **multiple protocols**, and **fine-grained traffic control**.
- Allows **modern health checks** (TCP, SSL, HTTP, HTTPS, HTTP/2).
- Enables **failover policies** and **high availability**.

---

## Comparison Table

| Feature | Proxy NLB | Passthrough NLB |
|----------|------------|----------------|
| OSI Layer | 4 (Transport) | 4 (Transport) |
| Acts as Proxy | ✅ Yes | ❌ No |
| Connection Handling | Terminates client connection | Forwards packets as-is |
| Supported Protocols | TCP (optionally SSL/TLS) | TCP, UDP, ICMP, GRE, ESP |
| Scope | Global or Regional | Regional only |
| Preserves Client IP | ❌ No | ✅ Yes |
| Response Path | Through load balancer | Direct Server Return (DSR) |
| Best For | Cross-region, SSL control | Low latency, multi-protocol use |

---

## Summary

- **Proxy NLBs** provide advanced control and SSL offloading for TCP-based traffic.
- **Passthrough NLBs** provide ultra-fast, low-latency load balancing while preserving source IPs.
- For **HTTP(S)**, use an **Application Load Balancer** instead.

---

> **References:**
> - [Google Cloud Load Balancing overview](https://cloud.google.com/load-balancing/docs)
> - [External passthrough Network Load Balancer](https://cloud.google.com/load-balancing/docs/network)
> - [Proxy Network Load Balancer](https://cloud.google.com/load-balancing/docs/proxy-network)
