y 48: Cloud Identity & IAM (Azure Identity, RBAC, Managed Identities, & AWS IAM Instance Profiles) - Comprehensive Notes

## 1. Microsoft Entra ID (Azure Active Directory)
* **What is it?** Microsoft Entra ID is a cloud-based central Identity and Access Management (IAM) service used to manage user identities and control access to cloud resources.
* **Core Components:**
  * **Users:** Individual accounts representing people in an organization (e.g., employees, administrators).
  * **Groups:** Collections of users managed together to simplify permission assignments (e.g., DevOps-Team, DB-Admins).
  * **Service Principals & App Registrations:** Digital identities given to non-human entities (applications, services, automated CI/CD pipelines like GitHub Actions or Jenkins) to authenticate and access cloud resources.

---

## 2. Azure RBAC (Role-Based Access Control)
* **Definition:** A fine-grained access management system that determines *who* (Security Principal) has *what* permissions (*Role Definition*) over *where* (*Scope*).
* **Key Built-in Roles:**
  * **Owner:** Full access to all resources, including the ability to assign roles to other users.
  * **Contributor:** Can create, manage, and delete resources, but cannot assign roles to others.
  * **Reader:** Read-only access to view resources without making any modifications.

---

## 3. Scope & Principle of Least Privilege
* **Scope Levels (Hierarchy from broad to specific):**
  1. **Management Group:** Top-level container managing multiple subscriptions.
  2. **Subscription:** The billing and resource boundary account.
  3. **Resource Group (RG):** A logical container grouping related resources for a specific project/application.
  4. **Resource:** A single individual item (e.g., a specific VM or Storage Account).
* **Principle of Least Privilege:**
  * Giving users, groups, or workloads **only the minimum necessary permissions** required to perform their jobs, avoiding over-privileged accounts (e.g., assigning a Reader role instead of Owner or Contributor when write access isn't needed).

---

## 4. Cloud Identity: Passwordless Workload Authentication
* **The Problem (Hard-coded Credentials):** Embedding passwords, connection strings, or access keys directly inside source code or configuration files poses a major security risk if exposed (e.g., leaked via GitHub).
* **The Solution (Passwordless Workload Authentication):** Granting servers or application code a direct cloud-native digital identity so they can authenticate securely without storing static secrets.

---

## 5. Managed Identities & IAM Instance Profiles
* **Azure Managed Identities:**
  * Allows Azure Virtual Machines (VMs) and services to authenticate to other Azure services (like Azure Blob Storage or SQL Databases) without managing credentials.
  * **System-assigned:** Created directly with the VM; deleted automatically when the VM is deleted.
  * **User-assigned:** Created as an independent standalone resource and can be attached to multiple VMs.
* **AWS IAM Instance Profiles:**
  * The AWS equivalent container that passes an **IAM Role** (containing specific permissions like S3 access) to an **AWS EC2 instance**, allowing applications running on the instance to access AWS services securely without hard-coded access keys.

---

## 6. Hands-On Lab Summary: Azure Identity & RBAC
1. **Created a User:** Provisioned `DevOps Test User` in Microsoft Entra ID.
2. **Created a Resource Group:** Set up `rg-identity-test-01` in Azure.
3. **Assigned RBAC Role:** Applied the **Reader** role to `DevOps Test User` scoped strictly to `rg-identity-test-01` following the Principle of Least Privilege.
4. **Verified Access:** Tested permissions using an incognito session—verified that the user could view resources within the group but received an **"Authorization Failed / Access Denied"** error when attempting unauthorized modifications or creation.
