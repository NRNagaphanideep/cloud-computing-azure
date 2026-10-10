y 54: FinOps, Auditing, and Monitoring Masterclass

---

## **Module 1: FinOps & Auditing - Azure Cost Management & Billing**

### **1. Core Concepts**
* **FinOps (Cloud Financial Operations):** An operational framework that brings financial accountability to the variable spend model of cloud, enabling distributed teams to make trade-offs between speed, cost, and quality.
* **Azure Cost Management + Billing:** A built-in native toolset that helps organizations monitor, allocate, and optimize their Azure spend with granular visibility down to individual resource groups and tags.
* **Budgets & Alerts:** Setting financial thresholds (e.g., $100/month) with automated alert rules configured to trigger email or webhook notifications when spending reaches specific percentages (e.g., 80%, 100%) of the budget.
* **Tagging Strategy:** Key-value metadata attached to resources (e.g., `Environment: Production`, `Owner: DevOps`) to enable accurate cost allocation and chargeback/showback reports.
* **Resource Cleanup:** A routine operational process to identify and delete orphan resources (unattached disks, unassociated elastic IPs, idle load balancers, and old snapshots) to prevent billing bleed.

---

### **Hands-On Practice: Azure Cost Management & Cleanup**

#### **Step 1: Create a Budget and Action Group Alert**
1. Navigate to the **Azure Portal** and search for **Cost Management + Billing**.
2. Select your scope (Subscription or Resource Group) and click on **Budgets** in the left menu.
3. Click **+ Add**:
   * **Name:** `Lab-Monthly-Budget`
   * **Reset period:** Monthly
   * **Creation date:** Current month start
   * **Expiration date:** 1 year out
   * **Amount:** `$50` (Lab limit)
4. Configure Alerts:
   * Threshold: `80%` of Budget (Actual)
   * Email recipient: Your operational email address.
5. Click **Create**.

#### **Step 2: Perform a Resource Cleanup Checklist**
Run the following Azure CLI commands to inspect and clean up dangling lab resources:
```bash
# 1. List all unattached managed disks in the subscription
az disk list --query "[?managedBy==null].{Name:name, ResourceGroup:resourceGroup, DiskSizeGB:diskSizeGb}" -o table

# 2. List unused public IP addresses
az network public-ip list --query "[?ipConfiguration==null].{Name:name, ResourceGroup:resourceGroup}" -o table

# 3. Clean up a specific abandoned resource group
az group delete --name myObsoleteLabGroup --yes --no-wait
```

---
