Welcome to my hands-on network engineering portfolio. This repository documents the structural deployment of a multi-tiered enterprise network fabric built using Cisco IOS. 

Rather than treating these labs as isolated assignments, this project tracks a continuous, modular engineering lifecycle—progressing from raw Layer 2 broadcast domain segregation up to complex Layer 3 routing protocols, core IP services, and robust edge security hardening.

## 🏗️ Portfolio Architecture & Project Modules

The infrastructure is broken down into five distinct engineering phases:

1. **[01-Layer2-Switching](./01-Layer2-Switching/)** – Building a highly available, loop-free local switching fabric utilizing LACP EtherChannels, 802.1Q trunking optimization, and Rapid-PVST+ tuning.
2. **[02-Layer3-Routing](./02-Layer3-Routing/)** – Establishing inter-VLAN communications via Router-on-a-Stick (RoaS) and Layer 3 SVIs, implementing floating static routes for ISP redundancy, and deploying single-area OSPFv2.
3. **[03-Core-IP-Services](./03-Core-IP-Services/)** – Implementing dynamic IP allocation via DHCP Relay configurations, deploying Network Address Translation (Static NAT & PAT Overload), and establishing secure administrative remote access via SSHv2.
4. **[04-Network-Security](./04-Network-Security/)** – Hardening the infrastructure using Standard and Extended Named Access Control Lists (ACLs), Switchport Port-Security mitigations, DHCP Snooping, and Dynamic ARP Inspection (DAI).
5. **[05-Enterprise-Mega-Lab](./05-Enterprise-Mega-Lab/)** – The final combined architectural showcase. A fully optimized, audited, and production-ready enterprise infrastructure deployment.
