# 🏢 Enterprise Network Design & Layer 2/3 Security Implementation

[![Simulation](https://img.shields.io/badge/Simulator-Cisco_Packet_Tracer-005073?style=for-the-badge&logo=cisco&logoColor=white)](https://www.netacad.com/courses/packet-tracer)
[![Platform](https://img.shields.io/badge/Platform-Cisco_IOS-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)](https://www.cisco.com/)
[![Security](https://img.shields.io/badge/Security-IPsec_VPN_%7C_DAI_%7C_Port_Security-red?style=for-the-badge)](https://en.wikipedia.org/wiki/Computer_security)

A comprehensive enterprise network infrastructure simulation developed in **Cisco Packet Tracer** for a multi-branch Real Estate organization (Cairo & Giza branches). This project covers hierarchical network architecture (Core, Distribution, Access), inter-VLAN routing, high availability, site-to-site IPsec VPN, and multi-layer attack mitigation strategies.

---

## 📌 Table of Contents
- [Executive Summary](#-executive-summary)
- [Enterprise Network Architecture](#-enterprise-network-architecture)
- [Key Features & Protocols](#-key-features--protocols)
- [Threat Modeling & Attack Mitigations](#-threat-modeling--attack-mitigations)
- [Configuration & CLI Implementation](#-configuration--cli-implementation)
- [Project Documentation](#-project-documentation)
- [Team Members](#-team-members)

---

## 📖 Executive Summary

The project simulates an end-to-end enterprise network for a corporate Real Estate firm to optimize communication, monitor production, and ensure data integrity between distinct departments (HR, Developers, Finance, Administration) across two primary branches: **Cairo Branch** and **Giza Branch**.

---

## 🏗️ Enterprise Network Architecture

The network follows the classic Cisco Three-Tier Hierarchical Model:

1. **Core Layer:**
   - Dual Multilayer Switches and redundant border routers connected to two distinct Internet Service Providers (Dual-homed ISP design) for redundancy and high availability.
2. **Distribution Layer:**
   - Aggregation switches managing inter-VLAN traffic routing, QoS policies, and department broadcast domain isolation.
3. **Access Layer:**
   - Cisco Layer 2 switches connecting end-user workstations, printers, and Cisco Wireless Access Points (APs) providing Wi-Fi coverage across all floors.
4. **Data Center / Server Farm:**
   - Centralized server room housing static DNS, Web (HTTP/HTTPS), and DHCP services for dynamic client configuration.

---

## ⚙️ Key Features & Protocols

- **Subnetting & VLSM:** Variable Length Subnet Masking to maximize IPv4 addressing efficiency and segment subnets logically.
- **Segmentation:** Departmental isolation via 802.1Q VLANs (HR, Finance, Dev, IT, Management).
- **Redundancy & Loop Prevention:**
  - **LACP EtherChannel:** Link aggregation across switch interconnections to maximize bandwidth and link redundancy.
  - **Spanning Tree Protocol (STP) Tuning:** Custom root bridge placement, `PortFast`, and `BPDU Guard` on access edge ports.
- **Dynamic Routing:** Multi-area **OSPF** and **EIGRP** routing instances configured for fast convergence.
- **Confidentiality:** Site-to-Site **IPsec VPN** tunnel utilizing AES-256 and pre-shared keys (PSK) for secure inter-branch data transit.

---

## 🛡️ Threat Modeling & Attack Mitigations

The project implements active countermeasures against common network-level security threats:

| Attack Vector | Vulnerability / Threat | Mitigation Mechanism | Implementation Command |
| :--- | :--- | :--- | :--- |
| **Man-in-the-Middle (MITM)** | Data sniffing & session hijacking | IPsec Site-to-Site VPN & TLS/HTTPS | `crypto isakmp policy ...` |
| **MAC Flooding** | Switch CAM table exhaustion | Port Security with MAC limits & Shutdown | `switchport port-security violation shutdown` |
| **ARP Spoofing / Poisoning** | Fake ARP replies intercepting traffic | Dynamic ARP Inspection (DAI) & Static ARP | `ip arp inspection vlan <id>` |
| **DoS / Packet Flooding** | Network & host exhaustion | Ingress Access Control Lists (ACLs) | `access-list <num> deny ...` |
| **IP Spoofing** | Forged source IP addresses | Ingress/Egress packet filtering (Anti-spoofing ACLs) | `access-list <num> deny ip 192.168.0.0 ...` |
| **Reconnaissance / Port Scan**| Mapping active listening ports | Disabling unused interfaces & Port Security | `interface range ... / shutdown` |
| **DNS Cache Poisoning** | Malicious redirection | DNS Filtering & DNSSEC validation | `ip dns domain-lookup` |

---

## 💻 Configuration & CLI Implementation

### 1. VLAN & Access Layer Setup
```cisco
vlan 10
 name Finance
vlan 20
 name HR
exit

interface range fa0/1 - 24
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
2. Switchport Security (MAC Limiting)
Cisco CLI
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
3. EtherChannel (LACP)
Cisco CLI
interface range gi0/1 - 2
 channel-group 1 mode active
4. Dynamic Routing (OSPF)
Cisco CLI
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
5. IPsec Site-to-Site VPN
Cisco CLI
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 2
 exit

crypto isakmp key YourSharedSecretKey address 192.168.2.1

crypto ipsec transform-set ESP-AES-SHA esp-aes 256 esp-sha-hmac
 exit

crypto map VPN-MAP 10 ipsec-isakmp
 set peer 192.168.2.1
 set transform-set ESP-AES-SHA
 match address 101
 exit

interface gi0/0
 crypto map VPN-MAP
📑 Project Documentation
Full design schematics, packet flow captures, feasibility study, and network topology diagrams are documented inside the repository:

📄 Enterprise-Network-Design-Documentation.pdf

👥 Team Members
Project completed under the supervision of Dr. Seham Muawad Ali & Dr. Musheera at Modern Academy for Engineering and Technology:

Abdelrahman Mahmoud Ali Ahmed 

Yousef Adel Emarra Hassan

Abdelrahman Yasser Mohamed Elbasuoni

Saif Alden Qotp Said Qotp

Soheil Yousry Abdelwahed Ibrahim

Mohamed Ahmed Abdelaleem Mostafa

Ali Ahmed Mohamed
