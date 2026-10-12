Dividing line between the upper layer and the lower layers of OSI model
### TCP (Transmission control protocol)
connection-oriented protocol that relies in a reliable way to transport segment across the network

>[!NOTE]
>Connection-oriented means that when the data is send over the network It expects to recieve an aknowledgement and if it doesnt recieve an ack for that data, it will ask for the data again. 

### TCP Acknowledgement Mechanism:
1. **Sending Data Segments**:
    - When a TCP sender transmits data, it breaks the data into segments, each with a unique sequence number.
    - The sequence number indicates the byte position of the first byte in the segment within the overall data stream.
2. **Receiving and Acknowledging Segments**:
    - The TCP receiver acknowledges the receipt of data segments by sending back an ACK (Acknowledgment) segment.
    - The ACK segment contains an acknowledgment number, which indicates the next byte the receiver expects to receive. This implicitly acknowledges all previous bytes up to, but not including, this byte.
3. **Cumulative Acknowledgement**: 
    - TCP uses cumulative acknowledgments, meaning that an ACK for a segment implies that all previous segments have been received correctly.
    - For example, if the receiver sends an ACK with an acknowledgment number of 101, it means that the receiver has successfully received all bytes up to byte 100 and is expecting byte 101 next.

[Check this out](https://www.youtube.com/watch?v=LyDqA-dAPW4)


![[Pasted image 20240804183214.png]]



This is how we are going to transfer the data. 
This is called three way handshake.

this is done once before the data transmission start between client and server 

in TCP if any packet is missing the reciever will not send an ack for that segment and sender will know thtat the data is missing



### UDP (User Datagram Protocol) 
UDP is a connectionless protocol and it is unreliable but it is fast. It is used in live stream. If dropped sender will be unaware that some datagrams(term used in Layer 4 in case of UDP) is missing


![[Pasted image 20240804184740.png]]





in tcp segment sequencing means that it is going to arrange the data that it recieves in the correc order but UDP will broadcast it the way it recieves the data 



### Windowing
Allows the client to adjust the amount of data in each segment. If server is sending data and it is getting a lot of retransmission it knows that the client is cant handle big segments so it will close (bits transmitted over segment) a lil bit

and if it is not getting any re transmission it means it is not going fast enough so it will increase the window size


It will continue to do that many times until it reaches to the point where it gets the best bandwidth it can 

Windowing sends information while minimizing retransmission and maximizing throughput



### Buffering
Occurs when device allocates memory to store segment if the bandwidth is not available

![[Pasted image 20240804185512.png]]



Imagine we are R5 and R4 is reciving data from all the inputs but it is total 210 mbps it means it will have to store a lot of content in its buffer (a storage inside r4) that it will send when the route from R4 to R5 is free due to low bandwidth capacity between them


usually the network doesnt work on 100 % of its capacity so there is no point to use expensive 210 mbps cable between them. It is a job of network engineer to see through that


### Examples
- TCP
- UDP
- WAN accelerator
- load balancers & firewall