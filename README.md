# hotel-management-network-design
Enterprise 3-floor Hotel Management Network designed in Cisco Packet Tracer featuring VLANs, Inter-VLAN Routing, OSPF, DHCP, Wireless APs, SSH, and Port Security.

# Enterprise Hotel Management Network Design & Implementation

A multi-floor enterprise network simulation built in **Cisco Packet Tracer** (`Hotel_Management_Network_Design.pkt`). The project models a modern 3-story hotel infrastructure with 8 isolated departments, dynamic routing, wireless connectivity, and Layer 2/Layer 3 security hardening.

![Network Topology](topology.png)

## Network Architecture & IP Addressing Plan

The topology consists of three **Cisco 2911** routers (one per floor) interconnected in a full-mesh triangle via **Serial DCE** links, with each router connected to a **Cisco 2960** access switch via an 802.1Q trunk link.

### LAN Subnets & VLAN Allocation

| Floor | Router | Department | VLAN ID | Network Subnet | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Floor 1** | `F1-Router` | Reception | VLAN 80 | `192.168.8.0/24` | `192.168.8.1` |
| **Floor 1** | `F1-Router` | Store | VLAN 70 | `192.168.7.0/24` | `192.168.7.1` |
| **Floor 1** | `F1-Router` | Logistics | VLAN 60 | `192.168.6.0/24` | `192.168.6.1` |
| **Floor 2** | `F2-Router` | Finance | VLAN 50 | `192.168.5.0/24` | `192.168.5.1` |
| **Floor 2** | `F2-Router` | HR | VLAN 40 | `192.168.4.0/24` | `192.168.4.1` |
| **Floor 2** | `F2-Router` | Sales & Marketing | VLAN 30 | `192.168.3.0/24` | `192.168.3.1` |
| **Floor 3** | `F3-Router` | Admin | VLAN 20 | `192.168.2.0/24` | `192.168.2.1` |
| **Floor 3** | `F3-Router` | IT | VLAN 10 | `192.168.1.0/24` | `192.168.1.1` |

### WAN Inter-Router Links (Serial DCE)

* **F2-Router ↔ F3-Router:** `10.10.10.0/30` (Clock Rate: `64000`)
* **F1-Router ↔ F3-Router:** `10.10.10.4/30` (Clock Rate: `64000`)
* **F1-Router ↔ F2-Router:** `10.10.10.8/30` (Clock Rate: `64000`)

## Implemented Technologies & Protocols

* **VLAN Segmentation & Trunking:** Isolated broadcast domains across 8 hotel departments with `802.1Q` trunk links between switches and routers.
* **Inter-VLAN Routing (Router-on-a-Stick):** Configured sub-interfaces (`GigabitEthernet0/0.10` through `GigabitEthernet0/0.80`) on floor routers to route traffic between department VLANs.
* **Dynamic Routing (OSPF):** Deployed single-area **OSPF (`Area 0`)** across all three routers to advertise LAN and WAN subnets for full end-to-end reachability.
* **DHCP Services:** Configured Cisco IOS DHCP pools per department to dynamically assign IP addresses, subnet masks, default gateways, and DNS settings to PCs, network printers, and wireless clients.
* **Wireless LAN Deployment:** Integrated dedicated Access Points on each floor (`Floor1-AP`, `Floor2-AP`, `Floor3-AP`) secured with WPA2-PSK authentication for laptops, tablets, and smartphones.
* **Security Hardening:**
  * **SSH Remote Management:** Secured remote CLI access across all routers using RSA 1024-bit encryption keys (`crypto key generate rsa`), local user authentication (`login local`), and `transport input ssh` on VTY lines.
  * **Layer 2 Port Security:** Hardened the IT department switch port (`Fa0/2`) using `sticky` MAC address learning, `maximum 1`, and `shutdown` violation mode to restrict physical port access strictly to `Test-PC`.

## Verification & Testing

* **DHCP & Connectivity:** Verified dynamic IP lease acquisition across all end devices and confirmed cross-floor/inter-VLAN reachability via ICMP `ping`.
* **OSPF Adjacencies:** Validated neighbor states and routing table propagation using `show ip ospf neighbor` and `show ip route`.
* **SSH Access:** Tested encrypted remote sessions from `Test-PC` (IT VLAN 10) to `F1-Router` and `F2-Router` via `ssh -l`.
* **Port Security:** Verified MAC binding and violation status on the IT switch using `show port-security interface fa0/2`.

## How to Run

1. Clone this repository or download `Hotel_Management_Network_Design.pkt`.
2. Open the file using **Cisco Packet Tracer 8.2** or newer.
