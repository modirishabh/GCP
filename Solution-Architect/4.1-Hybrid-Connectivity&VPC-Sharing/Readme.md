# GCP Hybrid Connectivity & VPC Sharing
## What is Cloud VPN?

**Cloud VPN** creates encrypted IPsec tunnels over public internet to connect on-premises networks to Google Cloud VPC. Provides secure private IP access (RFC1918) between environments.[web:1]

**Use Case**: Low-medium bandwidth hybrid connectivity, encrypted data transfer, temporary/migration scenarios, no direct physical connection available.


## Cloud VPN Types

Google Cloud offers **Classic VPN** (99.9% SLA, single interface, static routing) and **HA VPN** (99.99% SLA, dual interfaces, dynamic BGP routing only).[web:1][web:20]

**Classic VPN** uses one external IP, supports IKEv1/IKEv2, site-to-site tunnels over public internet (MTU ≤1460 bytes), suitable for low-volume encrypted connections.[web:1]

**HA VPN** auto-provisions two external IPs from unique pools, requires 2-4 tunnels for SLA, supports active/active or active/passive routing via Cloud Router BGP (link-local 169.254.x.x addresses).[web:1]



## What is BGP?
## BGP is a routing protocol that lets different networks tell each other which IP ranges they can reach and which path is best to get there. In HA VPN, BGP is used between your on‑premises router and Cloud Router so routes can be exchanged and updated automatically instead of being configured manually 
BGP stands for **Border Gateway Protocol**, the main routing protocol used on the internet to exchange “reachability” information between networks (autonomous systems).​

Each BGP router advertises which IP prefixes it can reach and receives advertisements from its peers, then chooses the best path based on policies and attributes (like path length, preference, etc.).​

In simple terms: BGP is like a dynamic map between networks that constantly updates which roads exist and which road is currently best.

**Why HA VPN requires BGP**
HA VPN tunnels must use dynamic routing, and on Google Cloud that means using Cloud Router with BGP to exchange routes with your on‑premises VPN device.​

BGP allows the VPN endpoints to automatically learn new subnets on either side (for example, when you add a new VPC subnet or a new on‑premises LAN) without changing tunnel configuration.​

This dynamic behavior is critical for high availability: if one tunnel or path fails, BGP detects the change and shifts traffic to the remaining healthy tunnel(s).


**How BGP works in HA VPN (with example)**
Imagine you have:

On‑prem network: 10.0.0.0/16 behind your data center router

GCP VPC network: 10.10.0.0/16 with an HA VPN gateway and a Cloud Router

**In HA VPN:**

You create 2 or 4 tunnels between your HA VPN gateway and your on‑premises VPN router (for example, tunnel A and tunnel B).

Cloud Router runs BGP sessions over these tunnels to your on‑prem router (using link‑local IPs like 169.254.x.x).​

Cloud Router advertises 10.10.0.0/16 to on‑prem; your router advertises 10.0.0.0/16 to Cloud Router.

Both sides now have dynamic routes:

GCP knows “to reach 10.0.0.0/16, send traffic through HA VPN tunnels”.

On‑prem knows “to reach 10.10.0.0/16, send traffic through HA VPN tunnels”.

If tunnel A fails (for example, link issue or router problem), BGP notices that session is down and stops advertising routes via that tunnel, so traffic is automatically shifted to tunnel B without you changing routes manually.​

**Benefits of BGP in HA VPN**
Automatic route updates: New subnets on either side are learned and advertised via BGP; no manual route updates needed.

High availability and failover: Multiple tunnels can be active; BGP withdraws routes on failed paths and keeps healthy ones, supporting 99.99% SLA for HA VPN when configured correctly.​

Load sharing: By tuning BGP route priorities/weights, HA VPN can be configured as active/active (load share across tunnels) or active/passive (one tunnel used, the other standby).​

So, BGP in HA VPN is what makes the connection dynamic, resilient, and scalable, instead of a fragile VPN with static, hard‑coded routes.





## Interconnect Options

|Type|Connection|Capacity|SLA|Layer|Use Case|
|----|----------|--------|----|------|--------|
|Dedicated|Direct physical to Google|10/100Gbps (up to 200Gbps)|99.9/99.99%|L2/L3|High bandwidth, colocation facility access[web:21]|
|Partner|Via service provider|50Mbps-50Gbps|99.9/99.99%|L2/L3|No direct Google access[web:21]|
|Cross-Cloud|Google ↔ AWS/Azure/OCI/Alibaba|10/100Gbps|N/A|Multi-cloud integration[web:21]|

All Interconnects provide RFC1918 private IP access (no public internet traversal).[web:21]
# Peering Services

### Direct Peering
- Connects your network directly with Google at **Google Edge Points of Presence (PoPs)**.
- Supports **10 Gbps+ bandwidth**, uses **public IPs** to reach Google services.
- **No SLA**, typically used for large, direct access to Google services.

### Carrier Peering
- Used when Direct Peering is not available.
- Connectivity through **Google-approved carriers**.
- Also **no SLA**, provides **public IP access** to Google services.

### Interconnect vs Peering
- **Interconnect**: Private network connectivity → uses **RFC1918 (private IPs)**, includes **SLA**.
- **Peering**: Public network access to Google APIs/services → uses **public IPs**, **no SLA**.

---

# Decision Flow (Choosing the Right Option)

1. **Need Google APIs/Workspace (public services)?** → Use **Direct or Carrier Peering**.  
2. **Need to connect to another cloud provider?** → Use **Cross-Cloud Interconnect**.  
3. **Need high bandwidth and colocated setup?** → Choose **Dedicated Interconnect**.  
4. **Need secure/encrypted or lower bandwidth connection?** → Use **Cloud VPN**.  
5. **If using Partner facility for connectivity?** → Choose **Partner Interconnect**.

**VPN over Interconnect:** Adds **IPsec encryption** layer on top of Interconnect for secure data transfer.

---

# VPC Sharing vs VPC Peering

| Feature | Shared VPC | VPC Network Peering |
|----------|-------------|--------------------|
| **Scope** | Within same organization (host & service projects). | Between same or different organizations/projects. |
| **Admin Model** | Centralized: Host project controls routes & firewalls. | Decentralized: Each VPC manages its own network. |
| **Transitivity** | Yes, via host VPC. | No, only direct peer-to-peer. |
| **Use Case** | Central IT managing multiple projects under one network. | Individual projects needing isolated connection. |

### Shared VPC
- A **host project** shares its **subnets** with **service projects**.  
- Service projects use **host’s IPs, routes, and firewalls**.  
- Ideal for centralized management and security.

### VPC Peering
- Connects **two VPC networks** privately using **RFC1918 IPs**.
- Requires **non-overlapping IP ranges** and **mutual configuration**.
- Great for **peer-to-peer** communication between separate networks.

---

