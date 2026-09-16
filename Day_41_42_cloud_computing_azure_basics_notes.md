
 Training - Day 41 & Day 42: Comprehensive Guide to Azure Infrastructure & Virtual Machines

---

## **Day 41: Introduction to Cloud Computing, Azure Architecture & Resource Management**

### **1. Cloud Computing Fundamentals**
* **What is Cloud Computing?** 
  Cloud computing is the on-demand delivery of compute power, database storage, applications, and other IT resources through a cloud services platform via the internet with pay-as-you-go pricing.
* **Deployment Models:**
  * **Public Cloud:** Owned and operated by a third-party cloud service provider (e.g., Microsoft Azure, AWS, Google Cloud), delivering computing resources over the public internet.
  * **Private Cloud:** Cloud infrastructure exclusively used by a single business or organization.
  * **Hybrid Cloud:** Combines public and private clouds, bound together by technology that allows data and applications to be shared.
* **Cloud Service Models:**
  * **IaaS (Infrastructure as a Service):** Provides fundamental computing resources like physical or virtual servers, storage, and networking (e.g., Azure Virtual Machines).
  * **PaaS (Platform as Service):** Supplies an on-demand environment for developing, testing, delivering, and managing software applications (e.g., Azure App Service).
  * **SaaS (Software as a Service):** Delivers software applications over the internet, on-demand, typically on a subscription basis (e.g., Microsoft 365).

---

### **2. Microsoft Azure Core Architecture**
* **Regions:** A set of datacenters deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network.
* **Availability Zones (AZs):** Physically separate locations within an Azure region. Each Availability Zone is made up of one or more datacenters equipped with independent power, cooling, and networking.
* **Resource Groups (RGs):** A logical container in Azure where related resources for an Azure solution are held. Resource groups act as a management boundary for access control, policies, and billing.

---

### **3. Step-by-Step Guide: Creating a Resource Group in Azure Portal**

1. **Log in to Azure Portal:** Open [portal.azure.com](https://portal.azure.com) using your credentials.
2. **Navigate to Resource Groups:** In the search bar at the top, type **Resource groups** and click on it from the service results.
3. **Create a New Resource Group:**
   * Click on the **+ Create** button.
   * **Subscription:** Select your subscription (e.g., *Azure subscription 1* / Free Trial).
   * **Resource group name:** Enter a unique and descriptive name (e.g., `rg-devops-training-dev`).
   * **Region:** Choose your preferred region (e.g., *South India* for low latency).
4. **Review and Create:** Click **Review + create**, and once validation passes, click **Create**.

---

### **4. Azure Networking: VNet and Subnets**
* **Virtual Network (VNet):** The fundamental building block for your private network in Azure. VNet enables many types of Azure resources, such as Azure VMs, to securely communicate with each other, the internet, and on-premises networks.
* **Subnets:** Segments into which a virtual network is divided, allowing you to partition the VNet address space into smaller sub-networks.
* **Network Security Groups (NSGs):** Contains security rules that allow or deny inbound network traffic to, or outbound network traffic from, several types of Azure resources (such as VMs and subnets).

---

### **5. Step-by-Step Guide: Creating a Virtual Network (VNet)**

1. In the Azure Portal search bar, type **Virtual networks** and select it.
2. Click on **+ Create**.
3. **Basics Tab:**
   * **Subscription & Resource Group:** Select your subscription and choose the resource group created earlier (`rg-devops-training-dev`).
   * **Name:** Provide a name for your VNet (e.g., `vnet-southindia-1`).
   * **Region:** Select the region (e.g., *South India*).
4. **IP Addresses Tab:**
   * Configure the IPv4 address space (e.g., `10.0.0.0/16` or `172.16.0.0/16`).
   * Add a subnet (e.g., Subnet name: `snet-southindia-1`, Subnet address range: `172.16.0.0/24`).
5. **Security & Review:** Leave default settings or adjust as required, then click **Review + create** and **Create**.

---
---

## **Day 42: Azure Virtual Machines (VM), Configuration & Cost Optimization**

### **1. Azure Virtual Machines Architecture**
* **What is an Azure VM?** A software emulation of physical computers providing scalable computing resources with flexibility without maintaining physical hardware.
* **VM Series & Sizes:** 
  * **B-Series:** Burstable VMs ideal for workloads that don't need continuous full CPU performance.
  * **D/DC-Series:** General-purpose / Confidential compute VMs providing balanced CPU-to-memory ratios, perfect for production apps, enterprise applications, and DevOps build servers.
* **Disks:**
  * **OS Disk:** Contains the pre-installed operating system (e.g., Ubuntu Server 24.04 LTS, default 30 GiB on Premium SSD).
  * **Temporary Disk:** Short-term storage for application data/scratch space.
  * **Data Disks:** Additional managed disks attached for expanded storage needs.

---

### **2. Step-by-Step Guide: Provisioning an Azure Linux VM**

1. In the Azure Portal, search for **Virtual machines** and click **+ Create** -> **Azure virtual machine**.
2. **Basics Tab:**
   * **Subscription & Resource Group:** Select `Azure subscription 1` and `rg-devops-training-dev`.
   * **Virtual machine name:** `vm-devops-app01`.
   * **Region:** `South India`.
   * **Availability options:** No infrastructure redundancy required.
   * **Security type:** Trusted launch virtual machines.
   * **Image:** `Ubuntu Server 24.04 LTS - x64`.
   * **Size:** `Standard_DC1ds_v3` (1 vCPU, 8 GiB RAM with local storage support).
   * **Authentication type:** Password (or SSH public key).
   * **Username:** `azureuser`, enter a secure password.
   * **Public inbound ports:** `Allow selected ports` -> Select **SSH (22)**.
3. **Disks Tab:**
   * **OS disk size:** `Image default (30 GiB)`.
   * **OS disk type:** `Premium SSD` (or Standard SSD for cost saving).
   * **Delete with VM:** Ensure this is **checked** to avoid orphaned disks and ongoing charges.
4. **Networking Tab:**
   * **Virtual network:** Select your created VNet (`vnet-southindia-1`).
   * **Subnet:** Select your created Subnet (`snet-southindia-1`).
   * **Public IP:** Automatically generated.
   * **NIC network security group:** Basic.
   * **Delete public IP and NIC when VM is deleted:** **Checked**.
5. **Management, Monitoring & Advanced Tabs:**
   * Keep default options (Boot diagnostics enabled with managed storage account).
6. **Review + create:**
   * Verify configuration details, check that trial credits apply (`Subscription credits apply`), and click **Create**.

---

### **3. Connecting to the Linux VM & Verification**

* **Method 1: Local Terminal / Command Prompt (SSH)**
  Open your local machine terminal and execute the following command using the VM's Public IPv4 address:
  ```bash
  ssh azureuser@<Public_IP_Address>. **Efficient IP Management:** Prevents wasting uniused IPv4 addresses within cloud infrastructure.
