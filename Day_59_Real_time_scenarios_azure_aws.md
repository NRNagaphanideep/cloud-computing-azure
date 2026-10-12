Day 59: Production Outage & Migration Incident Runbook

---

## Scenario 1: Production Virtual Machine (VM / EC2) Unreachable

### 1. Detect
* **Monitoring Alert**: CloudWatch / Azure Monitor triggers a high-severity alert indicating that health checks for the critical web instance have failed and ping/SSH connection attempts are timing out.
* **Symptom**: Users report 502/504 gateway errors when accessing the application portal.

### 2. Isolate
* **Action**: Immediately take the affected instance out of rotation by deregistering it from the Load Balancer's target group or application gateway backend pool to protect user experience.
* **Network Containment**: Apply a quarantine Network Security Group (NSG) to block lateral movement if a security breach is suspected.

### 3. Diagnose
* **Root Cause Analysis**: 
  * Check CloudWatch/Azure Boot Diagnostics screenshots to verify if the OS kernel panicked or locked up.
  * Check if the security group rules were accidentally modified to block port 22 (SSH) or port 3389 (RDP).
  * Check underlying physical hardware status or hypervisor degradation events in the cloud console.

### 4. Fix
* **Remediation**: 
  * If the OS is frozen, perform a soft reboot via the cloud console or CLI.
  * If the root EBS/Disk volume is corrupted, detach it, attach it to a helper recovery instance, run a filesystem check (`fsck`), or restore from the latest snapshot.
  * Correct the security group rules if access was blocked.

### 5. Prevent
* **Prevention Measures**: Implement automated instance recovery alarms, enforce Infrastructure as Code (IaC) governance to prevent manual security group changes, and ensure multi-AZ auto-scaling groups replace unhealthy nodes automatically.

---

## Scenario 2: Private Subnet Instance Losing Internet Access

### 1. Detect
* **Monitoring Alert**: Automated patch management or application telemetry logs report failure to connect to external package repositories or third-party payment APIs.
* **Symptom**: Application backend instances in private subnets cannot pull updates or integrate with external SaaS endpoints.

### 2. Isolate
* **Action**: Isolate the impacted private subnet route table from production traffic routing if routing loops or misconfigurations affect internal microservice communication.

### 3. Diagnose
* **Root Cause Analysis**:
  * Inspect the private subnet's route table to see if the default route (`0.0.0.0/0`) pointing to the NAT Gateway was deleted or modified.
  * Check if the NAT Gateway itself entered a failed state, or if its allocated Elastic IP was disassociated.
  * Check Outbound Network ACL rules denying traffic on ports 80 and 443.

### 4. Fix
* **Remediation**:
  * Re-add or correct the route in the private route table pointing `0.0.0.0/0` to the active NAT Gateway ID.
  * If the NAT Gateway is unhealthy, delete and recreate the NAT Gateway in the public subnet.

### 5. Prevent
* **Prevention Measures**: Use Terraform/CloudFormation modules to lock down VPC route tables, and set up CloudWatch alarms to monitor NAT Gateway packet drop metrics and error counts.

---

## Scenario 3: Critical DNS Resolution Failure (Name Resolution Outage)

### 1. Detect
* **Monitoring Alert**: Synthetic transaction monitors fail with `DNS_PROBE_FINISHED_NXDOMAIN` or `Server Failure` errors across global edge locations.
* **Symptom**: Users cannot resolve domain names (`app.production.com`) to target load balancer IP addresses.

### 2. Isolate
* **Action**: Switch client traffic temporarily to an alternate disaster recovery endpoint or failover DNS record if multi-region routing is configured.

### 3. Diagnose
* **Root Cause Analysis**:
  * Check Route 53 or Azure DNS hosted zones for accidental record deletions or incorrect CNAME/A record mappings.
  * Verify domain registrar DNSSEC settings and namespace delegations.
  * Inspect VPC internal DNS resolution attributes (`enableDnsHostnames` and `enableDnsSupport`).

### 4. Fix
* **Remediation**:
  * Recreate or correct missing DNS records in the authoritative public/private hosted zones.
  * Restore previous record configurations using version-controlled Infrastructure-as-Code state files or CloudTrail audit history.

