Prerequisites: GNS-3, Vm-Box, cumulus linux, 4 x lubuntu for data center

AS100
● AS100 is a transit Autonomous System providing network access to two customers:
AS200 and AS300
○ Configure eBGP peering with AS200 and AS300
○ Configure iBGP peering between border routers
○ Configure OSPF
○ Configure LDP/MPLS in the core network

AS200
AS 200 is a customer AS connected to AS100, which provides transit services.
● Setup eBGP peering with AS100
● Configure iBGP peering
● Configure internal routing as you wish (with or without OSPF)
● R203 is not a BGP speaker
○ It has a default route towards R202
○ It has a public IP address from the IP address pool of AS200
○ It is the Access Gateway for the LAN attached to it
■ Configure dynamic NAT
■ Configure a simple firewall to allow just connections initiated from the
LAN

Client 200
● This device is sensitive, so it must be configured to use Mandatory Access Control.
● OpenVPN → see later dedicated section.

AS300
AS 300 is a customer AS connected to AS 100, which provides transit services. It also has a
lateral peering relationship with AS 400.
● Setup eBGP peering with AS400 and AS100
● Configure iBGP peering
● Configure internal routing as you wish (with or without OSPF)
● GW300 is not a BGP speaker
○ It has a default route towards R302
○ It has a public IP address from the IP address pool of AS300
○ It is the Access Gateway for the Data Center network attached to it
■ Configure dynamic NAT

DC Network
DC Network is a leaf-spine Data Center network with two leaves and two spines. There are 2
tenants (A and B) in the cloud network, each hosting two virtual machines connected to leaf1
and leaf2. The tenants are assigned one broadcast domain each.
● Realize VXLAN/EVPN forwarding in the DC network to provide L2VPNs between the
tenants’ machines
● In L1, enable the connectivity to the external network. In other words, both tenants’
machines must reach the external network through the link between L1 and R303,
including the encapsulation in OpenVPN tunnels when necessary.

AS400
AS 400 has a lateral peering relationship with AS 300.
● Setup eBGP peering with AS400 and AS100
● R402 is not a BGP speaker
○ It has a default route towards R401
○ It has a public IP address from the IP address pool of AS400
○ It is the Access Gateway for the LAN attached to it
■ Configure dynamic NAT
■ Configure OpenVPN

Client 400
● This is a simple LAN device with a default route through R402.
OPENVPN
Setup OpenVPN to realize a VPN between client-200, R402’s LAN, and the DataCenter
network.

● Client-200 is an OpenVPN client

● R402 is an OpenVPN client, providing VPN access to and from the LAN attached to
it.

● GW300 is the OpenVPN server, providing VPN access to and from the Data Center
network. In particular, the network belonging to tenant A must be accessible through
the VPN.
