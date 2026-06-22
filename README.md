# Secure-Corporate-Network-Design

## Project Overview
This project features a fully functional, redundant two-tier network topology simulated in Cisco Packet Tracer. It demonstrates production-ready enterprise infrastructure implementation, focusing on secure device management, dynamic host configuration, optimized switching, and segmented Inter-VLAN routing.

## Network Topology Layout & VLAN Architecture
The network is segmented into distinct broadcast domains (VLANs) to enforce security boundaries and optimize traffic flow. 

### VLAN & Subnet Mapping
* **VLAN 10:** HR — `192.168.10.0/24`
* **VLAN 20:** SALES — `192.168.20.0/24`
* **VLAN 30:** IT — `192.168.30.0/24`
* **VLAN 99:** Management — `192.168.99.0/24`
* **VLAN 100:** Servers — `192.168.100.0/24`
*(Note: Feel free to update these VLAN IDs and IP subnets to match your specific configuration)*

### Switch Port Assignments
* **Switch 1 (s1):**
  * `G0/1` -> Uplink to Edge Router (802.1Q Trunk Link)
  * `Fa0/23`, `Fa0/24` -> EtherChannel Bundle to s2
  * `Fa0/1`, `Fa0/3`, `Fa0/5` -> End-User Group A (PC A, PC B, Printer 1)
  * `Fa0/6`, `Fa0/8`, `Fa0/10` -> End-User Group B (PC C, PC D, Printer 2)
* **Switch 2 (s2):**
  * `Fa0/23`, `Fa0/24` -> EtherChannel Bundle to s1
  * `Fa0/11`, `Fa0/13`, `Fa0/15` -> End-User Group C (PC E, PC F, Printer 3)

---

## Technical Features Implemented

### 1. Layer 2 Switching & Redundancy
* **Link Aggregation (EtherChannel):** Configured LACP (Link Aggregation Control Protocol) to bundle physical links `Fa0/23` and `Fa0/24` into a logical **Port-Channel 1** interface, doubling uplink bandwidth between switches and ensuring high availability.
* **Rapid Spanning Tree Protocol (Rapid PVST+):** Upgraded the STP environment to 802.1w (Rapid PVST+) to decrease network convergence and failover times from 50 seconds to under 2 seconds.
* **Root Bridge Optimization:** Manually adjusted bridge priorities (`spanning-tree vlan X priority 4096`) to force `s1` to act as the primary Root Bridge, ensuring deterministic and optimal traffic paths.

### 2. Layer 3 Routing & Core Network Services
* **Router-on-a-Stick (ROAS):** Configured 802.1Q encapsulation on subinterfaces of the router's `G0/0` interface to facilitate secure Inter-VLAN routing across the topology.
* **Centralized DHCP Server:** Configured dynamic IP addressing pools directly on a dedicated HQ server to automate IP, Subnet Mask, Default Gateway, and DNS distribution to end devices across all remote branch VLANs.
* **DNS Services:** Integrated a dedicated DNS server to demonstrate centralized domain name resolution for network hosts.

### 3. Infrastructure Security & Device Hardening
* **Secure Remote Access (SSH):** Disabled unsecured Telnet and implemented SSHv2 globally across all network hardware using local database authentication (`login local`) and a crypto key modulus of 1024 bits.
* **Password Security:** Enforced encryption for all local passwords using `service password-encryption` and secured privileged EXEC mode access using a strong `enable secret`.
* **Legal Banner:** Configured a Message of the Day (`banner motd`) on all console and VTY lines to explicitly deter unauthorized access and fulfill compliance guidelines.

---

## ow to Run and Verify the Lab
1. Download and install **Cisco Packet Tracer**.
2. Clone this repository and open the `.pkt` file.
3. **Verify EtherChannel & STP:** Open the CLI on `s1` and run `show etherchannel summary` and `show spanning-tree` to observe the active bundle and root bridge status.
4. **Verify Layer 3 Continuity:** Open any end-user PC, confirm it successfully leased an IP via DHCP, and run a `ping` to a device on a different VLAN or the DNS server to test Inter-VLAN routing.
