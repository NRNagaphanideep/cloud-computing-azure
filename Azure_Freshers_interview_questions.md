zure Fresher Interview Questions & Answers (Master Edition)

This comprehensive guide covers all Microsoft Azure topics from your curriculum tailored for entry-level (fresher) DevOps and Cloud Engineer interviews, including full forms, core definitions, theoretical concepts, and practical scenarios.

---

## 1. Global Infrastructure & Core Governance

### Q1: What is the full form of RG, VNet, NSG, UDR, and RBAC, and what do they mean?
* **RG:** Resource Group. A logical container into which Azure resources (like VMs, databases, storage) are deployed and managed.
* **VNet:** Virtual Network. Fundamental building block for your private network in Azure, enabling secure communication between VMs and other Azure services.
* **NSG:** Network Security Group. Contains security rules that allow or deny inbound network traffic to, or outbound network traffic from, several types of Azure resources.
* **UDR:** User Defined Route. Custom routes created by administrators to override Azure's default system routing or route traffic through Network Virtual Appliances (NVAs).
* **RBAC:** Role-Based Access Control. Authorization system built on Azure Resource Manager (ARM) that provides fine-grained access management of Azure resources.

### Q2: Explain the Azure Management Hierarchy.
* **Hierarchy levels (top to bottom):** Management Groups $\rightarrow$ Subscriptions $\rightarrow$ Resource Groups $\rightarrow$ Resources.
* **Purpose:** Helps organize resources logically, enforce governance policies, and control billing boundaries across large enterprise environments.

### Q3: Why do we use Azure Resource Tags?
* **Tags:** Name/value pairs attached to resources to categorize them by metadata (e.g., `Environment: Production`, `CostCenter: 401`, `Owner: DevOps`). They assist tremendously with cost tracking, billing allocation, and automation scripts.

---

## 2. Networking & Traffic Control

### Q4: What is the difference between public IP and private IP addresses in an Azure VNet?
* **Private IP:** Enables Azure resources to communicate with other resources inside the same VNet or connected virtual networks securely.
* **Public IP:** Enables internet-facing resources to communicate with inbound/outbound internet traffic.

### Q5: Scenario: A database VM in a private subnet cannot talk to a web server VM in another subnet. What troubleshooting steps would you take?
1. **Check NSG Rules:** Ensure the NSG associated with the database subnet/NIC allows inbound traffic from the web server's IP/subnet on the correct port (e.g., port 3306 for MySQL).
2. **Check UDR / Route Tables:** Verify if custom route tables are misrouting traffic away.
3. **Check OS Firewall:** Ensure the guest operating system firewall (e.g., `ufw` or `iptables`) permits the connection.

---

## 3. Identity & Secrets Management

### Q6: What is Microsoft Entra ID (formerly Azure Active Directory)?
* **Microsoft Entra ID:** Azure's cloud-based identity and access management (IAM) service, which helps employees sign in and access resources across external and internal applications.

### Q7: What is a Managed Identity in Azure, and why is it preferred over storing connection strings?
* **Managed Identity:** Automatically managed identity in Microsoft Entra ID for applications to authenticate to services supporting Entra authentication without embedding credentials in code.
* **Why preferred:** Eliminates hardcoded passwords or connection strings in configuration files, preventing credential leaks and removing manual credential rotation overhead.

### Q8: What is Azure Key Vault?
* **Azure Key Vault:** A cloud service for securely storing secrets (API keys, passwords), encryption keys, and TLS/SSL certificates. It integrates with Azure RBAC and access policies for fine-grained retrieval control.

---

## 4. Azure Storage Services

### Q9: What are the three core storage patterns in Azure Storage?
1. **Blob Storage:** Massively scalable object storage for unstructured data (text, binaries, logs, media).
2. **Managed Disks:** Block-level storage volumes attached to Azure VMs, functioning like physical server hard drives (`OS Disk` and `Data Disks`).
3. **Azure Files:** Fully managed file shares in the cloud accessible via industry-standard SMB and NFS protocols for cross-VM file sharing.

### Q10: What are Azure Storage Access Tiers, and when should you use them?
* **Hot Tier:** For frequently accessed data (higher storage cost, lowest access cost).
* **Cool Tier:** For infrequently stored data retained for at least 30 days.
* **Cold Tier / Archive Tier:** For long-term backups and data rarely accessed, offering minimal storage cost with high retrieval latency.
