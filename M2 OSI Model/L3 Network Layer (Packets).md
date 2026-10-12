## Network Layer (Layer 3)
- Layer 3 is all about how we are going to forward traffic using Logical addresses 
- Logical address is made up of IPv4 and IPv6. There are other logical addressing scheme.
- *Switching*: (Don't confuse it with switches from layer 2) switching is how we transfer packets to network layer. 
- Route Discovery and selection
- Connection services
- Bandwidth usage
- multiplexing strategy


### Logical addresses
B4 IP (Internet Protocol) becomes standard apple and windows had their own logical addresses 
- Apple - AppleTalk
- Windows/novell - IPX


There are two types of IP addresses 
- IPv4 - dotted octed notation 
- IPv6

### Switching 
Switching is the way of the forwarding the data packets to their correct destination. 

There are three methods of switching
##### Packet Switching 
- Data is divided into packets and then forwarded.
- Each packet is divided into small packets b4 sending over a network and these packets are transmitted independently and can take different routes to the destination where they can be reassembled into the original message. 
- Mostly used on our local devices and small companies


##### Circuit Switching 
- Dedicated communication link is established between two devices 
- constant communication path is maintained for the duration of a conversation
- data only flows from one predefined path unlike the packet switching 
- When we drop the connection and make another connection it can take another route but once the connection is established it is going to follow the same path



##### Message Switching
- Data is divided into messages which may be stored and then forwarded 
- In case of message switching, if for some reason the message can not be delivered, then it will store that data and then send to it unlike packet switching which does not do that if the device is offline it isnt persistent 






### Route Discovery and Selection
- Manually configured as a static route or dynamically through a routing protocol. 
- Uses routing table from router to forward the packets
- A routing table is a data structure used by network routers and hosts to determine the appropriate path for forwarding packets to their destinations. The routing table contains information about the network topology, including routes to various network destinations, and is essential for efficient and accurate packet delivery across networks.

![[Pasted image 20240804175215.png]]


If we want to go from R5 -> R1 there are two ways router 5 to router 1
1. R5->R4->R1
2. R5->R4->R3->R2->R1

if we want to send the data from R5 to R1 then what would be the best way??
If we are using dynamic protocol for this network, all the network device communicates with each other and tell the best path for the data to be transferred. 


this is route discovery and route selection



### Connection Services
provides two additional feature
- ###### Flow control
  Flow control (also presennt in L2 in LLC for different purpose check it out) is used to control the amount of data that is being flowed on our network. If the data that is flowing in the network is less than our current capacity we can request more but if it is more than we can handle.
- ###### Packet Reordering 
  In TCP/IP protocol, the big data is broken down into smaller chunk and these chunks of data is then routed to different locations in our network. When the reciver recieves the data its job is to put all that data in the correct order


>[!Important] flow control in Data Link Layer vs Network Layer
>*Data Link Layer :* Flow control manages the rate at which data frames are sent from one node (e.g. a computer or router) to another over physical link (like Ethernet or wifi)
>Some Mechanism
>Stop-and-wait protocol: Sender transmits frame and then wait for receiver to acknowledge b4 sending next frame
>Sliding window protocol: Allows multiple frames to be sent before needing an acknowledge, controlling the flow more efficiently by adjusting window size
>*Network Layer :* manages the flow control between routers
>- **Congestion Control:** Protocols like TCP, though primarily associated with the Transport Layer, rely on underlying network layer mechanisms to detect and avoid network congestion by adjusting the rate of packet transmission.
>- **ICMP Source Quench Messages:** Though less common today, ICMP messages were once used to inform the sender to slow down transmission due to congestion.

### Internet Control Message Platform (ICMP)
sends error message and operational information to an IP destination.
The most commonly used one is ping
![[Pasted image 20240804181246.png]]


tracert is another example of it
![[Pasted image 20240816155937.png]]


>[!NOTE]
>Each time we use traceroute for same website we are getting diferent values of routers through which packets went because in packet switching multiple packets can go through different paths



### Examples
- Router
  Switches has mac addresses whcih it uses to route traffic in the different devices connected to it. It is often used in internal network. 
  Router is used in the Internet to connect different private networks to the main network
![[Pasted image 20240804182022.png]]

it acts as a gateway to the internet and it only allows the data that is intended for it to pass in the network reject all others for example red wants to talk to blue it will send a blue packet and then the router will forward the packet accordingly

![[Pasted image 20240804182217.png]]


this is the symbol of router


Multilayer switch
It is combination of router and switch
![[Pasted image 20240804182303.png]]





  