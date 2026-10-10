## Topic 1: Azure CLI / PowerShell / Cloud Shell (Azure Operations & Automation)

These tools are used to automate every action performed through the Graphical User Interface (Portal) and to manage resources quickly from the command line.

### 1. Azure CLI (Command Line Interface)
* **Meaning:** It is a cross-platform command-line tool that works on Linux, macOS, and Windows.
* **Authentication (Logging in):**
  * `az login` – Running this command opens an authentication page via browser to log into your Azure account.
* **Subscription Selection:** If you have multiple subscriptions in your account, to select the desired one:
  * `az account list --output table` (to view all subscriptions)
  * `az account set --subscription "<Subscription-ID>"` (to set as the active subscription)

### 2. CLI Command Structure
* **Resource Group:**
  * `az group create --name myResourceGroup --location eastus` (to create a new global container).
* **VM (Virtual Machine):**
  * `az vm create --resource-group myResourceGroup --name myVM --image Ubuntu2204 --admin-username azureuser --generate-ssh-keys`.
* **Query & Output Filtering (`--query` & `--output`):**
  * To extract specific information (e.g., IP address) from large JSON responses returned by commands, we use JMESPath queries:
    * `az vm list-ip-addresses --name myVM --query "[0].virtualMachine.network.publicIpAddresses[0].ipAddress" -o tsv`.

### 3. PowerShell (Az Module) & Cloud Shell
* **Azure PowerShell:** Manage Azure resources from a Windows/Linux PowerShell terminal using Cmdlets like `Connect-AzAccount`, `Get-AzVM`, and `New-AzResourceGroup`.
* **Azure Cloud Shell:** An interactive terminal available directly in the Azure Portal browser without requiring any local installation. It comes pre-configured with both Bash (Azure CLI) and PowerShell.

---
### **2. Hands-on Practice: Create, Inspect and Remove Lab Resources from CLI**

#### **Step 1: Authenticate and Select Subscription**
```bash
# Login to your Azure account interactively via browser
az login

# List all subscriptions available in your account
az account list --output table

# Set your active subscription
az account set --subscription "<Your-Subscription-ID-or-Name>"

# Verify current active subscription
az account show --output json
```

#### **Step 2: Create Lab Resources**
```bash
# Define variables for easy execution
RG_NAME="rg-devops-day53"
LOCATION="eastus"
VM_NAME="vm-ubuntu-lab"

# 1. Create a Resource Group
az group create --name $RG_NAME --location $LOCATION

# 2. Create an Ubuntu Virtual Machine (automatically creates VNet, Subnet, NSG, and Public IP)
az vm create \
  --resource-group $RG_NAME \
  --name $VM_NAME \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s
```

#### **Step 3: Inspect Resources with Querying (`--query`)**
```bash
# Get the Public IP address of the VM and format as tsv (Tab-Separated Values)
az vm list-ip-addresses \
  --resource-group $RG_NAME \
  --name $VM_NAME \
  --query "[0].virtualMachine.network.publicIpAddresses[0].ipAddress" \
  --output tsv

# Inspect VM running state
az vm get-instance-view \
  --resource-group $RG_NAME \
  --name $VM_NAME \
  --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" \
  --output tsv
```

#### **Step 4: Clean Up / Remove Resources**
```bash
# Delete the entire Resource Group to avoid unwanted costs (deletes VM, disk, network interface, VNet instantly)
az group delete --name $RG_NAME --yes --no-wait
```

---
