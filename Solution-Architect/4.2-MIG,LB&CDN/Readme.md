# GCP Load Balancing & MIGs - Practitioner Notes

## Managed Instance Groups (MIGs)
Managed instance groups manage identical VM instances as a single entity using instance templates. Regional MIGs spread instances across multiple zones for higher availability and zonal failure protection.[web:1][web:20] MIGs provide autoscaling based on CPU utilization, load balancing capacity, or monitoring metrics, and autohealing recreates unhealthy instances using health checks.[web:1]

**Key MIG Features:**
- Rolling updates via new templates
- Automatic recreation of failed instances (same name/template)
- Load balancing integration
- Stateful IP preservation for databases/legacy apps
- Supports stateless (web/batch) and stateful workloads

**Creation Steps:**
1. Create instance template (VM config snapshot)
2. Define MIG: name, zone(s), template, autoscaling policy, health check
3. Manager auto-populates instances

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
