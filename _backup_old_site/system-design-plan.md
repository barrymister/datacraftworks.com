
# System Design Plan: Proxmox with Prometheus, Grafana, Wazuh, and Ansible

## **System Design Overview**

### **1. Proxmox Node**
- **Role:** The central hypervisor for hosting all containers and VMs.
- **Environment Setup:** Proxmox itself will run all services via LXCs and potentially a VM for Wazuh, given its resource needs and security focus.
- **Storage Considerations:** Ensure adequate disk space for logs and metrics (consider Proxmox-managed LVM or ZFS pools for efficient storage).

---

## **2. Components and Their Placement**

### **A. Ansible**
- **Placement:** A lightweight LXC container.
- **Role:** Configuration management and automation tool to provision and manage other services and containers.
- **Resource Requirements:** 
  - **CPU:** 1 vCPU
  - **RAM:** 512MB
  - **Disk:** 2GB
- **Dependencies:** SSH access to Proxmox node and containers.
- **Notes:** Used to automate initial setups and future changes.

---

### **B. Docker Host**
- **Placement:** A single LXC container for Docker.
- **Role:** Hosts Prometheus, Grafana, and other lightweight applications in containers for modularity and easy management.
- **Resource Requirements:**
  - **CPU:** 2 vCPUs
  - **RAM:** 2GB (minimum) to 4GB (recommended, expandable as services scale).
  - **Disk:** 20GB (expandable for persistent volumes).
- **Dependencies:** Docker runtime, storage for containers, and network access.

---

### **C. Prometheus**
- **Placement:** Inside the Docker LXC container.
- **Role:** Collects metrics from Proxmox, LXCs, and other systems.
- **Resource Requirements:**
  - **CPU:** 1 vCPU
  - **RAM:** 1GB (minimum), scales with time-series data.
  - **Disk:** 5GB (minimum) to 50GB (depending on retention policy).
- **Notes:** Persistent storage is critical for long-term metric retention.

---

### **D. Grafana**
- **Placement:** Inside the Docker LXC container.
- **Role:** Visualizes Prometheus metrics and other monitoring data.
- **Resource Requirements:**
  - **CPU:** 1 vCPU
  - **RAM:** 512MB (minimum), 1GB (recommended).
  - **Disk:** 2GB (minimum) for configuration and dashboards.
- **Notes:** Shares the Docker LXC with Prometheus for simplicity.

---

### **E. Wazuh**
- **Placement:** A dedicated VM (preferred for security and isolation).
- **Role:** Security monitoring, log collection, and compliance tracking.
- **Resource Requirements:**
  - **CPU:** 2 vCPUs (minimum), 4 vCPUs (recommended for log-heavy setups).
  - **RAM:** 8GB (minimum), 16GB (recommended for larger environments).
  - **Disk:** 100GB (minimum), scales with log storage.
- **Notes:** 
  - Install Wazuh Manager in the VM.
  - Use Wazuh agents on Proxmox host and containers.

---

### **F. Proxmox Exporter**
- **Placement:** Inside the Docker LXC container.
- **Role:** Collects Proxmox-specific metrics for Prometheus.
- **Resource Requirements:**
  - Minimal; it shares the Docker LXC resources.
- **Notes:** This provides critical visibility into Proxmox host performance.

---

## **3. Resource Allocation Summary**

| Component         | Placement            | CPU    | RAM       | Disk    |
|--------------------|----------------------|--------|-----------|---------|
| **Ansible**        | LXC Container        | 1 vCPU | 512MB     | 2GB     |
| **Docker Host**    | LXC Container        | 2 vCPUs | 4GB       | 20GB    |
| - Prometheus       | Inside Docker LXC    | 1 vCPU | 1GB+      | 5-50GB  |
| - Grafana          | Inside Docker LXC    | 1 vCPU | 512MB+    | 2GB     |
| - Proxmox Exporter | Inside Docker LXC    | Minimal | Minimal   | Minimal |
| **Wazuh Manager**  | Dedicated VM         | 4 vCPUs | 16GB      | 100GB   |

---

## **4. Order of Creation**

1. **Provision Ansible LXC Container:**
   - Deploy Ansible first to automate subsequent steps.
   - [Ansible LXC Setup](ansible-lxc-setup.md)

2. **Provision Docker LXC Container:**
   - Configure Docker to host Prometheus, Grafana, and the Proxmox exporter.

3. **Deploy Prometheus in Docker:**
   - Configure Prometheus to scrape metrics from the Proxmox node, LXCs, and other systems.

4. **Deploy Grafana in Docker:**
   - Set up dashboards and integrate with Prometheus.

5. **Deploy Proxmox Exporter in Docker:**
   - Export Proxmox-specific metrics to Prometheus.

6. **Provision Wazuh VM:**
   - Install Wazuh Manager and set up agents on the Proxmox host and containers.

7. **Configure Dashboards and Alerts:**
   - Use Grafana for monitoring dashboards.
   - Set up Prometheus Alertmanager and Wazuh notifications.

---

## **Why This Approach Works**
- **Isolation:** Critical components like Wazuh are isolated in a VM for security, while lightweight services share resources in containers.
- **Scalability:** The Docker LXC can be expanded easily as new services or workloads are added.
- **Manageability:** Ansible ensures consistent and repeatable deployments.
- **Future-Proofing:** The setup scales well when more Proxmox nodes are added.

---
