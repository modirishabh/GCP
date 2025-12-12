# Google Cloud Identity and Access Management (IAM)

## 1. What is IAM?
Identity and Access Management (IAM) defines **who can do what on which resource** in Google Cloud.

- **Who**: Users, groups, or applications.
- **What**: Actions or privileges (e.g., read, write, delete).
- **Resource**: A Google Cloud service or asset (e.g., Compute Engine, Storage).

Example: Assigning a user the “Compute Viewer” role gives them read-only access to view compute resources.

---

## 2. Resource Hierarchy
Google Cloud resources are structured hierarchically:

1. **Organization** – Root node representing your company. Roles granted here apply to all resources below.
2. **Folders** – Represent departments or teams. Roles are inherited by all resources inside.
3. **Projects** – Represent a trust boundary within an organization.
4. **Resources** – Individual services (VMs, buckets, etc.) with one parent project.

---

## 3. IAM Roles
Roles define **what actions are allowed**.

### Types of Roles
- **Basic Roles**: Broad access across projects.
  - Owner (full control)
  - Editor (modify/delete resources)
  - Viewer (read-only access)
- **Predefined Roles**: Granular roles for specific services (e.g., Compute Admin, Network Admin, Storage Admin).
- **Custom Roles**: User-defined roles with specific permissions following the principle of least privilege.

---

## 4. Creating a Custom Role
Steps in the Console:
1. Go to **IAM & Admin → Roles**.
2. Click **Create Role** and define name, ID, and launch stage.
3. Add necessary permissions (e.g., start/stop compute instances).
4. Save and assign the role.

Custom roles are not maintained by Google and must be updated manually when new permissions are added.

---

## 5. Members (Identities)
IAM defines **who** can access resources.

### Types of Members
- **Google Accounts** – Individual users.
- **Service Accounts** – Managed identities for applications.
- **Google Groups** – Collections of users or service accounts.
- **Google Workspace / Cloud Identity Domains** – Represent all users in an organization.

Policies bind members to roles. Permissions are inherited along the resource hierarchy.

---

## 6. Policies and Permissions
- **Policies**: Bind members to roles for a resource.
- **Inheritance**: Permissions flow downwards (from organization → folders → projects → resources).
- **Least Privilege**: Always grant the minimal access required.

**Allow Policies**: Grant access.  
**Deny Policies**: Explicitly block access.  
**Conditions**: Allow access only when specific criteria are met (e.g., time or location).

---

## 7. Service Accounts
Used by applications or services instead of human users.

- Identified by an email address.
- Types:
  - Default (created automatically)
  - Custom (user-created)
  - Google-managed API accounts
- Permissions defined via IAM roles.
- Can authenticate securely without embedding credentials.
- Keys can be **Google-managed** or **user-managed** (user-managed ones require manual security and rotation).

---

## 8. Organization Restrictions
This feature prevents data exfiltration and phishing risks by allowing access **only to authorized Google Cloud organizations**.

- Enforced via managed devices and proxy configurations.
- Inspects requests for organization restriction headers.

---

## 9. Best Practices
- Use the **principle of least privilege**.
- Group related resources within **projects**.
- Assign roles to **groups**, not individual users.
- Rotate and audit **service account keys**.
- Use **Cloud IAM Recommender** to remove excess permissions.
- Protect apps using **Cloud Identity-Aware Proxy (IAP)** for application-level access control.

---

## Summary
IAM in Google Cloud ensures that:
- The right users have the right access to the right resources.
- Access is managed efficiently, securely, and in accordance with organizational needs.

