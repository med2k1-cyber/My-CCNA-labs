
<img width="950" height="336" alt="image" src="https://github.com/user-attachments/assets/1dbf1977-dec3-4353-8625-b66836da6a21" />


Infrastructure Mapping:
Switch 1 (Core Master) Puts together all the connections from the middle switches. It set the Layer 2 network topology as the Root Bridge. Hosts PC0 and PC1.

Switch 2 (Distribution/Backup Master): Gives backup route calculation and serves as the backup master if the core is in a failure condition. Has PC2 connected to it.

Switch 3 (Access Edge): directly joins the network fabric with end-user computing resources executing active edge port security rules hosts PC3.

-------------------------

1. Intentional Staged EtherChannel Implementation (LACP)
All the inter-switch trunk links are Cisco standard logical EtherChannels (Port-channel1, Port-channel2 and Port-channel3) with active enabled on them by LACP ( Link Aggregation Control Protocol).

Decision of how to set up the staging strategy: To mimic an actual multi-phase deployment in the real world, the logical containers were pre-prepared and staged within a single physical link topology (transitions of Fa0/22, Fa0/23 and Fa0/24). All encapsulation, trunking configurations and security policies are applied to virtual Port-channel interface.

Value of Scalability: With this design, logical control plane is disjointed from physical provisioning. Any further change demanding higher bandwidth capacity, only field engineers will need to extend the interface range of additional lines in to the corresponding channel-group will not contribute to disruption of network services.

---------------------------

2. Upgraded Rapid PVST+ (802.1w) Engine
The network has been migrated from the myopic and cumbersome 802.1D timers to the forward looking and inevitable Rapid Per-VLAN Spanning Tree Plus (802.1w).

Legacy Recovery Limitation: The default 802.1D transitions force ports to traverse the listening and learning timers one at a time, creating holes in network connectivity up to 30 50 seconds when links recalculate.

Fast Deployment Effect: Link Recalculation and Topology Change using the Fast-converged proposal and agreement active handshake between adjacent switches, the convergence can be achieved under one second (below 2 seconds).

---------------------------

3. Priority Root Bridge Engineering
In order that unfathomable, arbitrarily assigned hardware MAC addresses should not influence the data flow, I have previously configured the standard Spanning Tree topology directly about the hardware's position in the structure: Switch 1 (Core Priority Root ID): 4096 (plus the unique production assignment adds up to 4106, which is the System Priority when VLAN10 is applied). Switch 2 (Failover Priority Root ID): 8192.

  --------------------------
  
4. Edge Mitigation Hardening (PortFast & BPDU Guard)
All active access ports (connected directly to user devices, Fa0/1 and Fa0/2 on Switch 1, and Fa0/1 on Switch 2 and Switch 3) are also configured like this:

Spanning-tree portfast: Skips the listening and learning states, goes straight to forwarding state enabling end-host nodes instant connection and on the fly DHCP tracking.

Spanning-tree bpduguard enable: This feature prevents unauthorized modifications to the network. When an untrusted switch is plugged into an access port by a user, BPDU Guard intercepts all control frames and takes the interface into a temporal operational err-disable shutdown state to terminate any bridging loop before it causes a campus-wide loss of traffic.
