# Azure Cloud & DevOps Learning Journey - Days 41 to 43: Advanced Networking Mastery

Welcome to my repository tracking my DevOps and Cloud learning path! This section documents **Days 41 through 43**, focusing heavily on **Azure Networking**, infrastructure provisioning, security layers, custom routing mechanisms, and real-time troubleshooting.

---

## 🚀 Key Learning Modules Overview

### 1. Virtual Networks (VNets) & Multi-Tier Subnetting (Day 41)
* **Architecture:** Provisioned `vnet-multitier-prod` with a Classless Inter-Domain Routing (CIDR) block of `10.1.0.0/16`.
* **Subnet Segmentation:** Designed isolated tiers for production workloads:
  * **Web Tier (`subnet-web`):** `10.1.1.0/24` (Public-facing components).
  * **App Tier (`subnet-app`):** `10.1.2.0/24` (Application logic layers).
  * **Data Tier (`subnet-data`):** `10.1.3.0/24` (Database and secure storage).

### 2. Network Security Groups (NSGs) & Traffic Control (Day 42)
* **Virtual Firewalls:** Implemented NSG rules (`nsg-web-tier`) to evaluate inbound and outbound traffic based on priority.
* **Granular Scope:** Learned how security rules can be applied at two critical layers:
  * **Subnet Level:** Securing an entire range of resources.
  * **NIC (Network Interface Card) Level:** Direct fine-grained control attached straight to a specific Virtual Machine (e.g., `vm-devops-app017`).

### 3. Advanced Routing, UDRs & Troubleshooting (Day 43)
* **User Defined Routes (UDRs):** Bypassed or overridden Azure system routes by creating custom Route Tables (`rt-prod-routing`).
* **Route Precedence:** Mastered the **"Most Specific Route Wins"** rule for packet forwarding.
* **Network Virtual Appliances (NVAs):** Configured next-hop types (`Virtual appliance`) to route traffic dynamically through security appliances or firewalls.

---

## 🛠️ Real-Time Troubleshooting Log (Day 43 Labs)

While validating routing behavior in the Azure Portal, I encountered and resolved a couple of critical real-time edge cases:

1. **Effective Routes Failure (`VM is Stopped`):**
   * *Issue:* Attempting to view **Effective routes** threw an error: *"Failed to retrieve effective routes because the virtual machine 'vm-devops-app01' is not running."*
   * *Resolution:* Azure requires the target virtual machine to be in a **Running** state to evaluate active route tables on its network interface. Starting the VM instantly resolved the data retrieval.
   * 
2. **Subnet & VNet Route Mismatch:**
   * *Issue:* The custom UDR (`Route-To-Internet`) wasn't appearing under the VM's effective routes because the VM was deployed in `vnet-southindia-1` (`snet-southindia-1`), whereas the route table was originally mapped to `subnet-app` in `vnet-multitier-prod`.
   * *Resolution:* Re-associated the Route Table to target the correct active subnet (`snet-southindia-1`), which successfully propagated the custom rules.

---

## 📖 Glossary of Abbreviations

| Abbreviation | Technical Term | Context in Azure |
| :--- | :--- | :--- |
| **VNet** | Virtual Network | Core private network boundary in Azure. |
| **VM** | Virtual Machine | Software-defined computing instance. |
| **NSG** | Network Security Group | Stateful virtual firewall rule engine. |
| **NIC** | Network Interface Card | Virtual adapter connecting a VM to a VNet. |
| **UDR** | User Defined Route | Custom administrative routing table override. |
| **NVA** | Network Virtual Appliance | Third-party routing/firewall virtual appliance. |
| **CIDR** | Classless Inter-Domain Routing | IP address range allocation format (`/24`, `/16`). |
| **SSH** | Secure Shell | Cryptographic network protocol on Port 22. |

---

## 📂 Hands-On Lab Summary

* **Lab 1:** Created multi-tier production VNets and structured subnets for isolation.
* **Lab 2:** Deployed and associated NSGs directly to individual VM Network Interfaces (NICs).
* **Lab 3:** Created Route Tables, configured custom default route overrides (`0.0.0.0/0`), and verified results using the portal's **Effective routes** diagnostic feature.

---
*Maintained as part of my continuous **Learning in Public** DevOps journey.*
