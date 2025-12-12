# GCP Resource Management Summary

## Resource Management Overview
- GCP resources are billable; effective management helps control costs.
- Access control is managed using **IAM**, and **quotas** limit consumption.
- Default quotas can be increased by request but act as a safeguard against overuse.
- Key focus areas: **Resource Manager**, **Quotas**, **Labels**, and **Billing**.

## Resource Hierarchy
- **Resource Manager** enables hierarchical management using **Organization → Folder → Project → Resource**.
- **IAM policies** are inherited from parent to child, while **billing** is accumulated from bottom (resources) up.
- A **project**:
  - Groups related resources,
  - Enables billing,
  - Manages permissions and APIs.
- Each project has:
  - **Project ID** – unique identifier (used by APIs),
  - **Project Number** – system-generated,
  - **Project Name** – human-readable.
- Resources are categorized as:
  - **Global** – e.g., networks, images,
  - **Regional** – e.g., IP addresses,
  - **Zonal** – e.g., VM instances, disks.

## Quotas
- Quotas limit how many resources can be used or the rate of API requests.
- Examples:
  - 15 VPC networks per project.
  - 24 CPUs per region.
  - API rate limits (e.g., 5 actions/sec for Cloud Spanner).
- Purpose of quotas:
  - Prevent runaway resource usage or billing spikes.
  - Encourage proper sizing and cost control.
  - Quotas can be adjusted via the **Cloud Console Quotas page**.
- Note: Quotas don’t guarantee availability (e.g., a region may run out of SSDs).

## Labels
- **Labels** are key-value pairs for organizing resources.
  - Example: `env:production`, `team:marketing`
- Uses:
  - Group and filter resources.
  - Enable cost tracking and reporting.
  - Run bulk operations.
- Each resource can have up to 64 labels.
- **Labels ≠ Network Tags**:
  - Labels = organization and billing.
  - Tags = networking (firewalls, routes).

## Billing and Budgets
- Each project links to one **billing account**.
- Create **budgets** to track spend and receive alerts (e.g., at 50%, 90%, 100% spend).
- Alerts can be sent via:
  - **Email** (to billing admins),
  - **Pub/Sub** (for automation with Cloud Functions).
- Best practices:
  - Label all resources for cost attribution.
  - Export billing data to **BigQuery** for analysis.
  - Use **Looker Studio** to visualize spend over time.
  - Optimize costs using insights—e.g., move VMs closer to users or cache with **Cloud CDN**.

## Key Takeaways
- Structure your resources logically within projects.
- Use IAM for access, quotas for limits, and labels for organization.
- Monitor and manage billing actively with budgets and data analysis tools.

