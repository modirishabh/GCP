# Google Cloud Networking – Summary Notes

## Introduction to GCP Networks
- GCP uses a global, software-defined network built on Google’s private fiber infrastructure.
- Resources are managed as services instead of hardware.
- The main networking service is **Virtual Private Cloud (VPC)**, which connects, isolates, and manages cloud resources.

## Key Networking Components
- **Project**: Main container for all Google Cloud resources, including networks.
- **Network**: Global, logical structure that connects resources across all regions.
- **Subnetwork**: Regional partition of a network. Multiple zones share one subnet.
- **IP Addresses**: Assigned internally or externally to VMs.
- **Routes**: Determine how traffic is directed.
- **Firewall Rules**: Control allowed or denied traffic.
- **Network Pricing**: Depends on egress/internet traffic, external IPs, and regions.

## Network Types
- **Default Network**: Auto-created in each project with ready subnets and firewall rules.
- **Auto Mode Network**: Automatically creates subnets for all regions with predefined IP ranges.
- **Custom Mode Network**: Fully user-defined control of subnets, regions, and IP ranges.
- Conversion from **auto → custom** is one-way.

## Regions and Zones
- **Region** = Geographical area containing multiple **zones** (data centers).
- Subnets are **regional**, meaning they span all zones within one region.
- **Global connectivity**—VMs in different regions can communicate via internal IPs on the same network.

## IP Addresses
- Each VM has:
  - **Internal IP**: Always assigned; used for private communication.
  - **External IP**: Optional; used for internet access.
- **Ephemeral IP**: Temporary, changes when VM restarts.
- **Static IP**: Reserved and persists across restarts.

## DNS in Google Cloud
- Two types: **Zonal DNS** and **Global DNS**.
- Internal DNS resolves hostnames within the same network.
- **Cloud DNS**: Managed, global DNS service with 100% uptime SLA.
- **Alias IP Ranges**: Assign multiple IPs to a single VM (useful for containers/services).

## Subnet Expansion
- You can expand subnets without downtime.
- Expanded range must not overlap with other subnets and must be larger (smaller prefix number).

## Routing
- **Default Routes** allow intra-network communication.
- **Custom Routes** override defaults for specific destinations.
- Routes + Firewall rules together determine allowed traffic.
- Routing tables are automatically managed per instance.

## Firewall Rules
- Protect VMs from unauthorized ingress (inbound) and egress (outbound) connections.
- **Parameters**:
  - Direction (ingress/egress)
  - Source or destination range
  - Protocol/port
  - Action (allow/deny)
  - Priority
- **Stateful rules**: Once a session is allowed, all return traffic is automatically allowed.
- Default behavior: `Deny all ingress`, `Allow all egress`.

## Network Pricing
- **Ingress (Inbound)** traffic is free.
- **Egress (Outbound)** costs vary:
  - Same zone (internal IP): free
  - Between zones/regions: charged
  - To external internet: charged
- **Static IPs** cost more when reserved but unused.
- Use the **GCP Pricing Calculator** for cost estimation.

## Common Network Designs
- **High Availability**: Deploy VMs in different zones of same region under one subnet.
- **Globalization**: Distribute resources across multiple regions for resiliency.
- **Global Load Balancer**: Routes users to the nearest region for lower latency.

---

**Crux:**  
GCP’s Virtual Private Cloud (VPC) gives globally scalable, software-defined networking with fine-grained control using subnetworks, IPs, routes, and firewalls across regions and zones—all while maintaining security and flexibility through managed services like Cloud DNS and detailed routing controls.
