# Kubernetes Fundamentals

Kubernetes uses an object model and declarative management to orchestrate containers reliably. Objects represent cluster state, while the control plane continuously reconciles desired and actual states via a watch loop.  

This document explains these fundamentals, including **Pods** and **Google Kubernetes Engine (GKE)** simplifications.

---

## Core Concepts

Kubernetes objects are persistent entities that define the cluster state.  

- **spec**: Describes the desired configuration (e.g., three nginx Pods).  
- **status**: Reflects the current state, continuously updated by the control plane.  

**Declarative management** means you declare the goal, and Kubernetes ensures the system converges toward it.  
Controllers monitor the API server, detect changes, and act to maintain consistency — for example, launching missing Pods to match the declared spec.  

The **watch loop** continuously compares desired and actual states, performing necessary actions to reconcile them.

---

## Pods Explained

**Pods** are the smallest deployable units in Kubernetes.  

Each Pod can host one or more tightly coupled containers that share:  
- **Networking**: Single IP address, allowing localhost communication.  
- **Storage volumes**: Shared persistent data volumes.  
- **Resources**: CPU and memory allocations.  

Pods are **ephemeral** — if one fails, controllers (like **Deployments**) automatically recreate or reschedule new ones, maintaining the desired number of instances (e.g., three `nginx` Pods).

---

## Control Plane Components

The control plane manages the cluster through several key components:

- **kube-apiserver**: The primary API endpoint for all operations and commands (e.g., via `kubectl`). Handles authentication, validation, and communication.  
- **etcd**: A distributed key-value store maintaining the entire cluster’s configuration and state data.  
- **kube-scheduler**: Assigns unscheduled Pods to nodes based on resource availability and scheduling constraints.  
- **kube-controller-manager**: Runs controllers (like the Deployment controller) to reconcile desired vs. actual states.

**Nodes** run:  
- **kubelet**: The agent responsible for starting and managing Pods using a container runtime (e.g., `containerd`).  
- **kube-proxy**: Manages networking and load balancing across Pods.  
- **Pods**: The application workloads themselves.

---

## GKE Differences

**Google Kubernetes Engine (GKE)** simplifies Kubernetes management by handling much of the control plane for you.  

Key distinctions:  
- The **control plane** is managed and hidden; only the API endpoint is exposed.  
- **Autopilot Mode** automates node provisioning, scaling, upgrades, and security — ideal for managed operations.  
- **Standard Mode** gives more flexibility and control, requiring manual node management.

---







# Kubernetes Default Namespaces Reference

Kubernetes automatically creates four default namespaces in every cluster. These provide essential system functions and a default space for user workloads.

## Namespace Summary

| Namespace       | Purpose                                              | Typical Contents                                   | Usage Notes                                      |
|-----------------|------------------------------------------------------|----------------------------------------------------|--------------------------------------------------|
| **default**     | Default namespace for user workloads when no namespace is specified | User deployments, services, pods                   | Always exists; use `-n default` explicitly if needed |
| **kube-node-lease** | Manages node heartbeats via lease objects            | Lease resources for node health checks             | Internal; updated every 10s by kubelet           |
| **kube-public** | Resources visible/readable by all users cluster-wide | ConfigMaps like cluster-info                       | Public read access; no auth required             |
| **kube-system** | Kubernetes system components                         | CoreDNS, kube-proxy, metrics-server, controller-manager | System-only; avoid modifying                     |


- **Cordon the existing node pool**: This operation marks the nodes in the existing node pool (node) as unschedulable. Kubernetes stops scheduling new Pods to these nodes once you mark them as unschedulable.
- **Drain the existing node pool**: This operation evicts the workloads running on the nodes of the existing node pool (node) gracefully.
- **Pod Affinity** is used to ensure pods are scheduled on the same node, 
- **Pod Anti Affinity** is used to ensure pods are not scheduled on the same no


# Access cluster credentials
gcloud container clusters get-credentials hello-demo-cluster --zone "ZONE" 

# Resize existing node pool
gcloud container clusters resize hello-demo-cluster --node-pool my-node-pool --num-nodes 3 --zone "ZONE" 

# Create new optimized node pool
gcloud container node-pools create larger-pool --cluster=hello-demo-cluster --machine-type=e2-standard-2 --num-nodes=1 --zone="ZONE" 

# Delete node pool after migration
gcloud container node-pools delete my-node-pool --cluster hello-demo-cluster --zone "ZONE" 

# Create regional cluster
gcloud container clusters create regional-demo --region="REGION" --num-nodes=1 
# Scale deployments
kubectl scale deployment hello-server --replicas=2 

# Node pool migration workflow
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=my-node-pool -o=name); do kubectl cordon "$node"; done 
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=my-node-pool -o=name); do kubectl drain --force --ignore-daemonsets --delete-emptydir-data --grace-period=10 "$node"; d

# Pod management
kubectl get pods -o=wide 
kubectl apply -f pod-1.yaml 
kubectl get pod pod-1 pod-2 --output wide
kubectl exec -it pod-1 -- sh 
kubectl delete pod pod-2 


