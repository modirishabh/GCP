# Google Cloud Application Load Balancing Overview

## Introduction
**Application Load Balancing (ALB)** operates at **Layer 7 (Application Layer)** of the **OSI model**.  
This means it routes traffic based on the **content** of HTTP/HTTPS requests — such as URLs, headers, or cookies — instead of just network-level information like IP addresses or ports.

For example, an ALB can send:
- `example.com/api` → Backend A  
- `example.com/images` → Backend B  

---

## Supported Backends
Google Cloud Application Load Balancer can distribute HTTP(S) traffic to various backends, including:

- **Compute Engine** (virtual machines)
- **Google Kubernetes Engine (GKE)** (containers)
- **Cloud Storage** (static sites or files)
- **Cloud Run** (serverless applications)
- **External backends** (connected via internet or hybrid networking)

---

## Deployment Modes
### 1. External Application Load Balancer
Handles **internet-facing** traffic.

- Uses **Google Front Ends (GFEs)** — Google’s globally distributed proxies that terminate traffic close to users for low latency.
- Can be deployed as:
  - **Global external ALB** (multi-region)
  - **Regional external ALB** (within one region)
  - **Classic ALB** (legacy type)
- Uses the **open-source Envoy proxy** for advanced traffic management.

### 2. Internal Application Load Balancer
Handles **private traffic** within a Virtual Private Cloud (VPC) network.  
Useful for internal microservices or backend-only applications.

---

## ALB Architecture
Here’s how a request flows through an external Application Load Balancer:

1. **External Forwarding Rule**  
   - Defines an **external IP**, **port**, and **target HTTP(S) proxy**.
   - Clients connect to this rule.

2. **Target HTTP(S) Proxy**
   - Evaluates the request using a **URL map**.
   - Routes traffic to the appropriate backend based on the URL.
   - Can use **SSL certificates** to terminate HTTPS connections.

3. **Backend Service**
   - Distributes requests to **healthy backends** (VMs, containers, etc.).
   - Contains:
     - **Health checks** — ensure traffic is sent only to healthy instances.
     - **Session affinity** — optionally bind clients to the same backend.
     - **Timeout settings** — default is 30 seconds before considering a failure.

4. **Backends**
   - Typically **instance groups** (managed or unmanaged).
   - Each backend has:
     - **Balancing mode** (CPU utilization or requests per second).
     - **Capacity scaler** (percentage of backend capacity to use).

---

## Example: Balancing Mode and Capacity
| Setting | Description | Example |
|----------|--------------|---------|
| **Balancing Mode** | Defines when an instance is considered full. | `80% CPU utilization` |
| **Capacity Scaler** | Controls how much of total backend capacity is used. | `50%` means only half the backend capacity is used. |

**Example configuration:**
- Balancing mode = 80% CPU utilization  
- Capacity = 50% → Effective max load = 40% of total CPU

---

## Load Balancing Behavior
- Uses **Round-Robin** by default to evenly distribute requests.
- **Session Affinity** overrides round-robin to stick client requests to one backend.
- **Health checks** ensure only healthy instances get new requests.
- **Configuration changes** (like adding/removing instances) take a few minutes to propagate globally.

---

## Summary
Google Cloud's Application Load Balancer:
- Operates at **Layer 7** of the OSI model for intelligent routing.
- Uses **GFEs** and **Envoy** for fast, secure, global network performance.
- Supports multiple backend types and offers advanced features like:
  - Health checks  
  - Session affinity  
  - Capacity scaling  
  - Global request routing  

---
# Google Cloud Application Load Balancer (HTTPS)

Application Load Balancers (ALBs) in Google Cloud handle HTTP and HTTPS traffic with key differences for secure connections.[web:15]

## HTTPS Configuration

HTTPS ALBs use a **target HTTPS proxy** instead of an HTTP proxy, requiring at least one signed SSL certificate on the proxy. Client SSL sessions terminate at the load balancer itself, offloading decryption from backends. Up to 15 SSL certificates can be configured per proxy, each created as an SSL certificate resource specifically for load balancing proxies.[web:15]

## QUIC Support

ALBs support **QUIC**, a transport protocol that:
- Speeds up client connection initiation
- Reduces head-of-line blocking in multiplexed streams
- Enables connection migration during IP address changes[web:15]

## Backend Options

### Backend Buckets
Integrate **Cloud Storage** for static content delivery via URL maps:
- Dynamic requests → Backend services
- Static paths (e.g., `/love-to-fetch/`) → Specific regional buckets (e.g., `europe-north`, `us-east`)[web:15]

### Network Endpoint Groups (NEGs)
Group endpoints for granular traffic distribution:
- **Containers** and VM services
- Works with load balancers and Traffic Director[web:15]

## NEG Types

| Type | Description | Endpoints |
|------|-------------|-----------|
| **Zonal NEG** | Compute Engine VMs/services in a zone | IP or IP:port |
| **Internet NEG** | External Google Cloud endpoints | FQDN:port or IP:port |
| **Hybrid NEG** | External Traffic Director services | Service references |
| **Serverless NEG** | Cloud Run, App Engine, Cloud Functions | No direct endpoints (regional services) |[web:15]

## Key Differences: HTTP vs HTTPS ALB

| Feature | HTTP ALB | HTTPS ALB |
|---------|----------|-----------|
| **Target Proxy** | Target HTTP Proxy | **Target HTTPS Proxy** |
| **SSL Certificates** | Not required | **Required (1-15 certificates)** |
| **SSL Termination** | N/A | **At Load Balancer** |
| **QUIC Support** | Supported | **Supported** |
| **Backend Types** | Services, Buckets, NEGs | **Services, Buckets, NEGs** |[web:15]

## References
- [Google Cloud Load Balancing Overview](https://cloud.google.com/load-balancing/docs/)
- [HTTP(S) Load Balancing Concepts](https://cloud.google.com/load-balancing/docs/https)