### 5. Prevent
* **Prevention Measures**: Enable deletion protection on critical DNS hosted zones, enforce strict IAM least-privilege policies for DNS management, and implement automated DNS integration testing in CI/CD pipelines.

---

## Scenario 4: Load Balancer Marking All Target Instances Unhealthy

### 1. Detect
* **Monitoring Alert**: Load balancer health check failure alarm triggers (`UnHealthyHostCount >= 1`).
* **Symptom**: 100% traffic failure; the load balancer returns HTTP 503 Service Unavailable errors to all incoming requests.

### 2. Isolate
* **Action**: Direct incoming traffic away from the failing cluster or display a maintenance landing page via DNS failover to manage customer impact.

### 3. Diagnose
* **Root Cause Analysis**:
  * Verify if the load balancer health check path (e.g., `/healthz`) is returning an HTTP 200 status code on the backend instances.
  * Check if backend Security Groups permit inbound traffic from the Load Balancer's IP range or security group on the application port.
  * Check if the web server process (Nginx/Apache/Node.js) crashed on the backend nodes.

### 4. Fix
* **Remediation**:
  * Fix the application bug causing health check endpoints to fail.
  * Update backend Security Group rules to explicitly allow inbound traffic from the Load Balancer security group.
  * Restart application services on target instances.

### 5. Prevent
* **Prevention Measures**: Implement robust health check endpoints with database connectivity validation, configure graceful application startup probes, and set up automated rollback pipelines for failed deployments.

---

## Scenario 5: Application Credential / Secret Retrieval Failure

### 1. Detect
* **Monitoring Alert**: Application error logs flood with `Authentication Failed`, `AccessDeniedException`, or `InvalidCredentials` when attempting to start up or scale out.
* **Symptom**: Newly auto-scaled instances fail to initialize and connect to the database or external APIs.

### 2. Isolate
* **Action**: Pause Auto Scaling group scaling activities to prevent cascading launch loops and excessive API throttling against secret vaults.

### 3. Diagnose
* **Root Cause Analysis**:
  * Check if the IAM role attached to the compute instance lacks permissions (`secretsmanager:GetSecretValue` or `keyvault:Get`) to read the secret.
  * Verify if the secret was recently rotated and the application cache failed to refresh.
  * Check KMS encryption key policies restricting decryption access.

### 4. Fix
* **Remediation**:
  * Attach the required IAM policy granting read access to the specific Secret ARN or Key Vault identifier.
  * Update KMS key policies to trust the instance role principal.
  * Restart the application service to force a fresh credential fetch.

### 5. Prevent
* **Prevention Measures**: Use automated secret rotation with built-in application notification hooks, test secret retrieval permissions during CI/CD security validation, and implement exponential backoff retry logic in application code for transient secret fetching errors.

---

## Scenario 6: Production Backup & Restore Incident (Data Recovery Failure)

### 1. Detect
* **Monitoring Alert**: Automated backup validation script reports snapshot corruption or restore pipeline failure during routine disaster recovery drills.
* **Symptom**: Inability to restore database volumes or object storage buckets during a simulated outage exercise.

### 2. Isolate
* **Action**: Halt corrupted or failed restoration jobs immediately to prevent overwriting existing partial data states or generating corrupted storage volumes.

### 3. Diagnose
* **Root Cause Analysis**:
  * Check EBS snapshot completion status logs; verify if snapshots were interrupted or deleted prematurely.
  * Inspect cross-region replication permissions for encrypted snapshots requiring KMS customer-managed keys (CMK) in the destination region.
  * Review S3 lifecycle or object lock expiration rules that might have purged required backup versions.

### 4. Fix
* **Remediation**:
  * Locate the last known good verified snapshot or backup point.
  * Re-execute the restoration workflow into an isolated staging environment, ensuring proper KMS key sharing and volume configuration.
  * Verify data integrity using database checksums or application smoke tests post-restoration.

### 5. Prevent
* **Prevention Measures**: Automate weekly disaster recovery restoration drills with validation scripts, enforce S3 Object Lock compliance rules, and set up CloudWatch alarms for failed backup job executions across AWS Backup / Azure Backup vaults.
