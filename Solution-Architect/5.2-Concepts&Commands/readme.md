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


