Day 47: Domain Name System (DNS) - Azure DNS & AWS Route 53

## 1. Introduction & Purpose of DNS
- **Definition**: DNS stands for Domain Name System, often referred to as the "Phone Book of the Internet."
- **Core Purpose**: Computers and servers communicate via numeric IP addresses (e.g., IPv4 `192.0.2.1` or IPv6), but humans use easy-to-remember domain names (e.g., `viskogroup.com`). DNS translates human-readable domain names into machine-readable IP addresses.
- **Cloud DNS Providers**:
  - **AWS Route 53**: Highly available and scalable cloud DNS service by Amazon Web Services (named "53" after port 53 used by DNS).
  - **Azure DNS**: Microsoft Azure's cloud DNS service for managing domain names using Azure infrastructure.

---

## 2. DNS Records
DNS records connect a domain name to servers or specific services. Key record types include:
1. **A Record (Address Record)**: Maps a domain name to an **IPv4 address** (e.g., `viskogroup.com` -> `192.168.1.10`).
2. **AAAA Record (Quad-A Record)**: Maps a domain name to an **IPv6 address**.
3. **CNAME Record (Canonical Name Record)**: Aliases one domain name to another (e.g., `www.viskogroup.com` -> `viskogroup.com`). Note: Cannot be used on a root/apex domain.
4. **Alias Record (Route 53 Feature)**: Similar to CNAME but can be used on **Root Domains** (`viskogroup.com`). It maps directly to AWS resources like Load Balancers, CloudFront distributions, or S3 static websites.

---

## 3. TTL (Time to Live) and IP Changes
- **What is TTL?**: TTL is an expiry timer (in seconds) set on a DNS record that dictates how long internet service providers (ISPs) and browsers cache the DNS lookup result.
- **Low vs. High TTL**:
  - **Low TTL (e.g., 60 - 300 seconds)**: Ideal during migrations, disaster recovery, or frequent IP changes because traffic redirects quickly. However, it increases DNS query loads.
  - **High TTL (e.g., 24 hours)**: Reduces server/DNS query load and improves performance because users fetch cached records locally, but delays global propagation when IPs change.
- **Why Change a Website's IP Address?**:
  - **Cloud Migration**: Shifting from on-premises data centers to AWS/Azure.
  - **Cost Optimization**: Changing cloud providers or regions.
  - **Scaling for High Traffic**: Attaching new Load Balancers to handle massive traffic spikes (e.g., flash sales).
  - **Disaster Recovery (DR)**: Switching traffic to a backup server if the primary server crashes.
  - **Security & DDoS Protection**: Moving behind a web application firewall/proxy (like Cloudflare) or switching to a clean server after an attack.

---

## 4. Hosted Zones
A Hosted Zone is a digital container holding all DNS records for a specific domain.
- **Public Hosted Zone**: Used for resources exposed to the public internet (e.g., public websites like `viskogroup.com`).
- **Private Hosted Zone**: Used exclusively within an internal cloud network (AWS VPC or Azure VNet) so internal services (e.g., `db.internal.viskogroup.com`) remain hidden from the outside world.

---

## 5. DNS Routing Policies
Routing policies determine how Route 53 or Azure DNS responds to queries and directs user traffic:
1. **Simple Routing Policy**: Single record pointing to a single server/IP without any special logic.
2. **Failover Routing Policy**: Implements Primary and Secondary (Backup) servers with health checks to automatically route traffic to the backup if the primary fails (High Availability).
3. **Geographic (Geo-location) Routing**: Directs users to the closest regional data center based on their physical location (e.g., Indian users to Mumbai servers).
4. **Latency-based Routing**: Routes traffic to the server that provides the lowest network response time/latency at that moment.
5. **Weighted Routing Policy**: Splits traffic by percentage (e.g., 90% to old version, 10% to new version) for canary deployments or A/B testing.

---

## 6. Health Checks and Basic Troubleshooting
- **Health Checks**: Automated probes sent by the cloud provider (every 10-30 seconds) to monitor server availability via HTTP/HTTPS/TCP. If a server goes down, failover routing triggers automatically.
- **Core Troubleshooting Commands**:
  - `nslookup domain.com`: Checks the current IP mapping and default DNS server.
  - `dig domain.com ANY`: Gathers comprehensive DNS record details and responses.
  - `dig @8.8.8.8 domain.com`: Queries a specific public DNS resolver (Google) to verify propagation.
  - `ping domain.com`: Tests basic network connectivity and packet loss.
  - `traceroute domain.com`: Traces the network path (hops) from the client to the server to isolate bottlenecks or drops.
