# Linux Node Cluster & Virtual Networking Lab
### Ubuntu Server Infrastructure and Node Configuration

**Project Status:** Complete | **Classification:** Infrastructure / Network Defense

## 🎯 Project Goal

In this lab, I built a small virtual network cluster consisting of a **Master Node** and two **Cloned Nodes** using **Ubuntu Server** within **VirtualBox**. This project demonstrates core principles of **virtualization, internal networking, and command-line configuration within Linux**—skills essential for building and securing networked environments.

## 📁 Technical Documentation

The comprehensive setup guide, detailed configuration steps, and full project goal alignment for this lab are documented in the official team report.

**[➡️ Download Full Project Guide (PDF)](https://github.com/saracena26/Linux-Node-Cluster-and-Virtual-Networking/blob/main/documentation/Linux-Node-Cluster-and-Virtual-Networking.pdf)**

## 💡 Skills Demonstrated

* Virtualization (VirtualBox)
* Ubuntu Server Command Line Interface (CLI)
* Network Configuration and Troubleshooting
* Efficient System Cloning and Setup

## 🛠️ Tools & Technologies Used

* **Virtualization:** VirtualBox
* **Operating System:** Ubuntu Server 22.04 LTS (this may have updated since this project was completed)
* **Components:** Master Node, Cloned Node 1, Cloned Node 2
* **Utilities:** `ping`, `ip addr`, `ssh`, Linux Network Configuration Tools

---

## ⚠️ Important Caution on Networking

***
**The IP addresses used in this documentation are for example only.** You must ensure that any IP address you use is available and not already in use on your own home or lab network to avoid connectivity issues. When cloning, always ensure the **MAC address** and the **hostname** are made unique for each node.
***

## ⚙️ Methodology: Building the Linux Node Cluster

This project was built entirely using VirtualBox and simulates a private network environment.

### 1. Initial Setup: The Master Node (External Access Allowed)

The Master Node was set up with two different network connections to serve as a gateway and control point.

**A. Virtual Machine (VM) Creation & Network Setup:**
* **Adapter 1 (Internet/Management):** Set to **NAT** (This adapter allows the Master Node to access the external internet for updates and management).
* **Adapter 2 (Internal Network):** Set to **"Internal Network"** and named `cluster-net` (This adapter connects the Master Node to the isolated internal network).

**B. Static IP Configuration:**
A static IP was configured on the internal adapter to ensure its address never changes.
* **Example of a Static IP Used (Internal):** `192.168.1.100`

### 2. Efficient Setup: Creating the Cloned Nodes (Fully Isolated)

The two cloned nodes were set up to be fully isolated, and are only able to communicate internally.

**A. Cloning the Master:**
* The Master Node was powered off.
* Two **Full Clones** were created: **Cloned Node 1** and **Cloned Node 2**.

**B. Post-Cloning Configuration (Isolation and Uniqueness):**
1.  **Network Isolation:** On both Cloned Nodes, the **NAT Adapter (Adapter 1) was completely disabled or removed**. **Crucially, only the "Internal Network" adapter (`CSF-NET`) was left enabled.** This ensures these nodes have no path to the external internet.
2.  **Unique Addresses:** The MAC address was regenerated, the hostname was changed, and the static IP was updated on each machine:
    * **Cloned Node 1 Example IP:** `192.168.1.150`
    * **Cloned Node 2 Example IP:** `192.168.1.200`

### Network Topology Diagram

*This diagram illustrates the segmented network architecture: the Master Node has dual network access (NAT for internet) while all nodes communicate securely over the internal 192.168.1.0/24 LAN.*

```bash
+------------------------------------------------------------------+
|           VIRTUALBOX HOST MACHINE / EXTERNAL INTERNET            |
+------------------------------------------------------------------+
               |
               | (NAT Adapter - Internet Access)
               |
+--------------V----------------------------------------------+
|                    MASTER NODE (srvmaster)                  |
|          IP: 192.168.1.100 (Internal) & NAT IP (External)   |
+-------------------------------------------------------------+
               |
               | (Internal Network: 192.168.1.0/24)
               |
+--------------V----------------------------------------------+
|             LINUX NODE CLUSTER LAN (Internal Only)          |
+-------------------------------------------------------------+
                          |           |
                          |           |
                          V           V
            +-------------------+-------------------+
            |  CLONED NODE 1    |  CLONED NODE 2    |
            |  (srvnode1)       |  (srvnode2)       |
            |  IP: 192.168.1.150|  IP: 192.168.1.200|
            +-------------------+-------------------+
         (Isolated from Internet) (Isolated from Internet)
```

---

## 📸 Validation and Proof of Concept

The following visual evidence confirms the successful implementation of network segmentation, internal communication, and secure remote management.

### 1. Proof of Internal Connectivity

This validation confirms the primary functionality: that all nodes can communicate securely within the local network.

* **Node-to-Node and Master Connectivity:** A ping test executed from **Cloned Node 2** successfully reached both **Cloned Node 1** and the **Master Node**. This confirms the internal `cluster-net` LAN is fully operational and allows all devices to communicate.

    ![Node 2 Successful Ping to Node 1 and Master](https://github.com/saracena26/Linux-Node-Cluster-and-Virtual-Networking/blob/main/screenshots/node2-internal-ping-success.png)

### 2. Secure Access Validation

This validation confirms that the networking setup enables secure command and control (C2) for management.

* **Master Node SSH to Cloned Node 1:** The **Master Node** successfully established an **SSH session** with **Cloned Node 1**. This validates the setup of passwordless SSH and confirms that the networking layer supports secure remote management.

    ![Master Node SSH Successful Connection to Node 1](https://github.com/saracena26/Linux-Node-Cluster-and-Virtual-Networking/blob/main/screenshots/ssh-master-to-node1.png)

### 3. Proof of Network Segmentation (Isolation Check)

This validation confirms the crucial security goal: that only the Master Node has internet access.

* **Cloned Node Internet Access:** A ping attempt executed from a Cloned Node 2 to Master Node and Cloned Node 1.
* **Master Node Internet Access:** A ping attempt executed from the Master Node to the same public IP address **succeeded**, confirming the Master Node's NAT adapter is functioning correctly for external management.

---

## 🧠 Lessons Learned & Next Steps (Reflection)

### Lessons Learned

* The most significant challenge was troubleshooting connectivity issues after cloning. This taught me that **cloned VMs always need unique MAC addresses and static IP addresses** before they can reliably connect to the same internal network.
* I learned how to efficiently manage network configuration files in Ubuntu Server using the command line, which is a key skill for a server administrator or SOC analyst.
* I also learned how to create a network topology to plan out how my network should look before creating the network.

### Next Steps

* **Security Integration:** The next iteration of this lab will integrate a dedicated firewall node (e.g., pfSense) and an Intrusion Detection System (IDS) like Snort and Wazzuh to analyze the traffic flowing between these nodes.
* **Automation:** Explore using Bash or Python scripting to automatically configure the static IPs and hostnames on new clones, increasing deployment efficiency.

---

## 🔒 Ethical & Security Compliance

This project is for **educational and portfolio purposes only**. All data, configurations, and IP addresses used are sanitized examples. **No real credentials or confidential information were used** in the creation or documentation of this lab.
