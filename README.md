Enterprise Multi-Building Network — Cisco Packet Tracer
📌 Project Overview

Designed and implemented a redundant enterprise network spanning two geographically separated buildings, connected through a routed WAN.

The network uses VLAN segmentation, Layer 3 switching, HSRP gateway redundancy, OSPF dynamic routing, centralized DHCP/DNS services, 802.1Q trunking, and DHCP relay.

The project was also used as a practical troubleshooting exercise, including identifying routing, SVI, OSPF, DHCP, and redundancy issues and validating the resulting fixes.

🏗️ Network Architecture


<img width="1921" height="1005" alt="Screenshot (39)" src="https://github.com/user-attachments/assets/89ad3d74-2916-46f5-9cdb-65b07414f5e5" />

      
Building A
VLAN 10 — HR
VLAN 20 — Sales
VLAN 30 — Finance
3 access switches
2 Layer 3 distribution switches
1 WAN router
Building B
VLAN 40 — HR
VLAN 50 — SALES
VLAN 60 — Finance
3 access switches
2 Layer 3 distribution switches
1 WAN router

Access switches have redundant connections to the distribution layer. The distribution switches provide Layer 3 routing and gateway redundancy for the VLANs.

⚙️ Technologies Implemented
VLAN Segmentation

Created separate VLANs for each department to provide logical segmentation and isolate broadcast domains.

802.1Q Trunking

Configured trunk links between access and distribution switches, as well as between distribution switches, allowing multiple VLANs to traverse shared links.

Layer 3 Inter-VLAN Routing

Configured SVIs on the distribution switches and enabled Layer 3 routing using:

ip routing

This allows communication between VLANs without relying on router-on-a-stick.

HSRP Gateway Redundancy

Implemented HSRP between the two distribution switches in each building.

Each VLAN uses an HSRP virtual IP as its default gateway. Failover was tested by intentionally taking a distribution switch offline and verifying that hosts retained connectivity through the surviving switch.

OSPF Dynamic Routing

Implemented OSPF across the routed infrastructure to dynamically exchange routes between distribution switches and WAN routers.

VLAN-facing interfaces were configured as passive interfaces, allowing their networks to be advertised without attempting to form OSPF adjacencies with end devices.

Centralized DHCP & DNS & web servers in a standalone server (VLAN99 named SERVER)

Configured a centralized server to provide DHCP and DNS services.

DHCP relay was implemented using:

ip helper-address 192.168.100.10(the server's ip address)

on the appropriate VLAN interfaces, allowing clients in different subnets and buildings to obtain DHCP addresses from the centralized server.

🛠️ Troubleshooting Experience

A major objective of the project was to troubleshoot configuration issues rather than simply build a functioning topology.

SVI Routing Failure

Issue: Inter-VLAN routing was not functioning despite correctly configured SVIs.

Cause: Layer 3 routing was not enabled on the distribution switches.

Resolution:

ip routing

Lesson: An SVI provides a Layer 3 interface, but the switch must have IP routing enabled to forward traffic between networks.

OSPF Configuration Issue

Issue: OSPF was initially configured on VLAN interfaces without passive-interface settings.

Resolution: Configured VLAN-facing interfaces as passive while keeping routed infrastructure links active for OSPF neighbor formation.

Lesson: Passive interfaces allow networks to be advertised without unnecessarily attempting OSPF adjacency formation toward end-user segments.

Centralized DHCP Across Layer 3 Boundaries

Issue: Clients in remote VLANs could not initially obtain DHCP addresses from the centralized server.

Cause: DHCP discovery uses broadcasts, which do not normally cross Layer 3 boundaries.

Resolution: Configured DHCP relay with ip helper-address on the appropriate SVIs.

Lesson: DHCP relay allows centralized DHCP services to operate across routed networks.

HSRP Failover

Test: Intentionally disabled a distribution switch.

Result: The remaining distribution switch assumed the active gateway role and hosts retained network connectivity.

Lesson: Redundancy was validated through an actual failure scenario rather than simply verifying that HSRP was configured.

🔍 Validation & Testing

The completed topology was validated using:

End-to-end connectivity tests
Inter-VLAN ping tests
WAN connectivity tests
DHCP address assignment
DHCP relay verification
DNS resolution testing
OSPF neighbor verification
Routing table inspection
HSRP failover testing
VLAN and trunk verification

The testing confirmed that the network could provide inter-VLAN and inter-building connectivity while maintaining segmentation, dynamic routing, centralized network services, and gateway redundancy.

🎯 Skills Demonstrated

Networking

VLANs
802.1Q trunking
Layer 3 switching
SVI configuration
Inter-VLAN routing
HSRP
OSPF
DHCP
DHCP relay
DNS
WAN routing
Network troubleshooting

Cisco IOS

Interface configuration
VLAN configuration
Routing configuration
OSPF configuration
HSRP configuration
DHCP relay
Verification and troubleshooting using show commands
