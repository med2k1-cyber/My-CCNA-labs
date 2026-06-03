

##  Project Overview
This repository contains the physical topology and running configurations for a highly redundant, secure Layer 2 campus network. Built from scratch using Cisco Catalyst 2960 switches, this lab demonstrates enterprise-standard switching architectures, focusing on preventing Layer 2 loops, maximizing inter-switch bandwidth, and mitigating common network attacks.

##  Topology & Architecture
* **Hardware:** 3x Cisco Catalyst 2960 Switches, 4x End Devices.
* **Physical Layout:** Triangle loop topology ensuring no single point of failure between core distribution nodes.
* **Logical Links:** All switch-to-switch connections are bundled using dual-cable Layer 2 EtherChannels to double bandwidth capability and provide instant failover.

##  Technologies & Protocols Implemented
* **VLAN Segmentation:** `802.1Q` Trunking, Access Ports.
* **Link Aggregation:** `LACP` (802.3ad) Layer 2 EtherChannel.
* **Loop Prevention:** `Rapid-PVST+` (802.1w) Spanning Tree Protocol.
* **Layer 2 Security:** Native VLAN manipulation, BPDU Guard, PortFast, Password Encryption.

---

##  Configuration Breakdown

### Phase 1: Base Configuration & Segmentation
* Configured global hostnames and encrypted console line passwords (`service password-encryption`).
* Segmented the network logically by creating **VLAN 10 (DATA)** and **VLAN 20 (VOICE)**.
* Hardcoded host-facing physical interfaces as static access ports assigned to VLAN 10.

### Phase 2: High Availability & Trunking
* **LACP EtherChannel:** Grouped redundant physical FastEthernet interfaces into logical Port-Channels (`Po1`, `Po2`, `Po3`) using `channel-group mode active` for dynamic negotiation.
* **802.1Q Trunking:** Configured all Port-Channels as static trunks to carry multi-VLAN traffic across the core.
* **Security Pivot (Native VLAN):** Changed the default Native VLAN on all trunks from VLAN 1 to an unused **VLAN 99**. This explicitly mitigates "VLAN Hopping" (Double-Tagging) attacks by preventing untagged traffic from interacting with the default native VLAN.

### Phase 3: Spanning Tree & Edge Security
* Migrated the global Spanning Tree mode from legacy PVST+ to **Rapid-PVST+** to achieve sub-second convergence times during link failures.
* **Root Bridge Election:** Manually engineered the spanning-tree topology by forcing `Switch 1` as the Primary Root Bridge (Priority 4096) and `Switch 2` as the Secondary Root Bridge (Priority 8192), ensuring optimal and predictable traffic flow.
* **Port Security:** * Enabled `spanning-tree portfast` on all host-facing access ports to bypass listening/learning states for immediate end-user connectivity.
  * Enabled `spanning-tree bpduguard` on all access ports to automatically `err-disable` the interface if a rogue switch is connected, preventing malicious spanning-tree manipulation.

---

##  Verification & Testing
The following commands were used to verify network integrity and functionality:
* `show vlan brief` - Verified VLAN database and access port assignments.
* `show interfaces trunk` - Confirmed 802.1Q encapsulation and Native VLAN 99 status.
* `show etherchannel summary` - Verified LACP negotiation, confirming all Port-Channels achieved the `(SU)` state (Layer 2 / In Use).
* `show spanning-tree` - Confirmed Rapid-PVST+ Root Bridge elections and active/blocked port states.

##  Repository Contents
* `Switch1_Config.txt`: Complete running configuration for Primary Root Bridge.
* `Switch2_Config.txt`: Complete running configuration for Secondary Root Bridge.
* `Switch3_Config.txt`: Complete running configuration for Access Switch.
* `Topology.png`: Visual layout of the physical wiring and logical channels.
