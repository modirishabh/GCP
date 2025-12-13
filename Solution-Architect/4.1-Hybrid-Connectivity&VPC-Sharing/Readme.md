# GCP Hybrid Connectivity & VPC Sharing
## What is Cloud VPN?

**Cloud VPN** creates encrypted IPsec tunnels over public internet to connect on-premises networks to Google Cloud VPC. Provides secure private IP access (RFC1918) between environments.[web:1]

**Use Case**: Low-medium bandwidth hybrid connectivity, encrypted data transfer, temporary/migration scenarios, no direct physical connection available.


## Cloud VPN Types

Google Cloud offers **Classic VPN** (99.9% SLA, single interface, static routing) and **HA VPN** (99.99% SLA, dual interfaces, dynamic BGP routing only).[web:1][web:20]

**Classic VPN** uses one external IP, supports IKEv1/IKEv2, site-to-site tunnels over public internet (MTU ≤1460 bytes), suitable for low-volume encrypted connections.[web:1]

**HA VPN** auto-provisions two external IPs from unique pools, requires 2-4 tunnels for SLA, supports active/active or active/passive routing via Cloud Router BGP (link-local 169.254.x.x addresses).[web:1]

## Interconnect Options

|Type|Connection|Capacity|SLA|Layer|Use Case|
|----|----------|--------|----|------|--------|
|Dedicated|Direct physical to Google|10/100Gbps (up to 200Gbps)|99.9/99.99%|L2/L3|High bandwidth, colocation facility access[web:21]|
|Partner|Via service provider|50Mbps-50Gbps|99.9/99.99%|L2/L3|No direct Google access[web:21]|
|Cross-Cloud|Google ↔ AWS/Azure/OCI/Alibaba|10/100Gbps|N/A|Multi-cloud integration[web:21]|

All Interconnects provide RFC1918 private IP access (no public internet traversal).[web:21]

## Peering Services

**Direct Peering**: 10Gbps+ settlement-free BGP peering at Google Edge PoPs (no SLA, public IP access to Google services).[web:7]

**Carrier Peering**: Via supported carriers when Direct Peering requirements unmet (no SLA).[web:12]

**Interconnect vs Peering**: Interconnect = private RFC1918 VPC access + SLA; Peering = public Google services access, no SLA.[web:21]

## Decision Flow

Need Google APIs/Workspace? → Peering (Direct/Carrier)
↓ No
Connect to other cloud? → Cross-Cloud Interconnect
↓ No
High bandwidth + colocation? → Dedicated Interconnect
↓ No
Encrypted/low bandwidth? → Cloud VPN
Else → Partner Interconnect


**VPN over Interconnect**: Adds IPsec encryption to Interconnect traffic.[web:20]

## VPC Sharing Comparison

|Feature|Shared VPC|VPC Network Peering|
|-------|-----------|------------------|
|Scope|Same organization, host/service projects|Same/different projects/organizations[web:8]|
|Admin Model|Centralized (host project controls network/firewalls)|Decentralized (each VPC independent)[web:8]|
|Transitivity|Yes (via host VPC)|No (direct peers only)[web:8]|
|Use Case|Central IT team manages multi-project networking|Peer-to-peer project isolation[web:8]|

**Shared VPC**: Host project shares subnets; service projects use host's IPs/routes/firewalls.[web:8]

**VPC Peering**: Private RFC1918 connectivity, requires non-overlapping CIDRs, mutual peering configuration.[web:13]
