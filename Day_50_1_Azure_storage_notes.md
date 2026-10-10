y 50: Cloud Storage Masterclass (Azure Storage & AWS Amazon S3)

This document provides a comprehensive, detailed guide covering core storage architectures, patterns, access tiers, and hands-on labs for both **Microsoft Azure Storage** and **Amazon Web Services (AWS) S3**.

---

## Part 1: Azure Storage Concepts & Architecture

Azure Storage is Microsoft's managed cloud storage solution designed to be scalable, secure, and highly durable. To use any Azure storage service, you must first create a **Storage Account**.

### 1. The Three Core Storage Patterns
Azure Storage offers distinct patterns depending on the data type and access requirements:
* **Blob Storage (Binary Large Object Storage):** Designed for storing unstructured data such as text, images, videos, application logs, and backups. It scales massively.
* **Managed Disks:** Provides block-level storage volumes managed by Azure and used with Azure Virtual Machines (VMs). They act like physical hard drives (`C:` or `D:` drives) attached to servers.
* **Azure Files:** Fully managed file shares in the cloud that can be accessed via industry-standard **SMB (Server Message Block)** and **NFS (Network File System)** protocols, allowing multiple VMs or local systems to share a common file system.

### 2. Access Tiers (Cost Optimization)
Azure categorizes storage based on how frequently data is accessed:
* **Hot Tier:** Optimized for storing data that is accessed frequently (higher storage cost, lowest access cost).
* **Cool Tier:** Optimized for storing data that is infrequently accessed and stored for at least 30 days (lower storage cost, higher access cost).
* **Cold Tier:** For data stored for at least 90 days with very low access requirements.
* **Archive Tier:** For long-term backup, legal, and compliance data that is rarely accessed (lowest storage cost, but retrieval takes hours).

### 3. Redundancy Basics
To protect against hardware failures, Azure replicates data across locations:
* **LRS (Locally Redundant Storage):** Replicates data 3 times synchronously within a single data center in the primary region.
* **ZRS (Zone-Redundant Storage):** Replicates data synchronously across 3 Azure Availability Zones (AZs) in the primary region.
* **GRS (Geo-Redundant Storage):** Replicates data locally using LRS, then asynchronously replicates it to a secondary region hundreds of miles away.

---

## Part 2: Azure Hands-on Practice Guide

### Hands-on 1: Creating a Storage Account & Testing Storage Patterns

#### **Step 1: Create a Storage Account**
1. Log in to the [Azure Portal](https://portal.azure.com).
2. In the top search bar, type and select **Storage accounts**.
3. Click **+ Create**.
4. In the **Basics** tab, provide the following parameters:
   * **Subscription:** Select your active Azure subscription.
   * **Resource group:** Select your target resource group (e.g., `rg-identity-test-01`).
   * **Storage account name:** Enter a globally unique name using lowercase letters and numbers only (e.g., `stdevopsdemo01`).
   * **Region:** Choose a region close to you (e.g., `East US`).
   * **Performance:** `Standard`
   * **Redundancy:** `Locally-redundant storage (LRS)`
5. Click **Review + create**, wait for validation to pass, and click **Create**.
6. Once deployment completes, click **Go to resource**.

#### **Step 2: Test Blob Storage Pattern**
1. In the left-hand menu of your storage account, look under **Data storage** and click **Containers**.
2. Click **+ Container**.
3. Enter **Name:** `demo-files` and set **Anonymous access level:** `Private (no anonymous access)`.
4. Click **Create**.
5. Click into the newly created `demo-files` container and click **Upload** to select and upload any sample local file.

#### **Step 3: Test Azure Files Pattern**
1. In the left-hand menu under **Data storage**, click **File shares** (or **Classic file shares** depending on your portal wizard view).
2. Click **+ File share** (or `+ Classic file share`).
3. Enter **Name:** `devops-share`.
4. Click **Create**.
5. Open `devops-share`, click **Upload**, and upload a test file to verify shared file system functionality.

---
