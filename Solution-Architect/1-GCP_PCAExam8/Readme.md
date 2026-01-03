# Google Cloud Professional Cloud Architect - CI/CD & Networking Questions (141-150)

## Overview
This README covers core concepts from Google Cloud Professional Cloud Architect exam questions 141-150, focusing on VPC networking, storage, CI/CD pipelines, monitoring, security, and Kubernetes best practices. Each section explains the problem, key concepts, and best answer with citations.[web:1][web:21]

## Table of Contents
- [Question 141: Multi-VPC Communication](#question-141-multi-vpc-communication)
- [Question 142: Debian OS Patching](#question-142-debian-os-patching)
- [Question 143: Shared Filesystem](#question-143-shared-filesystem)
- [Question 144: Anthos Service Mesh](#question-144-anthos-service-mesh)
- [Question 145: Immutable Storage](#question-145-immutable-storage)
- [Question 146: CI/CD Pipeline](#question-146-cicd-pipeline)
- [Question 147: Autoscaling Incident](#question-147-autoscaling-incident)
- [Question 148: GKE Microservices](#question-148-gke-microservices)
- [Question 149: Shared VPC Teams](#question-149-shared-vpc-teams)
- [Question 150: Log Monitoring](#question-150-log-monitoring)

## Question 141: Multi-VPC Communication
**Problem**: Instance in VPC1 needs internal IP access to instances in VPC2/VPC3 (non-overlapping subnets).[web:1]

**Core Concepts**:
- VPC Peering: Non-transitive (VPC1↔VPC2↔VPC3 doesn't connect VPC1-VPC3)
- Cloud Router: Hybrid connectivity only
- **Multi-NIC VMs**: Attach multiple VPCs to one instance[web:2]

**Best Answer**: **B** Add NIC2 (VPC2 subnet) + NIC3 (VPC3 subnet) to Instance1 + firewall rules[web:1][web:2]

## Question 142: Debian OS Patching
**Problem**: Debian app needs automated OS updates with minimal manual work.[web:6]

**Core Concepts**:
- Instance templates: Manual recreation for updates
- Containers: App-level, not OS patches
- **OS Patch Management**: Automated VM patching service[web:6]

**Best Answer**: **B** Create Debian VM → configure app → enable OS Patch Management[web:6]

## Question 143: Shared Filesystem
**Problem**: Stateful workload needs POSIX shared FS at 100MB/s writes across instances.[web:7]

**Core Concepts**:
- Persistent Disk: Single VM read-write only
- gcsfuse: Non-POSIX, poor write performance
- **Filestore**: Multi-VM NFSv3 filesystem[web:7][web:15]

**Best Answer**: **C** Create Filestore instance → mount on all VMs[web:7]

## Question 144: Anthos Service Mesh
**Problem**: Identify slow microservice in Anthos cluster with Service Mesh.[web:8]

**Core Concepts**:
- Config Management: Policies/configs only
- **Service Mesh**: Telemetry (latency/errors) visualization[web:16]

**Best Answer**: **A** Use Service Mesh visualization dashboard[web:8][web:16]

## Question 145: Immutable Storage
**Problem**: Make Cloud Storage immutable for 5 years.[web:9]

**Core Concepts**:
- IAM: Access control only
- CMEK: Encryption, not immutability
- **Bucket Lock**: Retention policy + irreversible lock[web:9][web:17]

**Best Answer**: **A** 5-year retention policy → lock policy[web:9]

## Question 146: CI/CD Pipeline
**Problem**: Automated build/test/deploy from GitHub dev branch to GKE.[web:10]

**Core Concepts**:
- Git hooks: Decentralized, unreliable
- Vuln scanning ≠ functional tests
- **Cloud Build + Deploy Pipeline**: Separation of concerns[web:10][web:18]

**Best Answer**: **C** Cloud Build trigger → registry → deploy pipeline (restricted perms)[web:10]

## Question 147: Autoscaling Incident
**Problem**: App drops requests (max CPU), autoscaler at instance limit.[web:11]

**Core Concepts**:
- Restarts: Fail under sustained load
- Wrong metrics: Doesn't solve CPU issue
- **Max instances**: Primary scaling bottleneck[web:11]

**Best Answer**: **D** Increase autoscaling group max instances[web:11][web:19]

## Question 148: GKE Microservices
**Problem**: Internal microservices with replicas + stable addressing.[web:12]

**Core Concepts**:
- Raw Pods: No replica management
- Ingress: External traffic only
- **Deployment + Service**: Replicas + stable DNS[web:20]

**Best Answer**: **A** Deployments → Services → service DNS[web:12]

## Question 149: Shared VPC Teams
**Problem**: Networking team manages network; Devs manage sensitive VMs.[web:32]

**Core Concepts**:
- Single project: IAM escalation risk
- Standalone VPCs: Decentralized management
- **Shared VPC**: Host (network) + Service (compute) projects[web:32][web:40]

**Best Answer**: **C** Host project (Shared VPC, Network Admin) + Service project (Compute Admin)[web:32]

## Question 150: Log Monitoring
**Problem**: Real-time security alerts from Cloud Logs.[web:10]

**Core Concepts**:
- Cron/BigQuery/Storage: Batch/polling delays
- **Pub/Sub + Cloud Functions**: Event-driven, low-latency[web:10]

**Best Answer**: **C** Logs → Pub/Sub → Cloud Function[web:10]

## Key Takeaways
| Concept | Best Practice | Common Mistakes |
|---------|---------------|-----------------|
| VPC Connectivity | Multi-NIC for single VM multi-VPC | Peering (non-transitive), VPN |
| OS Management | Patch Management | Manual templates |
| Shared Storage | Filestore | PD (single attach), gcsfuse |
| Monitoring | Service Mesh viz | Config Management |
| Security | Bucket Lock | IAM only |
| CI/CD | Build + separate Deploy | Git hooks, single tool |
| Scaling | Increase max instances | Restarts |
| Kubernetes | Deployment + Service | Raw pods, Ingress internal |
| Organization | Shared VPC | Single project |
| Logging | Pub/Sub + Functions | Polling/batch [web:21]

## Study Tips
- Focus on **Google-recommended patterns** over complex workarounds
- Understand **service limitations** (PD single-attach, peering non-transitive)
- Practice **separation of concerns** (build vs deploy, network vs compute)
- Memorize **real-time vs batch** use cases[web:26]
# Google Cloud Professional Architect Exam: Questions 151-158 (Mountkirk Games Case Study)

## Overview
This README covers Google Cloud Professional Cloud Architect certification exam questions 151-158, focusing on secure VM access, IAM hierarchy, and the Mountkirk Games case study. Each section explains core concepts, why wrong answers fail, and validates the best solution with GCP best practices [web:1][web:7][web:11].

## Question 151: Secure SSH to Private Instances
**Scenario**: Instances lack public IPs, no VPN exists. Connect via SSH without violating security.

### Core Concepts
- **Cloud NAT**: Outbound-only; blocks inbound SSH [web:1].
- **TCP Proxy LB**: For load distribution, not single-instance SSH [web:2].
- **Bastion Host**: Needs public IP, creates attack surface [web:45][web:47].
- **IAP TCP Forwarding**: Identity-based SSH tunnels via Google's backbone, zero-trust [web:1][web:15][web:52].

### Best Answer: **C**
Configure IAP, grant `IAP-secured Tunnel User` role, use `gcloud compute ssh --tunnel-through-iap` [web:1][web:5].

**Why Others Fail**:
- A: NAT doesn't enable inbound [web:1].
- B: LB not for direct access [web:2].
- D: Bastion exposes new risks [web:45].

## Question 152: IAM Folder Permissions
**Scenario**: Dev group has Organization Project Owner; block Finance folder resource creation, allow Shopping.

### Core Concepts
- **Inheritance**: Additive top-down (Org → Folder → Project); no override without removal [web:6][web:11][web:48].
- **No Deny (exam era)**: Revoke broad role first [web:21].
- **Least Privilege**: Target permissions post-revocation [web:16].

### Best Answer: **C**
Remove Org Project Owner, grant Shopping folder only [web:6][web:11].

**Why Others Fail**:
- A/B: Viewer doesn't override Owner inheritance [web:48].
- D: Ignores Org role [web:6].

## Mountkirk Games Case Study Summary
Mobile gaming company migrates from physical servers/MySQL to GCP for autoscaling backends, NoSQL, real-time analytics on streaming metrics. Past scaling failures drive requirements [web:7][web:12][web:31].

**Key Needs**:
- Dynamic scaling on game activity.
- Real-time stream processing, late data handling, 10TB+ SQL analytics.
- Managed services only [web:7].

## Question 153: Testing Strategy Evolution
**Scenario**: Past scaling failures; new autoscaling backend needs scaled tests.

### Core Concepts
- **Beyond Unit/Infra Tests**: Focus load/stress at production scale [web:26].
- **Shared Responsibility**: App scaling, not GCP infra [web:26].

### Best Answer: **A**
Tests scale beyond prior approaches (performance/load tests) [web:26][web:31].

**Why Others Fail**:
- B: Unit tests essential [web:26].
- C: Post-prod risky [web:26].
- D: Customer tests app, not GCP [web:26].

## Question 154: Scalable Testing Environment
**Scenario**: Test new backends at production load economically pre-release.

### Core Concepts
- **Dynamic Validation**: Test env must autoscale like prod [web:31].
- **Avoid Static/Legacy**: Can't test scaling with fixed resources [web:46].

### Best Answer: **A**
Scalable GCP env simulates production load [web:31][web:46].

**Why Others Fail**:
- B: Legacy can't test scaling [web:31].
- C: Component-only, not system [web:31].
- D: Static misses autoscaling [web:46].

## Question 155: Continuous Delivery Pipeline
**Scenario**: Microservices, multi-region, single frontend IP, immutable artifacts, quick rollbacks.

### Core Concepts
- **Containers + Global LB**: GKE (Container Engine), Container Registry, HTTPS LB (global anycast IP) [web:27][web:32].
- **Immutable Deploys**: Container images [web:32].

### Best Answer: **C**
Container Registry, Container Engine, HTTPS Load Balancer [web:27][web:32].

**Why Others Fail**:
- A: Dataflow not hosting [web:27].
- B: Network LB regional [web:27].
- D: Serverless, not microservices [web:32].

## Question 156: Autoscaling Troubleshooting
**Scenario**: New feature spikes traffic → 503s, slow responses, no scaling.

### Core Concepts
- **503 = Overload**: Autoscaler blocked [web:8][web:30].
- **Quotas First**: CPU/VM limits halt new instances [web:25][web:30][web:49].

### Best Answer: **B**
Check project quotas (CPUs/VMs) [web:30][web:49].

**Why Others Fail**:
- A: DB outage ≠ partial 503s [web:8].
- C: Bug doesn't block scaling [web:30].
- D: Less likely than quota [web:49].

## Question 157: Environment Isolation
**Scenario**: Isolate dev/test (cross-access OK) from staging/prod; staging needs prod services.

### Core Concepts
- **Project = Strong Boundary**: IAM/billing isolation [web:9][web:14].
- **Networks Weak**: Same-project access bypasses [web:9].
- **Peering**: Staging-prod link [web:9].

### Best Answer: **D**
Separate projects: dev, staging, prod [web:9][web:14].

**Why Others Fail**:
- A: Staging+prod risky [web:9].
- B/C: Network ≠ IAM boundary [web:9].

## Question 158: Real-Time Analytics Platform
**Scenario**: Stream ingest/process (late data), 10TB+ SQL, user files, managed.

### Core Concepts
- **Pipeline**: Pub/Sub (ingest), Dataflow (process), BigQuery (analyze), Storage (raw) [web:28][web:33].
- **Avoid SQL DB**: Transactional, not analytics [web:33].

### Best Answer: **B**
Dataflow, Storage, Pub/Sub, BigQuery [web:28][web:33].

**Why Others Fail**:
- A/C/D/E: Cloud SQL or no BigQuery [web:33].

## Key Takeaways
- **IAP > Bastion**: Zero-trust SSH [web:1].
- **IAM**: Revoke high-level before targeting [web:11].
- **Mountkirk**: Containers (GKE), streaming (Pub/Sub+Dataflow+BQ), quotas, project isolation [web:7][web:31].
- **Testing**: Scale simulates prod failures [web:26].

**References**: Official GCP docs, case studies [web:1][web:7][web:11][web:28].

