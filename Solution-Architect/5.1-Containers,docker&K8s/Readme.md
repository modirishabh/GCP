# Understanding Containers, Docker, and Kubernetes

## What Are Containers?
Traditionally, applications were deployed on physical machines, requiring setup and maintenance. Virtualization improved efficiency by allowing multiple virtual machines (VMs) on one physical host using a hypervisor. However, VMs still carried full operating systems, making them heavy and slow to start.

Containers solve this by isolating applications and their dependencies in lightweight environments. Instead of virtualizing hardware, containers virtualize the user space above the kernel. They start quickly, use fewer resources, and allow consistent behavior across environments.

Containers let developers package code and dependencies together, ensuring applications run reliably anywhere. They support efficient scaling and enable microservices-based architectures.

## Container Images and How They Work
A container image is a package that includes an application and its dependencies. A container is a running instance of that image. Developers use images to ensure software runs identically on different systems.

Docker is an open-source tool for creating and running containers, while Kubernetes handles orchestration at scale.

### Key Linux Technologies
Containers rely on:
### 1. **Processes** 
- Every app runs as a Linux *process* with private memory
- Start/stop in milliseconds ⚡
- **Analogy**: Each kid gets their own bedroom (no mess sharing)

### 2. **Namespaces** 
- Hides system parts from container
- **PID**: Sees only its processes  
- **Mount**: Sees only its files
- **Network**: Own IP addresses
- **Analogy**: Each kid in separate house (can't see neighbors)

### 3. **cgroups (Control Groups)**
- Hard limits on resources
- CPU: max 2 cores
- RAM: max 4GB
- Disk: 100MB/s
- **Analogy**: Strict budget - "Only $20 for toys!"

### 4. **Union File Systems**
- Stores files in stacked, read-only layers
- Each Dockerfile command = 1 layer
- Running container adds 1 thin, temporary writable layer on top
- **Analogy:** Like transparent overhead sheets – stack them, write on top sheet only


### Container Image Layers
A container image is built in layers using a **Dockerfile**, which lists the build instructions:
1. **FROM** — Defines the base image (e.g., Ubuntu).
2. **COPY** — Adds files.
3. **RUN** — Builds the application.
4. **CMD** — Specifies the startup command.

Modern practice uses **multi-stage builds**: one container builds the app, and another runs it. The writable top layer of a running container is temporary, while the base image remains unchanged.

Images are stored in registries like Google’s Artifact Registry or Docker Hub. Private images can be secured using Identity and Access Management (IAM).

## Building Containers with Cloud Build
**Cloud Build** automates container creation. It integrates with Cloud IAM and supports repositories like GitHub and Bitbucket. Each build step runs in a container and can handle tasks such as fetching dependencies, compiling code, or running tests. Finished images can be deployed to **Kubernetes Engine**, **App Engine**, or **Cloud Run**.

## What Is Kubernetes?
Kubernetes is an open-source platform for managing and orchestrating containers at scale. It automates deployment, scaling, and management across clusters of machines.

### Key Features
- **Declarative Configuration** — Define your system’s desired state; Kubernetes keeps it that way.
- **Scalability** — Automatically adjusts workloads based on usage.
- **Workload Types** — Supports stateless, stateful, and batch jobs.
- **Resource Control** — Ensures optimal CPU and memory usage.
- **Extensibility** — Allows custom resources and plugins.
- **Portability** — Runs on-premises or in any cloud provider.

## Google Kubernetes Engine (GKE)
**GKE** is Google’s managed Kubernetes service. It handles cluster setup, scaling, node management, and upgrades automatically.

Key features include:
- **GKE Autopilot** — Fully manages cluster configuration.
- **Auto-upgrade and auto-repair** — Keeps clusters healthy and up to date.
- **Google Cloud Integration** — Works with Cloud Build, IAM, Observability, VPCs, and the Cloud Console.
- **Simplified Dashboard** — Offers visibility and control without manual maintenance.

GKE helps developers focus on building applications rather than managing infrastructure.

---
