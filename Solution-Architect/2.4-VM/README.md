# Google Cloud VMs – Quick Notes

## What is a VM?

- A virtual machine (VM) is a software-based computer with virtual CPU, memory, disk, and IP address running on Google Cloud Compute Engine.[web:12][web:29]  
- VMs are used to run most server-style workloads (web apps, databases, batch jobs) in the cloud.[web:12][web:38]  

## How to create VMs

- You can create VMs using:
  - Cloud Console  
  - gcloud command-line  
  - REST API[web:29][web:32]  
- The Console can show the equivalent gcloud/REST commands to help you automate configurations.[web:29][web:33]  

## Main VM options

- **CPU & Memory (Machine type)**  
  - Choose number of vCPUs and RAM using predefined or custom machine types.[web:1][web:29]  
  - Custom machine types let you fine-tune vCPU and memory within documented limits.[web:1][web:11]  

- **Storage**  
  - Persistent disks: Standard (HDD) or SSD; same max size, different performance and cost.[web:29][web:35]  
  - Local SSD: very fast, attached to the host, but data lasts only while the VM runs.[web:29][web:12]  

- **Networking**  
  - Internal and external IPs, firewall rules, and optional HTTP(S) or network load balancers.[web:15][web:12]  

## Machine families (high level)

- **General-purpose**: Balanced price and performance for most workloads.[web:1][web:11]  
- **Compute-optimized**: Highest performance per core for CPU-heavy workloads (HPC, gaming, media processing).[web:1][web:35]  
- **Memory-optimized**: Very large RAM for in-memory databases and analytics.[web:1][web:11]  
- **Accelerator-optimized**: VMs with GPUs for ML and high-performance computing.[web:1][web:12]  

## VM access and lifecycle

- Linux VMs: connect with SSH; Windows VMs: connect with RDP using generated credentials.[web:29][web:15]  
- Typical states: provisioning → staging → running → stopping/terminated; some changes (like machine type) require the VM to be stopped.[web:29][web:1]  
- Availability policies control maintenance behavior (live migrate vs terminate) and automatic restart after failure.[web:29][web:15]  

## OS patch management

- OS patch management helps apply and schedule OS and software patches across many VMs.[web:7][web:13]  
- Provides patch compliance reports, patch jobs, flexible schedules, and pre/post patch scripts to keep VMs secure and up to date.[web:7][web:10]  

## Cost basics

- When a VM is stopped/terminated, you no longer pay for vCPU and memory, but you still pay for attached disks and reserved external IPs.[web:29][web:5]  
