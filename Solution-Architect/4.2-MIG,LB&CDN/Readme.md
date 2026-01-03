# GCP Load Balancing & MIGs - Practitioner Notes

## Managed Instance Groups (MIGs)
Managed instance groups manage identical VM instances as a single entity using instance templates. MIGs provide autoscaling based on CPU utilization, load balancing capacity, or monitoring metrics, and queue-based workload like Pub/Sub or schedule such as start-time, duration and recurrence.
Autohealing recreates unhealthy instances using health checks. If an instance in the group stops, crashes or is deleted by an action other than the instance group commands, the managed instance group automatically recreates the instance so it can resume its processing tasks.[web:1]

**Key MIG Features:**
- Rolling updates via new templates
- Automatic recreation of failed instances (same name/template)
- Load balancing integration
- Stateful IP preservation for databases/legacy apps
- Supports stateless (web/batch) and stateful workloads



# Google Cloud Health Checks (Simple Explanation)

## What is a Health Check?

Health checks are small automatic tests that Google Cloud runs to see if your VM or backend is alive and working.

- If the backend is **healthy**, Google Cloud **sends traffic** to it.
- If the backend is **unhealthy**, Google Cloud **stops sending traffic** to it.

These checks are used internally by load balancers and managed instance groups to decide which instances should receive requests.

## How a Health Check Works

You configure how Google Cloud should test your backend:

- **Protocol**: HTTP, HTTPS, TCP, etc.
- **Port**: For example `80`, `443`, `8080`.
- **Path (for HTTP/HTTPS)**: For example `/healthz`.

Google Cloud then regularly sends requests to the configured IP:port and:

- Marks the instance **Healthy** → traffic is allowed.
- Marks the instance **Unhealthy** → traffic is stopped.

## Key Terms

### Check Interval

- Defines **how often** the health check runs.
- Example: Every **5 seconds**, send a health check request.

### Timeout

- Defines **how long to wait** for a response before considering that attempt as failed.
- Example: Wait up to **5 seconds**; if no response → this attempt = **failed**.

### Healthy Threshold

- Defines how many **successful health checks in a row** are required to mark an instance as **healthy**.
- Example: Healthy threshold = **2** → need **2 successful** checks in a row.

### Unhealthy Threshold

- Defines how many **failed health checks in a row** are required to mark an instance as **unhealthy**.
- Example: Unhealthy threshold = **2** → need **2 failed** checks in a row.

# Stateful IP Addresses in Managed Instance Groups (MIGs)

## 🎯 Problem It Solves
**Normal MIG**: VM restarts → **New random IPs** → Apps break (DB connections, configs fail)

**Stateful IP**: VM restarts → **Same IPs preserved** → Apps continue seamlessly

## 🔄 Real-World Example

**Before Stateful IP (Problem):**

**MIG: web-server-mig (3 VMs)**

**VM1: 10.128.1.10 ← Running your app**

    **↓ Autoheal triggers** 
    
**VM1 recreated: 10.128.1.25 ← NEW IP! App breaks!**

**After Stateful IP (Fixed):**

**VM1: 10.128.1.10 ← Running your app**

     **↓ Autoheal triggers**
     
**VM1 recreated: 10.128.1.10 ← SAME IP! App continues working!**


## How It Works (Step-by-Step)
1. **Enable Stateful Policy** on existing MIG
   gcloud compute instance-groups managed update web-server-mig \
     --stateful-network-interface=network-tier=PREMIUM,disable-external=true

2. **Magic happens automatically**:
   - All existing VMs' ephemeral IPs → Promoted to **static/reserved IPs**
   - New VMs get the **exact same IPs** as their predecessors

3. **VM restarts** → Gets back **same IP + same disks** (if configured)


# OSI Model Layers - Quick Reference Table

## OSI 7 Layers Summary

| Layer | Name | Job | Real Example | Protocols/Devices |
|-------|------|-----|--------------|-------------------|
| **7** | **Application** | User apps talk to network | Web browser, email client | **HTTP, HTTPS, FTP, SMTP** |
| **6** | **Presentation** | Translate/encrypt data | JPEG → bytes, SSL encryption | **SSL/TLS, JPEG, ASCII** |
| **5** | **Session** | Manage connections | Login session, video call | **APIs, RPC, NetBIOS** |
| **4** | **Transport** | End-to-end delivery | TCP (reliable), UDP (fast) | **TCP, UDP** |
| **3** | **Network** | Route across networks | IP addresses, router paths | **IP, ICMP, Routers** |
| **2** | **Data Link** | Local network delivery | MAC addresses, frame check | **Ethernet, Switches, WiFi** |
| **1** | **Physical** | Bits over wire | Cable signals, light pulses | **Cables, Hubs, Fiber** |


## Application Load Balancer (Layer 7)
External Application Load Balancers distribute HTTP(S) traffic globally using Google Front Ends (GFEs) or Envoy proxies to backends like Compute Engine, GKE, Cloud Storage, Cloud Run.[web:21] Uses single anycast IP with URL maps for content-based routing (e.g., /video → video backend). Backend services include health checks, session affinity, timeouts; supports cross-region failover.[web:21]

**Architecture Components:**
| Component | Purpose |
|-----------|---------|
| Forwarding Rule | External IP/port → HTTP(S) proxy |
| URL Map | Routes based on URL headers |
| Backend Service | Health checks, load distribution to instance groups/NEGs |
| Backend Bucket | Static content from Cloud Storage |

HTTPS requires SSL certificates on target proxy; supports QUIC protocol.

## Network Load Balancers (Layer 4)
Proxy Network Load Balancers terminate TCP/SSL traffic at load balancer (GFEs/Envoy), suitable for non-HTTP workloads. Passthrough Network Load Balancers preserve client source IP with direct server return (DSR), support TCP/UDP/ESP/ICMP; regional using Maglev/Andromeda.[web:21]

**Comparison:**
| Type | Proxy | Passthrough |
|------|--------|-------------|
| Client IP | Not preserved | Preserved |
| Protocols | TCP/SSL | TCP/UDP/ESP/GRE/ICMP |
| Scope | Global/Regional | Regional only |
| Use Case | Reverse proxy, on-prem | DSR, multi-protocol |

## Internal Load Balancing
Internal Application Load Balancers (Envoy-based) handle HTTP(S) behind private IP; regional or cross-region modes. Internal passthrough Network Load Balancers (Andromeda) for TCP/UDP behind VPC IP, low latency via software-defined routing without proxy termination.[web:21]

## Cloud CDN Integration
Enable on Application Load Balancer backend service for edge caching (90+ global PoPs). Cache modes: USE_ORIGIN_HEADERS (respects origin TTL), CACHE_ALL_STATIC (static content), FORCE_CACHE_ALL (overrides origin). Logs show hit/miss status.[web:21]

**Key Benefits for GCP Practitioner:**
- MIGs + Load Balancers = HA scalable apps
- Choose L7 (ALB) for HTTP routing, L4 (NLB) for protocols/IP preservation
- Regional MIGs > Zonal for production
