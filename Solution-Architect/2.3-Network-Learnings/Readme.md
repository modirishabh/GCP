## VM placement and connectivity

- VM region/zone determine which subnet the NIC uses and therefore the internal IP range. [web:2][web:5]  
- Internal IP connectivity:
  - Works between VMs in the same VPC across any regions/zones (subject to firewall). [web:2][web:20]  
  - Does not work between VMs in different VPCs unless you configure peering/VPN/Interconnect and appropriate routes/firewall. [web:2][web:5]  

- External connectivity:
  - If a VM has an external IP and firewall allows it, you can reach it from the internet (e.g., SSH/ICMP). [web:5][web:10]  
  - Pinging external IPs only depends on firewall and external IP, not on VPC or zone, as long as routes exist. [web:5][web:20]  

## Default and custom firewall patterns

- Common “default-style” rules:
  - allow-icmp: ICMP from anywhere.  
  - allow-ssh: tcp:22 from anywhere.  
  - allow-rdp: tcp:3389 from anywhere.  
  - allow-internal / allow-custom: all protocols within the VPC’s internal CIDR. [web:5][web:20]  

- For IAP SSH access:
  - Use a firewall rule allowing tcp:22 from 35.235.240.0/20 (IAP’s IP range). [web:6][web:20]  

## Instances without external IPs

- A VM without an external IP:
  - Cannot be reached directly from the internet. [web:5][web:10]  
  - Can be reached from Cloud Shell or other clients using Identity‑Aware Proxy (IAP) TCP forwarding plus an SSH-allow firewall rule from the IAP IP range. [web:6][web:20]  
  - Can reach other internal resources in the VPC via private IP, subject to firewall and routes. [web:2][web:5]  

## Private Google Access (PGA)

- Private Google Access lets VMs without external IPs reach Google APIs and services using the VPC’s default route and Google’s public API IPs. [web:6][web:12]  
- PGA is enabled per subnet (On/Off). It only affects VMs that have no external IP attached. [web:6][web:18]  
- When PGA is ON for a subnet:
  - VMs with only private IPs can access Google APIs like Cloud Storage, Pub/Sub, Cloud Run over the default 0.0.0.0/0 route via the internet gateway, even though they themselves have no public IP. [web:6][web:12]  
  - It does not provide general internet egress; it is limited to Google APIs/services. [web:12][web:18]  

## Cloud NAT (for internet egress without public IP)

- Cloud NAT is a managed regional NAT gateway that provides outbound internet access to private resources (no external IP) while keeping them unreachable from the public internet. [web:7][web:10]  
- Works at VPC level:
  - You associate a NAT gateway with one network and region, then select which subnets or IP ranges use it. [web:10][web:16]  
  - Outbound connections from private VMs are translated to one or more public IPs owned by the NAT gateway; responses flow back through NAT. [web:7][web:13]  

- PGA vs Cloud NAT:
  - PGA: access to Google APIs/services only; configured at subnet level; no external IPs on VMs required. [web:6][web:12]  
  - Cloud NAT: general outbound internet access (OS updates, third‑party repos, etc.) for private VMs; configured as regional gateway with router; still no external IPs on VMs. [web:7][web:10]  
  - In many designs, you enable both: PGA for Google APIs, Cloud NAT for broader internet egress. [web:12][web:19]  

## Exam-relevant key points

- Need a VPC to create VMs; no VPC → no routes/firewall → no VM creation. [web:2][web:20]  
- Auto mode: quick start, limited control; Custom mode: full IP/subnet control; recommended for production. [web:2][web:3]  
- Internal IP communication is VPC‑scoped, not zone‑scoped. Same VPC across regions works; different VPC in same zone does not, unless you interconnect. [web:2][web:20]  
- IAP + firewall from 35.235.240.0/20 enables SSH/RDP to private VMs without bastion hosts. [web:6][web:20]  
- Private Google Access: private VMs → Google APIs (only). [web:6][web:12]  
- Cloud NAT: private VMs → broader internet egress, still no inbound exposure. [web:7][web:10]  
