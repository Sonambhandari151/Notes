## Encapsulation
Process of putting headers and trailers around some data


## Decapsulation
Is removing these headers from these different layers
![[Pasted image 20240816172205.png]]


when we are going down the osi model encaplution and when going up decapsulation


### PDU (protocol data unit)
A single unit of information is transmitted in a computer network

For example L7 PDU means layer 7 PDU (Protocol data unit)


# Let's transmit Data
When we send a packet it encapsulates that packet with different data depending on what kind of layer it is going through
- Layer 7 Application layer adds data like the method of protocol and methods associated with it
- Layer 6 Presentation layer includes data like the ASCII character encryption algo format of image or video etc
- Layer 5 Session layer encapsulates the data with the session ids and other stuff related to session 

But the main encapsulation are from layer 4 Transport Layer
>[!Note] 
>Encapsulating the data is same like writing a letter and then keeping it in envelope with sender's address reciever address and some additional metadata on it
>and decapsulating the data is like removing the letter from envelope

#### Transport Layer (layer 4)
There are mainly two protocols in transport layer 
- TCP (transmission control protocol)
  For tcp the byte size is 20 and it contains the following data
  ![[Pasted image 20240817201857.png]]
It has 10 mandatory fields 
The reserved data is always set to 0
The source port and destination port is important because it makes the packet goes through a particular firewall that is responsible for passing the data.

another important field is control flag
![[Pasted image 20240817202122.png]]


In three way handshake we discussed about the Syn and Ack control flags.

![[Pasted image 20240817202309.png]]

![[Pasted image 20240817202322.png]]

![[Pasted image 20240817202332.png]]
![[Pasted image 20240817202340.png]]This is the last flag that appears when the host is ready to tear down the session


![[Pasted image 20240817202406.png]]

for example if you send a connection request to a server that is not ready to accept any new connection it can send a reset flag packet which means that it isnt expecting any new connection

![[Pasted image 20240817202516.png]]
mostly this flag is added at the beginning or end of the data transfer
PUSH is used by the sender to indicate the data with a higher priority level

![[Pasted image 20240817202547.png]]
Urgent is used to tell the rcipient to process it immediately and ignore anything else that is in the queue

With Urg it fails the packet FIFO priority order so it must be used only by particular application when necessary

![[Pasted image 20240817202941.png]]


This is UDP headers and it contains the 8 byte headers. UDP is unreliable and connection less protocol so its header is significantly smaller than TCP. 

#### Network Layer (Layer 3)
includes data related to IP address 

 ![[Pasted image 20240817203257.png]]

#### Data Link Layer (Layer 2)
![[Pasted image 20240817203722.png]]

Data link layer contains information like the destination MAC address source MAC address and stuff like that But remember MAC address is not sent over a local network. So for example a packet may travel from one router to another the mac address will be of the router and not the client that is sending the request so we cannt uniquely identify the mac address

MAC Address - physical address that is used to identify a network card on a local area network

EtherType - Used to indicate when protocol is encapsulated in the payload of a frame. Like IPv4 or IPv6

A frame that is being sent on layer 2 also contains a payload 
using VLAN 42 bytes 
without VLAN 46 Bytes

![[Pasted image 20240817204129.png]]

![[Pasted image 20240817204209.png]]


The data is being transmitted in form of 1's and 0's containing all above header.

Now a router will translate that data and check for the ethernet header.

If the mac address is in one of its connected switch port it will send it to switch but if not it will forward that data to its default gateway that is a router after encapsulating the data 


>[!NOTE]
>A hub is a network hardware device for connecting multiple Ethernet devices together and making them act as a single network segment. It operates at the physical layer (Layer 1) of the OSI model. All devices connected by a hub are in a single collision domain and a single broadcast domain, so it must use half-duplex for communication and CSMA/CD. A switch operates at layer 2 of the OSI model and provides more advanced functionality than a hub. A bridge also operates at layer 2. It connects two separate network segments. A router operates at layer 3 and forwards data packets among networks.