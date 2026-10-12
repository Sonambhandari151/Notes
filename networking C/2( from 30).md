### **Infrastructure as Code (IaC)**

Infrastructure as Code (IaC) is a way to manage and set up IT infrastructure using code instead of manual processes. Instead of configuring servers and networks manually, you write definition files that describe how everything should be set up.

- You can define servers, networks, and applications as code.
- It allows you to update and modify infrastructure easily and track changes over time.   
- You can use the same code to create multiple identical setups, ensuring consistency.

### **Playbooks**

Playbooks are step-by-step guides that describe how to handle specific situations, often used in automation and security operations.

- They contain conditional steps to follow in different scenarios.
- Example: Investigating a data breach or recovering from a ransomware attack.
- They act as reusable templates that can automate tasks.
- Often used in **SOAR (Security Orchestration, Automation, and Response)** platforms, which integrate security tools and automate responses.

### **Automation Use Cases**

Automation helps ensure consistency, efficiency, and security in IT operations. Some common use cases include:

1. **Configuration Management**
    
    - Ensures all systems have the same settings.
    - Testing and production environments remain identical.
    - IaC makes deployments uniform and repeatable.
2. **Upgrades**
    
    - Configuration changes can be made with a single line of code.
    - Easily update software and infrastructure settings.
3. **Dynamic Inventories**
    
    - Query devices in real-time to manage and monitor them.
    - Adjust configurations based on real-time data.

### **Source Control**

Source control is used to track and manage changes in infrastructure code.

4. **Managing Change**
    
    - Developers write and publish infrastructure definition files.
    - This ensures a clear and organized setup.
5. **Ongoing Updates**
    
    - A version control system (like Git) helps track and manage updates.
    - Every change is recorded, so nothing gets lost.
6. **Tracking Changes**
    
    - A central repository stores all updates.
    - Multiple people can work on the code without conflicts.
    - Changes are merged safely to keep everything stable.

## IPv6 addresses
- 128 bit address.
- Groups of zeroes can be abbreviated with a double colon.
- Leading zeroes are optional.
- eg. 2601:04C3:4002:BE00:0000:0000:0000:0066
- 2601:4C3:4002:BE00::66
- Not all devices can talk IPv6, require an alternate form of communication(all are temporary)
     - TUNNEL: encapsulate one protocol within another
     - Dual- stack: Have the option to use IPv4 and IPv6
     - TRANSLATE: Convert between IPv4 and IPv6
### Tunneling IPv6
- A migration option, for temp use
- **6to4 addressing**
     - Send IPv6 over an existing IPv4 network.
     - Creates an IPv6 address based on the IPv4 address
     - require relay routers
     - no support for NAT
     - no longer available as an option in windows.
 - **4in6 tunneling**
     - Tunnel IPv4 traffic on an IPv6 network.
- **Dual - stack routing**
     - Run IPv4 & IPv6 at the same time.
     - IPv4
         - Configured with IPv4 addresses.
         - Maintains an IPv4 routing table
         - Uses IPv4 dynamic routing protocols.
    Same with IPv6.
- **Translating between IPv4 and IPv6**
     - Network address translation using NAT64
     - Translate between IPv4 and IPv6
     - Seamless to the end user.
     - Requires a NAT64 -capable router.
     - Works with a DNS64 server to translate the DNS requests.
## Routing Table
- Identify the destination IP address, it's in the packet.
- If the destination IP address in on a locally connected subnet, forward the packet to the local device.
- IF it's on a remote subnet . forward to the next-hop router/gateway.
- This map of forwarding locations is routing table.
## Static Routing
Static routing is when network routes are manually set up by an administrator. The routes are fixed and do not change unless the administrator updates them. It’s simple, predictable, and works well for small networks. However, it doesn't automatically adjust if something in the network changes, like a connection failure.

For example, you tell a router, "To reach this network, send the data through this specific route," and that route stays the same until you decide to change it.

## Dynamic Routing
- Routers send routes to other routers.
- Some router overhead required( CPU, memory, bandwidth) .
- Require some initial configuration.
- PROTOCOLS
    - Listen for subnet information.
    - provide subnet information to other routers.
    - Determine the best path based on this information.
    - When network change occur, update the available routes.
- OSPF and BGP are standard protocol
- EIGRP is cisco Proprietary.
## Enhanced Interior Gateway Routing Protocol
- Partly proprietary to Cisco.
- Commonly used on internal Cisco- routed networks.
- Cleanly manage topology changes.
- Loop free operation.
- minimize bandwidth usem by efficient discovery of neighbor routers.
## Open Shortest Path First (OSPF)
- A common interior gateway protocol.
- Used within a single autonomous system(AS)
- Link-state protocol
    - Routing is based on the connectivity between routers.
    - Each link has a cost.
    - throughput, realiability, round-trip time.
    - Low cost and fastest path wins, identical costs are load balanced.
## border Gateway Protocol(BGP)
- Exterior Gateway Protocol.
- Connect different autonomous systems(AS).
- The "three-napkins protocol"
    - sketched out to solve an immediate problem.
    - Turned into one of the most popular.
- A popular standard , used around the world for Internet routing.

## Routing entries
- R 10.10.30.0/24 [120/1] via 10.10.50.2 . 00:00:14 , Serial 0/3/1
- Route code - subnet ID with prefix length- Administrative distance - metric - Next Hop - Route Timestamp - Outgoing Interface,

### Source & Administrative Distance
Local: 0
Static route : 1
EIGRP : 90
OSPF : 110
RIPv1 RIPv2 : 120
DHCP default route: 254
Unknown : 255

