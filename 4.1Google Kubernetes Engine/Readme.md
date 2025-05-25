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

## Key Takeaways for the GCP Cloud Associate Exam

- GKE leverages Kubernetes to provide a scalable and fully-managed environment for containerized apps.
- Familiarize yourself with the Kubernetes control plane, nodes, and pod structure.
- Understand different workload types and their respective characteristics within a GKE cluster.

Thank you for exploring GKE with me. Stay tuned as we continue our GCP journey and prepare for deploying our first GKE cluster in the next lesson. I'll see you in the next video!
