# Enterprise Multi-Site Network Infrastructure:

## Table of Contents
- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Topology Overview & Devices](#topology-overview--devices)
- [Configuration Details](#configuration-details)
  - [VLAN Implementation & IP Addressing](#vlan-implementation--ip-addressing)
  - [Routing Protocols](#routing-protocols)
  - [High Availability & Redundancy](#high-availability--redundancy)
  - [Network Services](#network-services)
- [Key Features](#key-features)
- [Challenges & Solutions](#challenges--solutions)
- [Network Topology Diagram](#network-topology-diagram)
- [Skills Demonstrated](#skills-demonstrated)
- [Getting Started](#getting-started)
- [Repository Structure](#repository-structure)
- [Author & Contact](#author--contact)
- [License](#license)

---

## Project Overview
**Title:** Enterprise Multi-Site Network Infrastructure with OSPF Routing and HSRP Redundancy

This Cisco Packet Tracer–based project demonstrates the design and implementation of a robust, scalable, and secure multi-site enterprise network. Four geographically dispersed locations are interconnected through a hierarchical three-tier architecture (core, distribution, access). The solution incorporates industry-standard routing protocols (OSPF and EIGRP), VLAN segmentation, gateway redundancy (HSRP), and centralized services to ensure optimal performance, high availability, and simplified management across all sites.

---

## Objectives
1. **Seamless Connectivity**  
   Ensure reliable communication between four distinct geographical locations while maintaining performance and uptime.

2. **High Availability**  
   Implement redundant pathways and failover mechanisms (HSRP) to minimize downtime during equipment failures or maintenance.

3. **Scalability**  
   Build a hierarchical, modular design that supports future expansion and additional sites without major redesign.

4. **Security & Segmentation**  
   Use VLANs to logically separate different user groups and applications, enforcing segmentation and access controls.

5. **Centralized Management**  
   Standardize configurations and deploy centralized services to streamline administration and maintain consistency across locations.

---

## Topology Overview & Devices

### Devices Used
- **Core Routers**  
  - 2 × Cisco ISR4331 Integrated Services Routers (Core Layer)

- **Branch/Remote Routers**  
  - 4 × Cisco ISR4331 Integrated Services Routers (One per remote site)

- **Distribution/Access Switches**  
  - 4 × Cisco Catalyst 3560-24PS Multilayer Switches (One per site)  
    - 24 × 1 GbE ports with PoE support

- **End-User Devices**  
  - Desktop and laptop computers distributed across VLANs (Approx. 24–30 devices total)

### Topology Architecture
- **Three-Tier Hierarchy**  
  1. **Core Layer**  
     - Two ISR4331 routers form the backbone, running OSPF in Area 0 for internal routing and EIGRP for external connections.  
  2. **Distribution/Access Layer**  
     - At each site, a local ISR4331 router uplinks to a Catalyst 3560 switch.  
     - Switches provide Layer 2 switching for VLANs and local inter-VLAN routing.  
  3. **Branch Links & Redundancy**  
     - Hub-and-spoke design with redundant WAN links between core routers and each remote site.  
     - HSRP configured on each site’s ISR4331 pair (active/standby) for gateway failover.

- **VLAN Segmentation (consistent across all sites)**  
  - VLAN 10 (Administrative network)  
  - VLAN 20 (General user network)  
  - VLAN 30 (Specialized departmental network)

---

## Configuration Details

### VLAN Implementation & IP Addressing
- **VLAN 10 (Administrative Network)**  
  - Purpose: Management and administrative systems (e.g., NMS, infrastructure monitoring)  
  - Example IP subnet: `10.10.10.0/24`

- **VLAN 20 (General User Network)**  
  - Purpose: Standard employee workstations, general business applications  
  - Example IP subnet: `10.10.20.0/24`

- **VLAN 30 (Specialized Network)**  
  - Purpose: Department-specific resources requiring distinct security or QoS (e.g., R&D, finance)  
  - Example IP subnet: `10.10.30.0/24`

> **Note:**  
> - All sites follow the same VLAN numbering and IP‐scheme template.  
> - Catalyst 3560 switches provide local inter‐VLAN routing where needed to reduce unnecessary WAN traffic.

### Routing Protocols
1. **OSPF (Interior Routing)**  
   - Single-area OSPF (Area 0) across the entire enterprise.  
   - All OSPF-enabled subnets form full adjacency, building a unified link-state database.  
   - Timers tuned (hello = 5 sec, dead = 20 sec) to improve convergence during topology changes.

2. **EIGRP (External/Border Routing)**  
   - Deployed on edge interfaces connecting to external networks or service providers.  
   - Provides fast convergence and efficient metric calculation for external routes.  
   - Redistributes routes between OSPF and EIGRP for centralized Internet access or WAN interconnect.

### High Availability & Redundancy
- **HSRP (Hot Standby Router Protocol)**  
  - Configured on each site’s pair of ISR4331 routers.  
  - Assigns a virtual IP address (e.g., `10.10.X.1`) as the default gateway for each VLAN.  
  - Active/standby roles determined by HSRP priority (higher priority = active).  
  - Preemption enabled so the preferred router regains active status once it returns to service.

- **WAN Redundant Links**  
  - Dual uplinks from each remote site to both core routers.  
  - Static or floating static default routes for failover complement HSRP at Layer 3.

- **STP (Spanning Tree) on Catalyst 3560s**  
  - Prevents Layer 2 loops in the event of redundant inter-switch connections.  
  - Rapid PVST+ with PortFast and BPDU Guard on access ports for faster convergence.

### Network Services
- **DHCP Services**  
  - Distributed DHCP server instances at multiple sites for redundancy and local resiliency.  
  - Centralized DHCP scope management ensures consistent addressing across all VLANs.  
  - DHCP relay configured on ISR4331 routers to forward requests to the appropriate DHCP server.

- **NTP (Network Time Protocol)**  
  - NTP servers hosted in the core (VLAN 10).  
  - All routers and switches synchronize clocks for consistent logging and timestamping.

- **SNMP & Syslog**  
  - SNMPv2c for basic monitoring (community strings restricted to the management VLAN).  
  - Centralized Syslog server in VLAN 10, with all devices forwarding messages for auditing and troubleshooting.

---

## Key Features
- **Advanced Routing**  
  - OSPF for internal multi-site connectivity with fast convergence.  
  - EIGRP for external routing (ISP connectivity) with efficient metric calculations.

- **High Availability & Fault Tolerance**  
  - HSRP for default-gateway redundancy at each site.  
  - Redundant WAN links between remote sites and core routers.  
  - Rapid PVST+ on switches to eliminate Layer 2 loops and accelerate convergence.

- **Logical Network Segmentation (VLANs)**  
  - Separates administrative, user, and departmental traffic.  
  - Enhances security by isolating management and sensitive resources.  
  - Simplifies Quality of Service (QoS) policy enforcement per VLAN.

- **Scalable Hierarchical Design**  
  - Core → Distribution → Access model allows new sites to integrate using the same template.  
  - Modular configuration files for routers and switches (`.cfg` templates) facilitate rapid deployment.

- **Centralized Services**  
  - Distributed DHCP ensures local address assignment with failover.  
  - Centralized NTP and Syslog simplify administration and auditing.

---

## Challenges & Solutions
1. **Routing Protocol Convergence Optimization**  
   - **Challenge:** OSPF convergence was slow during topology changes, causing packet loss between sites.  
   - **Solution:** Tuned OSPF hello/dead intervals (hello = 5 sec, dead = 20 sec) on WAN interfaces. Adjusted EIGRP feasible successor metrics to improve backup path selection.

2. **Consistent VLAN Implementation Across Sites**  
   - **Challenge:** Ensuring uniform VLAN numbering, IP addressing, and security policies at each site.  
   - **Solution:** Created a standardized VLAN/IP scheme document. Used common switch/router templates and configuration scripts to maintain consistency.

3. **Gateway Redundancy Without Routing Loops**  
   - **Challenge:** Configuring HSRP so active/standby roles reflect link capacity and geographic priorities, avoiding suboptimal routing during failover.  
   - **Solution:** Set HSRP priorities based on link bandwidth and physical location. Enabled preemption on higher-priority routers only after verifying link status.

4. **Comprehensive Network Documentation**  
   - **Challenge:** Documenting device configurations, IP plans, and VLAN assignments for future troubleshooting and expansion.  
   - **Solution:** Created detailed documentation in a shared `docs/` folder, including:  
     - IP addressing spreadsheets  
     - VLAN assignment charts  
     - Router/switch configuration templates  
     - HSRP and routing protocol parameter tables  

---

## Network Topology Diagram
> **Note:** A visual Packet Tracer topology file (`.pkt`) is included in the `topology/` directory. It shows:  
> - Core ISR4331 pair (OSPF backbone)  
> - Four remote ISR4331 routers connected via redundant WAN links  
> - Catalyst 3560 switches at each site with VLANs color-coded (pink: VLAN 10, blue: VLAN 20, green: VLAN 30)  
> - End devices (approx. 6–8 per site) on respective VLANs  

If you are viewing this README on GitHub, open `topology/enterprise_multi_site.pkt` in Cisco Packet Tracer (v8.x or higher) to see the full graphical representation.

---

## Skills Demonstrated
- Multi-site WAN design and hierarchical architectures  
- OSPF single-area configuration and tuning  
- EIGRP external routing integration  
- HSRP for gateway redundancy  
- VLAN segmentation and inter-VLAN routing  
- DHCP scope management and relay configuration  
- STP optimization (Rapid PVST+, PortFast, BPDU Guard)  
- Centralized NTP and Syslog server deployment  
- Documentation and templating for large-scale network deployments  

---

## Getting Started

1. **Prerequisites**  
   - Cisco Packet Tracer v8.x (or latest) installed on your workstation.  
   - Basic familiarity with Cisco CLI, OSPF/EIGRP, HSRP, VLANs, and DHCP.

2. **Clone the Repository**  
   ```bash
   git clone https://github.com/<your-username>/enterprise-multi-site-network-infra.git
   cd enterprise-multi-site-network-infra
