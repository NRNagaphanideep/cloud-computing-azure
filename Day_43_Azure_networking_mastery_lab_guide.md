Day 43: Comprehensive Azure Networking Mastery & Lab Guide

## Introduction
Azure Networking forms the backbone of any cloud-based infrastructure. This document provides an exhaustive, production-grade reference for Day 43 of the DevOps learning path. It covers core architectural components, security configurations, advanced routing logic, and hands-on validation labs.

---

## 1. Complete Glossary of Abbreviations & Full Forms

| Abbreviation | Full Form / Technical Term | Description & Context in Azure |
| :--- | :--- | :--- |
| **VNet** | Virtual Network | The fundamental building block for your private network in Azure, enabling VMs and resources to securely communicate. |
| **VM** | Virtual Machine | An on-demand, scalable computing resource provided by Azure as a software-defined computer. |
| **NSG** | Network Security Group | A virtual firewall containing security rules that allow or deny inbound/outbound network traffic to resources. |
| **NIC** | Network Interface Card | A virtual adapter that connects a VM to a Virtual Network, managing IP addresses and network traffic. |
| **UDR** | User Defined Route | Custom routing tables created by administrators to override or supplement Azure system routes. |
| **NVA** | Network Virtual Appliance | A specialized virtual appliance (such as a firewall or router) that monitors and controls network traffic. |
| **IP** | Internet Protocol | A numerical label assigned to every device connected to a computer network. |
| **CIDR** | Classless Inter-Domain Routing | An IP addressing methodology used for packet routing and IP address allocation (e.g., `10.1.0.0/16`). |
| **SSH** | Secure Shell | A cryptographic network protocol for operating network services securely over an unsecured network (Port 22). |
| **HTTP** | Hypertext Transfer Protocol | An application protocol for distributed, collaborative, hypermedia information systems (Port 80). |
| **RDP** | Remote Desktop Protocol | A proprietary protocol providing a user with a graphical interface to connect to another computer (Port 3389). |
| **BGP** | Border Gateway Protocol | A standardized exterior gateway protocol designed to exchange routing and reachability information. |

---

## 2. Deep Dive: Core Azure Networking Topics

### 1. VNet & Multi-Tier Subnetting (`vnet-multitier-prod`)
* **Concept:** A Virtual Network allows Azure resources to securely communicate with each other, the internet, and on-premises networks. Subnetting divides a VNet into smaller logical segments (tiers) such as Web, Application, and Data.
* **Production Value:** Implementing multi-tier subnets enforces strict architectural isolation, ensuring that database tiers are completely hidden from direct public internet exposure.

### 2. Network Security Groups (NSGs)
* **Concept:** NSGs contain security rules that filter network traffic to and from Azure resources. Rules are evaluated based on priority (lower numbers are processed first).
* **Application Scope:** NSGs can be attached directly to **Subnets** (protecting all resources within that subnet) or to individual **NICs** (fine-grained control per VM).

### 3. NICs (Network Interface Cards) & IP Addressing
* **NIC Role:** Acts as the communication bridge between the VM operating system and the physical/virtual network infrastructure.
* **IP Separation:**
  * **Private IP:** Used for internal communication within the VNet or peered networks.
  * **Public IP:** Enables inbound/outbound internet connectivity directly to the resource.

### 4. User Defined Routes (UDRs) & Route Tables (`rt-prod-routing`)
* **Concept:** Azure automatically creates system routes for VNets. UDRs allow DevOps and Network engineers to override these defaults and direct traffic through specific paths or gateways.
* **Use Case:** Forcing specific subnet traffic to flow through an inspection firewall or corporate gateway.

### 5. Route Precedence & Effective Routes
* **Evaluation Logic:** Azure uses the **"Most Specific Route Wins"** rule. If multiple routes match a destination, the route with the smallest IP prefix (e.g., `/24` over `/16`) takes precedence.
* **Effective Routes:** A diagnostic view showing all active system, UDR, and BGP routes currently applied to a running VM's network interface.

### 6. Network Virtual Appliances (NVAs)
* **Concept:** Third-party virtual machines acting as routers, load balancers, or firewalls.
* **Routing Integration:** By setting a UDR's "Next Hop Type" to `Virtual appliance` and providing the NVA's private IP, all target subnet traffic is routed through the security appliance for inspection.

---

## 3. Detailed Hands-On Labs

### Lab 1: Provisioning Multi-Tier VNet and Subnets
* **Objective:** Create an isolated production network structure with three functional tiers.
* **Execution Steps:**
  1. Navigate to the Azure Portal and search for **Virtual networks**.
  2. Click **+ Create** and configure the basic settings:
     * **Subscription / Resource Group:** `rg-devops-training-dev`
     * **Name:** `vnet-multitier-prod`
     * **Region:** `South India`
  3. Under **IP Addresses**, define the IPv4 address space: `10.1.0.0/16`.
  4. Create three distinct subnets:
     * `subnet-web`: `10.1.1.0/24`
     * `subnet-app`: `10.1.2.0/24`
     * `subnet-data`: `10.1.3.0/24`
  5. Review and click **Create**.

### Lab 2: Configuring Network Security Groups (NSGs) & NIC Association
* **Objective:** Secure virtual components using virtual firewalls applied at the NIC level.
* **Execution Steps:**
  1. Search for and select **Network security groups**.
  2. Create an NSG named `nsg-web-tier`.
  3. Add an inbound security rule:
     * **Source:** Any
     * **Destination:** Any
     * **Service/Port:** `22` (SSH) / `80` (HTTP)
     * **Action:** `Allow`
     * **Priority:** `300`
  4. Navigate to your target virtual machine (`vm-devops-app01`), go to **Networking** settings, click on the attached **NIC** (`vm-devops-app017`), and associate `nsg-web-tier` directly to the network interface.

### Lab 3: Implementing UDRs and Validating Effective Routes
* **Objective:** Create a custom route table and verify its application on a running virtual machine.
* **Execution Steps:**
  1. Search for and create a **Route table** named `rt-prod-routing` in the same region.
  2. Go to **Routes** under settings and add a custom route:
     * **Route name:** `Route-To-Internet`
     * **Address prefix destination:** `IP Addresses`
     * **Destination CIDR:** `0.0.0.0/0`
     * **Next hop type:** `Internet`
  3. Go to **Subnets** within the Route Table and associate it with `subnet-app` (and verify matching target subnets where your test VM resides).
  4. Ensure your test VM (`vm-devops-app01`) is in a **Running** state.
  5. Navigate to the VM's **Networking** -> **Effective routes** tab to confirm that the custom UDR appears with source `User` or `UserDefined`.
