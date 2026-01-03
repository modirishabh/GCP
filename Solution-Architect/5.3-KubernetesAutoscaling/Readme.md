# Kubernetes Autoscaling Overview

This document explains the different types of autoscaling available in Kubernetes — **Horizontal Pod Autoscaler (HPA)**, **Vertical Pod Autoscaler (VPA)**, **Cluster Autoscaler (CA)**, and **Node Auto Provisioning (NAP)** — along with configuration examples and key details for each component.

---

## Horizontal Pod Autoscaling

**Horizontal Pod Autoscaling (HPA)** changes the shape of your Kubernetes workload by automatically increasing or decreasing the number of pods in response to:

- CPU or memory consumption.
- Custom metrics reported from within Kubernetes.
- External metrics from sources outside of your cluster.

HPA helps ensure your application can scale horizontally to handle changing load efficiently.

---

## Vertical Pod Autoscaling

**Vertical Pod Autoscaling (VPA)** adjusts your container’s CPU and memory **requests and limits**. This removes the need to manually specify these values. The autoscaler can:

- Recommend CPU and memory requests/limits.
- Automatically update resources based on historical usage.

### Example: Vertical Pod Autoscaler Manifest

```
cat << EOF > hello-vpa.yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
name: hello-server-vpa
spec:
targetRef:
apiVersion: "apps/v1"
kind: Deployment
name: hello-server
**updatePolicy:
updateMode: "Off"**
EOF
```
This manifest creates a **Vertical Pod Autoscaler** targeting the `hello-server` deployment with an **Update Policy** of `Off`.

### Update Policies

| Policy  | Description |
|----------|-------------|
| **Off** | Generates recommendations based on historical data. Apply manually. |
| **Initial** | Applies recommendations once when creating new pods. Pod sizes remain fixed afterward. |
| **Auto** | Automatically resizes pods regularly to match recommendation sizes. |

### Applying and Viewing the VPA
When you apply above VPA, after deploy you describe the pods you will found **Container Recommendations**

### Container Recommendations

At the end of the output, you'll see CPU and memory recommendation types:

| Recommendation | Description |
|----------------|-------------|
| **Lower Bound** | Minimum threshold. Below this, the VPA may scale down. |
| **Target** | Preferred CPU/memory values to use during resizing. |
| **Uncapped Target** | Target utilization if no limits are set. |
| **Upper Bound** | Maximum threshold. Above this, the VPA may scale up. |

---

## Cluster Autoscaler

The **Cluster Autoscaler (CA)** adds or removes nodes in your cluster based on resource demand:

- **Scale up:** Adds nodes when pending pods can’t be scheduled due to insufficient resources.  
- **Scale down:** Removes underutilized nodes to save cost.

### Autoscaling Profiles

| Profile | Description |
|----------|-------------|
| **Balanced** | Default mode. Balances utilization and availability. |
| **Optimize-utilization** | Prioritizes utilization over spare capacity by removing nodes aggressively. Ideal for batch workloads. |

### Pod Disruption Budgets (PDB)

**Pod Disruption Budgets** define how Kubernetes handles disruptions pod removals, running out of resources, etc. In PDBs, you can specify :

- `maxUnavailable`: Maximum pods that can be unavailable.
- `minAvailable`: Minimum pods that must remain available.

Well-configured PDBs allow **Cluster Autoscaler** to safely reschedule pods, enabling smoother scale-down operations.

It’s important to note that, while Cluster Autoscaler removed an unnecessary node, Vertical Pod Autoscaling and Horizontal Pod Autoscaling helped reduce enough CPU demand so that the node was no longer needed. Combining these tools is a great way to optimize your overall costs and resource usage.

So, the cluster autoscaler helps add and remove nodes in response to pods needing to be scheduled. However, GKE specifically has another feature to scale vertically, called node auto-provisioning
---

## Node Auto Provisioning

**Node Auto Provisioning (NAP)** extends the Cluster Autoscaler by automatically creating **new node pools** when needed.

- Without NAP: The Cluster Autoscaler only adds nodes to existing node pools.Without node auto provisioning, the cluster autoscaler will only be creating new nodes in the node pools you've specified, meaning the new nodes will be the same machine type as the other nodes in that pool.  
- With NAP: It dynamically creates new node pools sized to meet the workload’s resource requirements.

This feature helps optimize node types for varying workload needs without manual intervention.

---

## Summary

- **HPA**: Scales pods horizontally based on resource usage or custom metrics.  
- **VPA**: Adjusts CPU/memory requests vertically per pod.  
- **CA**: Scales the cluster size by adding/removing nodes.  
- **NAP**: Creates new node pools dynamically to meet demand.

Combining these autoscalers helps achieve **cost efficiency, availability, and optimal resource utilization** across your Kubernetes environment.

