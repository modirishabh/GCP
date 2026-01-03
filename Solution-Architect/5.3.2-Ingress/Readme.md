# Kubernetes Ingress Overview

## What is Ingress?

**Ingress** is a Kubernetes object that acts like a smart front door to your cluster. It controls how external HTTP/HTTPS traffic reaches internal **Services** and **Pods**.

### Key Concept

An **Ingress** is a Kubernetes API resource that defines routing rules for external requests (usually HTTP/HTTPS) going to Services within the cluster.

Instead of giving every Service its own external IP or load balancer, Ingress lets multiple Services share a single entry point (one IP or hostname) using host- and path-based routing rules.

---

## Ingress vs Ingress Controller

| Component | Description |
|------------|-------------|
| **Ingress** | Stores routing rules for external-to-internal traffic. |
| **Ingress Controller** | Enforces those rules by configuring an actual load balancer or reverse proxy. |

Examples of ingress controllers include **NGINX**, **GCE**, and **Traefik**.

The ingress controller:
- Watches Kubernetes ingress resources in the cluster.
- Configures the underlying load balancer or proxy.
- Ensures traffic flows to the correct backend Services and Pods.

---

## How Ingress Works (Step-by-Step)

1. **Create Ingress Resource**  
   Define routing rules, e.g.,  
   `example.com/api → service: api-service port: 80`.

2. **Controller Programs Load Balancer**  
   The ingress controller detects your Ingress and configures a load balancer or reverse proxy with your host/path rules.

3. **Traffic Flow**  
   - Client sends request to external IP or hostname.  
   - Load balancer evaluates the request against Ingress rules.  
   - Traffic gets forwarded to the correct Service and underlying Pods.

---

## Ingress in Google Cloud (GKE)

In a **Google Kubernetes Engine (GKE)** environment:

- When you create an Ingress, Google Cloud automatically provisions an **HTTP(S) Load Balancer**.  
- It also creates **Network Endpoint Groups (NEGs)** in each zone where your cluster runs.
- NEGs represent the backend endpoints — the Pods exposed via Services.
- Once provisioning completes, the load balancer receives an **external IP** for public access.

---

## Backend Service and Health Status

- The **Google HTTP(S) Load Balancer** includes a **backend service** that defines how traffic is distributed across NEGs/endpoints.
- This backend service controls:
  - Load balancing policy  
  - Timeouts  
  - Traffic distribution behavior

Each backend service has an associated **health check** to ensure reliability:
- Only healthy Pods receive traffic.  
- If health checks fail, endpoints are marked **unhealthy** and removed from active rotation.

---

## Summary

Using Ingress and an Ingress Controller helps you:
- Simplify external access to multiple services.
- Centralize routing and SSL termination.
- Automatically manage Google Cloud load balancing and health checks.

