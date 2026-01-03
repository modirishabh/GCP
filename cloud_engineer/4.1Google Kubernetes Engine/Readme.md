# Google Kubernetes Engine (GKE) in Google Cloud Platform (GCP)

Welcome back, Gurus. In this lesson, we delve into Google Kubernetes Engine (GKE), learning how it provides a robust platform for deploying and managing containerized applications.

## Introduction to Google Kubernetes Engine (GKE)

GKE offers a managed Kubernetes service that simplifies the deployment, scaling, and management of applications within Google Cloud.

## How Does GKE Work?

GKE is built upon the powerful Kubernetes architecture, which is divided into two core components:

- **Control Plane**: The brain of the cluster, managing the worker nodes and ensuring the cluster is functioning correctly.
- **Nodes**: The workhorses of the cluster, where your applications are run within pods.

## Types of Workloads Deployed with GKE

GKE supports a variety of application types, each suited for different use cases:

- **Stateless Applications**: Ideal for services where data persistence is not required between sessions.
- **Stateful Applications**: Necessary for services that maintain data state across sessions.
- **Batch Jobs**: Suitable for processing tasks that run to completion without ongoing user interaction.
- **Daemon Jobs**: Used for running background tasks across all or some nodes within a cluster.

# Google Cloud Platform (GCP) - GKE Cluster Configuration

## Overview

This project outlines the use of **Google Kubernetes Engine (GKE)** on **Google Cloud Platform (GCP)** with a focus on:

- Choosing between **Autopilot** and **Standard** clusters
- Registering clusters into a **Fleet** for unified management
- Configuring a **cost-optimized cluster**

---

## GKE Cluster Types

### 🧭 Autopilot Clusters

Autopilot clusters are recommended for most production workloads. These clusters:

- Are pre-configured with Kubernetes best practices
- Optimize for **security**, **scalability**, and **cost**
- Reduce operational overhead

Use Autopilot when:
- You want to reduce manual configuration
- Your workloads fit into common production patterns

### ⚙️ Standard Clusters

Standard clusters give you more control over configuration:

- You manage node configuration and upgrades
- Google manages the control plane
- Useful when Autopilot restrictions don't suit your workload

Use Standard when:
- You require custom security or network configurations
- You need advanced workload tuning

---

## 🚀 Fleet Registration (Multi-cluster Management)

> **New Feature**

A **Fleet** allows you to group Kubernetes clusters for unified management and policy enforcement.

**Benefits:**
- Logical grouping of clusters
- Enables **multi-cluster** capabilities
- Simplifies application of consistent policies and configurations

To register a cluster to a Fleet:
- Use `gcloud container hub memberships register`
- Or use the GCP Console under **GKE > Fleet > Register Cluster**

---

## 💰 Cost-Optimized Cluster Settings

Below is an example of a cost-efficient GKE cluster setup:

```yaml
Cluster name:                cost-optimized-cluster-1
Cluster zone:                us-central1-c
Cluster size:                user-selected
Machine type:                auto-selected (based on desired cluster size)
Cluster autoscaling:         Enabled
Autoscaling profile:         Optimize utilization
Vertical pod autoscaling:    Enabled
Node auto-provisioning:      Enabled
GKE cost allocation:         Enabled
