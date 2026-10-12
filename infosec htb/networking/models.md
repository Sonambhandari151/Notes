
## **OSI Model (ISO/OSI Layer Model)**

**Definition:**

- **OSI** = _Open Systems Interconnection_
    
- Published by **ITU** and **ISO**.
    
- A **reference model** to describe how systems communicate over a network.
    
- Has **7 layers**, each with distinct tasks.
    

---

### **The 7 Layers (Top to Bottom)**

1. **Application** – Interfaces for user applications (HTTP, FTP, SMTP).
    
2. **Presentation** – Data formatting, encryption, compression.
    
3. **Session** – Starts, manages, and ends communication sessions.
    
4. **Transport** – Reliable delivery of data (TCP) or fast delivery (UDP).
    
5. **Network** – Logical addressing and routing (IP).
    
6. **Data Link** – Physical addressing (MAC), error detection.
    
7. **Physical** – Hardware signals, cables, connectors.
    

---

## **TCP/IP Model**

**Definition:**

- **TCP/IP** = _Transmission Control Protocol / Internet Protocol_
    
- A **protocol family** that powers the Internet.
    
- Includes many protocols beyond TCP and IP, e.g.:
    
    - **ICMP** (ping, error messages)
        
    - **UDP** (fast, connectionless data transfer)
        
- Responsible for **transporting and routing** data packets in private or public networks.
    

---

### **Layers of TCP/IP Model**

1. **Application** – Protocols like HTTP, FTP, DNS, SMTP.
    
2. **Transport** – TCP, UDP (manages how data is sent/received).
    
3. **Internet** – IP, ICMP (addresses and routes packets).
    
4. **Network Access / Link** – Ethernet, Wi-Fi (handles physical transmission).
    

---
Here’s your **Packet Transfers** section rewritten into clean, clear, and exam-friendly notes, matching the style we’ve been using:

---

## **Packet Transfers & Encapsulation**

### **Protocol Data Units (PDUs)**

- In layered communication systems, **each layer** exchanges data in its own format — called a **PDU**.
    
- Example: Browsing a website → data passes through the layers on the sender side → sent across the network → travels back up the layers on the receiver side.
    

---

### **Process (Sender to Receiver)**

1. **Application Layer** (e.g., HTTP): Creates the data for transfer.
    
2. **Encapsulation**:
    
    - Each lower layer **adds its own header** (control & identification info) to the PDU from the layer above.
        
    - This continues down to the Physical/Link layer.
        
3. **Transmission**:
    
    - At the lowest level, data is sent as bits over cables or wireless.
        
4. **Decapsulation** (Receiver side):
    
    - Each layer reads its header, processes the data, and removes the header before passing the PDU upward.
        
5. **Application**:
    
    - The receiving software finally uses the data.
        

---

### **PDU Naming by Layer**
![[Pasted image 20250812220641.png]]
---

### **Encapsulation Example**

**Sender Side:**

- Data → **TCP Header** added → **IP Header** added → **MAC Header** added → Binary Transmission.
    

**Receiver Side:**

- Reverse process — headers removed layer by layer until original data is delivered to the application.
    

---

### **Why This Matters for Pentesting**

- **TCP/IP model**: Quickly shows _how_ a connection is established.
    
- **OSI model**: Allows detailed breakdown and troubleshooting of each step.
    
- In network traffic analysis, you’ll often:
    
    - Capture packets
        
    - Identify layers & headers
        
    - Spot anomalies or malicious activity
        

---
## OSI Model –
- **Goal:** Create a **standard reference model** to enable communication between different technical systems using various devices and technologies.
    
- **Structure:**
    
    - **7 hierarchical layers**, each performing specific, well-defined tasks.
        
    - Layers 2–4 → **Transport-oriented**
        
    - Layers 5–7 → **Application-oriented**
        
- **Principle:**
    
    - Each layer **offers services** to the layer above.
        
    - Each layer **uses services** from the layer below.
        

---

### **7 Layers and Their Functions**

| **Layer** | **Name**     | **Function**                                                                                                             |
| --------- | ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **7**     | Application  | Handles **input/output** of data and provides application services (HTTP, FTP, SMTP).                                    |
| **6**     | Presentation | Converts **system-dependent** data into a format **independent** of the application (encryption, compression, encoding). |
| **5**     | Session      | Manages **logical connections** between systems; prevents breakdowns; controls dialog between devices.                   |
| **4**     | Transport    | **End-to-end control** of transferred data; detects/avoids congestion; segments data streams (TCP, UDP).                 |
| **3**     | Network      | Establishes connections in circuit-switched networks; forwards packets in packet-switched networks (IP, routing).        |
| **2**     | Data Link    | Provides **reliable, error-free** transmission; converts bitstreams into **frames**; uses MAC addressing.                |
| **1**     | Physical     | Defines **physical transmission** (electrical, optical, or electromagnetic signals) over wired or wireless media.        |

---

### **Communication Flow**

1. **Sending system:** Processes data from **Layer 7 → Layer 1** (encapsulation).
    
2. **Receiving system:** Processes data from **Layer 1 → Layer 7** (decapsulation).
    
3. **Both sender and receiver** run through all 7 layers during communication.
    

---

### **Key Points**

- Each layer has **precise tasks** and **defined interfaces** to adjacent layers.
    
- OSI helps **trace and visualize** how a connection is structured and established.
    
- Communication security, reliability, and performance depend on correct operation at **every layer**.
    

---


## **Network Layer (Layer 3 – OSI)**

### **Main Role**

- **Controls the exchange of data packets** between devices across different networks.
    
- **Provides logical addressing** and **routing** so packets can travel from source to destination.
    
- Packets are **forwarded node-to-node** (e.g., via routers) until they reach the final destination.
    
- Intermediate nodes **do not process higher-layer data** — they only handle Layer 3 info (addresses, routing).
    

---

### **Key Functions**

1. **Logical Addressing**
    
    - Assigns unique addresses (e.g., IPv4, IPv6) to devices.
        
    - Ensures correct delivery between different networks.
        
2. **Routing**
    
    - Selects the best path for packet delivery.
        
    - Uses routing tables built from routing protocols.
        

---

### **Important Protocols at Layer 3**

- **IPv4 / IPv6** – Core addressing and delivery protocols.
    
- **IPsec** – Provides encryption and authentication for IP traffic.
    
- **ICMP** – Error reporting & diagnostics (e.g., ping).
    
- **IGMP** – Manages multicast group memberships.
    
- **RIP** – Distance-vector routing protocol.
    
- **OSPF** – Link-state routing protocol.
    

---

### **Why Routing is Needed**

- Sender and receiver may:
    
    - Be in **different subnets**
        
    - Use **different or incompatible addressing schemes**
        
- Direct communication may not be possible → packets go through **intermediate nodes (routers)**.
    
- Routers:
    
    - Assign a **new intermediate destination**
        
    - Forward packet to the next hop
        
    - Repeat until the packet reaches its final destination
        

---
