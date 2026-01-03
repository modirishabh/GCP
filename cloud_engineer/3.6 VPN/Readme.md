# VPN Connections in Google Cloud Platform (GCP)

Welcome back, Gurus. This lesson focuses on VPN connections within GCP, a fundamental concept for securely extending your on-premises network to the cloud.

## Understanding VPN

A Virtual Private Network (VPN) enables a secure and encrypted connection over the internet, allowing for safe data transmission and privacy protection.

## How Does VPN Work?

VPN connections are facilitated by gateways that establish a secure tunnel through which data can pass safely:

- **Peer VPN Gateway**: Your on-premises or other cloud network's VPN gateway.
- **Cloud VPN Gateway**: GCP's VPN gateway that connects to your peer gateway.

## Types of VPNs in Google Cloud

GCP offers two types of VPN connections:

- **High Availability VPN (HA VPN)**: Ensures higher uptime with redundant interfaces and IP addresses, meeting a strong service level agreement.
- **Classic VPN**: A basic VPN setup with a single tunnel and IP address, suitable for less critical connections.

## Key Takeaways for the GCP Cloud Associate Exam

- VPNs provide a secure method to connect on-premises networks to GCP VPCs.
- Differentiate between HA VPN and Classic VPN, understanding the benefits and use cases of each.
- Each VPN setup requires a cloud VPN gateway and a customer gateway to establish the tunnel.

Thank you for participating in this VPN lesson. As we proceed with our exam preparation, we'll explore more networking services and configurations. I'll see you in the next video!
