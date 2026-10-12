### Layer 1 (Physical Layer)
Bottom of the OSI Model.
Layer where the transmission of bits across the network occurs. Transmits bits through light or electric voltage. Light on or off being 1 / 0 and
0 voltage being 0 and 5 or -5 being 1.
- This layer tells which kind of cable/media is being used to transmit data that is bits form one location to another. This tells whether using an ethernet network, fibre optic network, copper cables even radio frequency for wifi
- The light is used in fibre optic cable 
- electric voltage in cat5 or cat6 ethernet cables made up of coppers
- using wifi to send data (radio frequency)



>*Clock cycle* : is the basic unit of time in a computer's central processing unit (CPU). 
##### Transition Modulation
Transition modulation is a way of sending data signals where the information is encoded based on changes in the signal rather than the signal's actual levels. Here's an easy way to understand it:

Imagine you're blinking a flashlight to send a message in Morse code. Instead of just thinking about whether the light is on or off, you focus on when you switch the light from on to off or off to on. Each change (or transition) carries the information.

Cables are also part of the physical layer. In case of Cat5 or Cat6 cable the connector is RJ45. The way that connector is wired based on two standards

- TIA/EIA-568A
- TIA/EIA-568B

this tells us whether we are using
- crossover cables 
  ![[Pasted image 20240804121051.png]]
- straight through cables
  ![[Pasted image 20240804121104.png]]



-  we use topology like bus, star etc to physically create for layer 1 (we discuss this in layer 1 )
- another issue of layer 1 is *synchronizing our connections*
  how does recieving end know i is ready to accept ones and zeroes?
  ### Asynchronous Communication

- **Definition**: Data is sent one byte or character at a time, with each byte framed by start and stop bits.
- **How it works**:
    - Each byte of data starts with a **start bit** (usually a low signal) to signal the beginning of the data.
    - The actual data bits follow.
    - A **stop bit** (usually a high signal) indicates the end of the byte.
- **Characteristics**:
    - No need for a shared clock between the sender and receiver.
    - The timing between bytes can vary; there can be gaps.
    - Simple and cheap to implement, but less efficient for high-speed communication.
- **Example**: Communication between a computer and a serial device like a mouse or keyboard.

### Synchronous Communication

- **Definition**: Data is sent in a continuous stream, synchronized by a shared clock signal between the sender and receiver. So we can say like when a sec pass we are going to start transmission and stop after 2 sec.
- **How it works**:
    - Both the sender and receiver share a clock signal to keep their data in sync.
    - Data is sent as a continuous stream without start and stop bits for each byte.
    - Special synchronization characters or sequences might be used to align the communication.
- **Characteristics**:
    - Requires a shared clock signal or an agreed timing mechanism.
    - More efficient for high-speed data transfer since there are no start and stop bits for each byte.
    - More complex and expensive to implement but allows for higher data rates.
- **Example**: Communication between network devices in a Local Area Network (LAN), like Ethernet.



Along for it to be async or sync u are also going to figure out the bandwidth of the cable. One is called broadband and one is called baseband





### Data Link layer (frames)
Roles of data link layer
- Converts bits from physical layer to frame
- Provide error detection and correction
- adds mac address to identify unique devices
- Flow Control - controls the amount of the data that is transferred
- Access control 
- Logical Link Control (LLC)



##### Mac address (Media access control)
Physical addresses of the network device which is used to uniquely identify that Network Interface Card *NIC* on the internet

MAC address is made up of 48 bits character. With first 24 bits containing the information about the vendor and the other 24 bits uniquely identifies the card.

![[Pasted image 20240804162213.png]]


##### Logical Link Control (LLC)

- In Layer 2 we discuss about the logical design of the network not physical (done in layer 1) layer 2 manages the flow of data so that more than 2 devices are not talking over each other 
- In LLC if the sender is sending a lot of data LLC will tell the sender to slows down and some data is corrupted it will ask to resend that data or if a frame is missing 
- It also provides a basic error detection. If the last bit is 0, the sum of other bits (frame is usually made of 0 and 1) is even then it is correct or for 1 it must be odd but if it doesnt meet the requirements then data is corrupted. 

#### Communication in Data - Link Layer
- *Isochronous*: Network devices use a common reference clock and create time slot for transmission.
- *Synchronous*: Network devices agree on clocking method to indicating the beginning and end of frame and can use control characters.
- *Asynchronous*: Each devices reference internal clock and use start and stop bit for synchronization 



##### Examples
- Network Interface Card (NIC): 
- Bridges
- Switches

Bridges are dumb machine they just transmit all the data they recieve from the port but switches are smarter they can detect what kind of device is connected to the port based on what is the mac address of the device.
these topics are for later 
- CAM tables usage of MAC address
- Switching mechanism across the network 