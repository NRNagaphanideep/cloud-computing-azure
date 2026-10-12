Day 57: Production Lab - Azure Application Gateway & DNS + Security Integration

## 1. Detailed Topic Explanations

### A. Azure Application Gateway
* **Layer 7 Routing**: Operates at the application layer to perform path-based routing, cookie-based session affinity, and multi-site hosting.
* **Listeners**: Evaluates incoming request parameters (Port, Protocol like HTTP/HTTPS, Hostname).
* **Backend Pools**: Groups backend servers (Azure VMs, Scale Sets, App Services) that process web traffic.
* **Health Probes**: Dynamically inspects backend application endpoints to ensure only healthy nodes receive traffic.
* **TLS/HTTPS Termination**: Decrypts incoming encrypted traffic at the gateway, offloading SSL processing from backend servers.
* **VNet & NSG Integration**: Demands a dedicated gateway subnet (`AppGatewaySubnet`) and requires specific NSG rules allowing inbound web traffic (80/443) and Azure infrastructure ports (65200-65535).

### B. DNS & Security Integration
* **Route 53 & Azure DNS Routing**: Managed DNS services providing public name resolution and private zone name resolution within isolated virtual networks.
* **NSG / NACL Layering**:
  * **NSG (Stateful)**: Evaluates rules per network interface or subnet; return traffic is automatically permitted.
  * **NACL (Stateless)**: Evaluates numbered rules sequentially at the subnet boundary; requires explicit inbound and outbound rule configurations.
* **Least Privilege Network Access**: Denies all traffic by default and explicitly grants permissions only to required ports and IP ranges.
* **Traffic Paths**: Ensures public web users hit public DNS and load balancers, while internal app components route securely over private DNS zones and internal subnets.

---

## 2. Hands-On Practice Guide

### Lab Part 1: Configure Azure Application Gateway & Test Backend Health

#### Step 1: Network & Backend VM Setup
1. Create a Virtual Network (`VNet-Prod`) with two subnets:
   * `AppGatewaySubnet` (Minimum `/24` required for Application Gateway).
   * `BackendSubnet` (For hosting your application virtual machines).
2. Deploy two Azure Virtual Machines (`VM-1` and `VM-2`) into `BackendSubnet`.
3. Install a simple web server (Nginx or Apache) on both VMs.

#### Step 2: Create Azure Application Gateway
1. Open the **Azure Portal**, search for **Application Gateways**, and click **Create**.
2. **Basics Tab**:
   * Resource Group: Select your resource group.
   * Name: `app-gateway-prod`.
   * Region: Same region as your VNet.
   * Tier: **Standard v2**.
   * Virtual Network: Select `VNet-Prod` and Subnet: `AppGatewaySubnet`.
3. **Frontends Tab**:
   * Add a Public IP address (`app-gw-pip`).
4. **Backends Tab**:
   * Click **Add a backend pool** named `web-backend-pool`.
   * Add target type: **Virtual machine** and select `VM-1` and `VM-2`.
5. **Configuration Tab**:
   * Add a routing rule (`rule-http`):
     * **Listener**: Name `http-listener`, Protocol `HTTP`, Port `80`.
     * **Backend Targets**: Backend target `web-backend-pool`, HTTP settings (Port 80).
6. Review and click **Create**.

#### Step 3: Test Backend Health & Routing
1. Once deployed, navigate to **Application Gateway** > **Backend health** in the left menu.
2. Verify that both `VM-1` and `VM-2` report a status of **Healthy**.
3. Access the Public IP of the Application Gateway in your browser to verify round-robin traffic distribution.

---

### Lab Part 2: DNS Resolution and Security Testing (Allowed vs. Blocked Paths)

#### Step 1: Configure Security Rules (NSG / NACL)
1. On the `BackendSubnet` NSG:
   * **Allow Rule**: Allow inbound HTTP (Port 80) **only** from the `AppGatewaySubnet` IP range.
   * **Deny Rule**: Deny all other direct inbound traffic from the public internet.
2. Verify least-privilege enforcement by directly pinging/curling `VM-1`'s private IP from outside the VNet (Traffic should be **Blocked**).

#### Step 2: Configure Private DNS Resolution
1. In Azure DNS / Route 53, create a **Private DNS Zone** (e.g., `internal.myservice.local`).
2. Link the Private DNS zone to your Virtual Network.
3. Add an `A Record` mapping `app.internal.myservice.local` to the private endpoint or Application Gateway private IP.
4. From an internal VM, test DNS resolution:
   ```bash
   nslookup app.internal.myservice.local
   curl [http://app.internal.myservice.local](http://app.internal.myservice.local)