## Routing metric

A **routing metric** is a value used by routing protocols to determine the best path to a destination. Lower metric values indicate better paths. Different routing protocols use different metrics:

### **Common Routing Metrics:**

- **Hop Count** → Used by **RIP** (fewer hops = better route).
- **Bandwidth** → Used by **EIGRP** (higher bandwidth = better route).
- **Delay** → Used by **EIGRP** (lower delay = better route).
- **Cost** → Used by **OSPF** (lower cost = better route, based on interface bandwidth).
- **Reliability** → Some protocols consider link stability.
- **Load** → Some protocols consider traffic on the link.
## First Hop redundancy Protocol(FHRP)
**FHRP (First Hop Redundancy Protocol)** ensures network continuity by providing a **backup default gateway** in case the primary gateway fails.
- Devices use a virtual IP (VIP) for the default gateway .
- If a router disappears, another one take its place.

## Subinterfaces
A subinterface is a logical interface created on a physical interface to allow multiple networks (VLANs) to share the same physical link. It is commonly used for Inter-VLAN routing and traffic segmentation using 802.1Q encapsulation.
Key Information:
Purpose: Enables a single physical interface to handle multiple VLANs.
Encapsulation: Uses 802.1Q tagging to differentiate VLAN traffic.
Routing Support: Used in Router-on-a-Stick configurations for Inter-VLAN Routing.
Efficiency: Reduces the need for multiple physical interfaces.
Trunk Requirement: Works over a trunk port on a switch.

## NAT Overload (PAT) – Port Address Translation

NAT Overload, also known as Port Address Translation (PAT), is a type of Dynamic NAT where multiple private IP addresses are mapped to a single public IP using different port numbers.
EG. 
- 10.10.20.50:3233(pvt) --> 94.1.1.1:1055(pub)
- 10.10.20.70:5782 (pvt) --> 94.1.1.1:1056 (pub)

### LAN : A group of devices in the same broadcast domain.
###  VLAN : A group of devices in the same broadcast domain. Separated logically instead of physically.
## VLAN Trunking
### 802.1Q trunking
- Take a normal Ethernet frame.
- Add a VLAN header in the frame.
- VLAN IDs- 12 bits long, 4094 VLANs
   - Normal range - 1 through 1005
   - extended range - 1006 through 4094
   - 0 and 4095 are reserved VLAN numbers.
   - Before 802.1Q , there was ISL( Inter Switch Link).

## The native VLAN
- Each trunk has a native VLAN, which doesn't add an 802.1Q header.
- The native VLAN connects switches without a tag.
     - Some devices won't talk 802.1Q.
     - just use the native VLAN!
- Native VLAN should match between switches
- you'll get a msg if the VLAN IDs don't match.
## Layer 3 switches
- A switch(L2) and a router (L3) in the same physical device.
- Switching still opertes at OSI layer2 , routing still operates at OSI layer3.
- The internal route connects to the VLANs over VLAN interfaces , also called Switched virtual interfaces(SVI).
- May need to enable routing on your switch.
    - May operate as an L2 device until enabled.
    - May require a switch restart.
- Doesn't replace a standalone router.
     - Not all designs require extensive routing.
     - You probably use a layer 3 switch at home.

## Interface Configurations
### Basic in ethernet connection
- Speed and duplex 
  - speed : 10/100/10 Gig (Configuration should be equal on both sides.)
- Duplex : half/full
- Automatic and manual .
- Speed config should be same on both sides, but Duplex type can differ on both sides( can affect the efficiency)
### IP address management
- Layer 3 interfaces.
- VLAN interfaces.
- Management interfaces.
- IP address, subnet mask / CIDR block , default gateway, DNS (optional).
- Everything should match as given by our admin.
- If we change any of them , we''ll not be able to connect to other IP devices on this network.
### Port bonding / Link aggregation (LAG)
- Multiple interfaces act like one big interface.( we can use LAG instead of using  eg 4 interfaces forming a loop in between two devices.)
## LACP
- Some switches  will support an automatic configuration of this link aggregation using a protocol called LACP.
- Link aggregation Control Protocol.
- Adds additional automation and management.
- This allows you to simply configure the interface as LACP . plug - in the interfaces and all of the underlying link  aggregation configs will automatically  configure themselves between those two devices. LACP traffic passes through these two devices.

- To communicate , we need to send IP packets , that should not be fragmented.
- Fragmentation slows things down , losing a fragment loses an entire packet and requires overhead along the path.
- Normally the determination of the most optimal maximum transmission unit(MTU) is automatically  done when you first connect to a device.
- Sometimes, filtering and firewalls won't allow the automated process to work properly, we may need to manually configure an MTU on our system.

## Jumbo frames
- Jumbo frames are Ethernet frames with a payload larger than the standard 1500 bytes, typically up to 9000 bytes. They reduce overhead and improve network efficiency, especially in high-speed networks like data centers and storage networks.
- Up to 9216 bytes of an MTU ( 9000 is the accepted form).
- Increase transfer efficiency , fewer packets to switch/route
- Ethernet devices(switches, interface cards etc)  must support jumbo frames.

## Spanning Tree Protocol
- Connect two switches to each other.
   - Create a loop with two cables.
   - They'll send traffic back and forth forever.
   - There's no "counting" mechanism at the MAC layer.
   - This will bring down a network.
- IEEE standard 802.1D to prevent loops  in Bridged(switched) networks (1990): **SPANNING TREE PROTOCOL**.