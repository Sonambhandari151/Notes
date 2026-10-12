# 🌐 Networking Primer (Layers 1–4)

## 🔹 OSI vs TCP/IP Models

- **OSI Model (7 layers):** Application, Presentation, Session, Transport, Network, Data-Link, Physical.
    
- **TCP/IP Model (4 layers):** Application, Transport, Internet, Link.
    
- **Key difference:**
    
    - OSI = theory, strict, protocol-independent.
        
    - TCP/IP = practical, flexible, based on real-world protocols.
        
- **Mapping:**
    
    - OSI L7–L5 → TCP/IP Application
        
    - OSI L4 → TCP/IP Transport
        
    - OSI L3 → TCP/IP Internet
        
    - OSI L2–L1 → TCP/IP Link
        

---

## 🔹 PDU (Protocol Data Unit)

- **Encapsulation** = each layer wraps data with its own header.
    
- **Types by layer:**
    
    - Application = Data
        
    - Transport = Segment (TCP) / Datagram (UDP)
        
    - Network = Packet
        
    - Data-Link = Frame
        
    - Physical = Bits
        

---

## 🔹 Addressing

### MAC Address

- **48-bit**, Hex format, unique per NIC.
    
- Used at **Layer 2 (Data-Link)** within a broadcast domain.
    
- Routers replace Layer 2 header at each hop.
    

### IP Address

- **IPv4**: 32-bit, 4 octets, e.g. `192.168.1.1`.
    
- **IPv6**: 128-bit, 16 octets (Hex), e.g. `2001:db8::1`.
    
- **IPv6 advantages**: Huge address space, IPSec support, multicast, simpler headers.
    
- IPv6 addressing types:
    
    - Unicast = one-to-one
        
    - Anycast = one-to-nearest-one in group
        
    - Multicast = one-to-many
        
    - Broadcast = not in IPv6 (replaced by multicast)
        

---

## 🔹 Transport Layer (TCP vs UDP)

|Feature|**TCP**|**UDP**|
|---|---|---|
|Type|Connection-oriented|Connectionless|
|Reliability|Reliable (ACK, seq. nos.)|Unreliable (fire & forget)|
|Speed|Slower (overhead)|Faster|
|Use cases|SSH, HTTPS, File transfer|DNS, Streaming, VoIP|

---

## 🔹 TCP Session Management

### 3-Way Handshake (Connection Establishment)

1. **SYN** → Client → Server
    
2. **SYN + ACK** → Server → Client
    
3. **ACK** → Client → Server
    

✅ Connection established.

### Teardown (Graceful Termination)

1. FIN, ACK
    
2. FIN, ACK
    
3. ACK
    

✅ Connection closed properly.

---

# ✅ Quick Takeaways

- OSI = theory (7 layers), TCP/IP = real-world (4 layers).
    
- Encapsulation = data wrapped at each layer → PDU.
    
- MAC = Layer 2 local addressing, IP = Layer 3 global addressing.
    
- TCP = reliable but slower, UDP = faster but unreliable.
    
- TCP handshake = SYN → SYN/ACK → ACK.
    
- TCP teardown = FIN/ACK → FIN/ACK → ACK.
    

---

