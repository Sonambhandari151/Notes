# strings
- mkdir python
- cd python/
- gedit first.py(use export DISPLAY=:0 in case of error opening gedit)
- #./bin/python3
- python3 first.py: to display file content edited
# math
gedit math.py&(can be named anything): maths operations
- python3 math.py: to execute
# variables & methods
- gedit script.py: to create script
- python3 script.py
- str(var): to change to string
- script.py& : & is used for: After starting `gedit`, you can continue typing other commands in the terminal because `gedit` runs in the background, rather than blocking the terminal
# functions
- cd python/
- gedit script.py&
- pyhton3 script.py
#### lists are mutable: changeable[]
#### tuples are immutable: Do not change()


# modules
- import sys
  - print(sys.version)
  - sys.exit()
- from datetime import datetime as dt
  - print(dt.now())

# advanced strings
eg:
- sentence = "this is a sentence"
- print(sentence[:4]) : this
- print(sentence.split()): ['This' , 'is', 'a', 'sentence.' ]
- sen_split = sen.split()
- sen_join = ' '.join(sen_split) : 
- print(sen_join): This is a sentence.
- quote = "he said,  \"give me all your money\" "
- print(quote):  "he said, "give me all your money" "
- var.strip(): to remove spaces
- print("A" in "Apple) : True
- movie = "The Hangover"
- print("My fav movie is {}. " .format(movie))

# socket
- gedit s.py&: script
- IN editor:
     -  #!/bin/python3
     - import socket
     - HOST = '10.0.2.15' : target host(server) the client will connect to
     - PORT = 7777 ; port number on which the client will attempt to connect to the server.
     - s= socket.socket(socket.AF_INET , socket.SOCK_STREAM)
         - This creates a **TCP/IP** socket. 

        - **`AF_INET`**: Refers to the IPv4 address family (so it's an IPv4 connection).
        -  **`SOCK_STREAM`**: Specifies that this is a TCP socket (as opposed to `SOCK_DGRAM` for UDP).
     - s.connect((HOST, PORT))
 - python3 s.py  : attempt to create a TCP connection to the server at IP `10.0.2.15` and port `7777`
 - in new terminal:` nc -nvlp 7777` :sets up **Netcat** (often abbreviated as `nc`) to act as a simple TCP or UDP server, listening for incoming connections on port **7777**.
     - nc: to create a TCP server here
     - **`n` (No DNS Lookup)**:
    - **`-n`** tells Netcat to avoid resolving DNS names. It ensures that only numeric IP addresses (and no hostnames) are used, which can make the operation faster.
    - **`-v` (Verbose Mode)**:
    - **`-v`** enables verbose mode, which means Netcat will provide more detailed output in the terminal about what it’s doing.
    - **`-l` (Listen Mode)**:
    - **`-l`** tells Netcat to operate in **listen mode**, making it act as a server that listens for incoming connections on a specific port.
    -  Without the `-l` option, Netcat works as a client and connects to remote hosts. With `-l`, it waits for incoming connections from remote clients.
    - `-p 7777` (Port Specification)
 ## How It All Works:

1. **Client (`s.py`)**:
    
    - The Python script (`s.py`) acts as a **TCP client**. It tries to establish a connection to the IP address `10.0.2.15` on port `7777` using the socket created.
2. **Server (`netcat`)**:
    
    - In the new terminal, `netcat` is running as a **TCP server**, listening for incoming connections on port `7777`.
3. **Connection**:  
    When you run `python3 s.py`, the client script connects to the `netcat` server on `10.0.2.15:7777`. Once connected, you can send and receive data between the two.



# Building a Port Scanner

### 1. **Importing Libraries**


`import sys import socket  from datetime import datetime`

- `sys`: Used for handling command-line arguments and system exit functions.
- `socket`: This is the core library for networking, allowing the program to create network connections.
- `datetime`: Used to print the current time, which helps track when the scan starts.

---

### 2. **Input Validation**

python

Copy code

`if len(sys.argv) == 2:     target = socket.gethostbyname(sys.argv[1]) else:     print("Invalid number of arguments.")     print("Syntax: python3 scanner.py <ip>")     sys.exit()`

- `sys.argv` is used to read command-line arguments. `sys.argv[1]` represents the first argument passed (which should be the target hostname or IP address).
- If the user doesn't provide exactly one argument, the program prints an error message and exits using `sys.exit()`.
- If an argument is provided, `socket.gethostbyname(sys.argv[1])` translates the hostname to an IP address. If the user provides an IP directly, it remains unchanged.

---

### 3. **Display Information**

python

Copy code

`print("-" * 50) print(f"Scanning target {target}") print(f"Time started: {str(datetime.now())}") print("-" * 50)`

- This section is for aesthetic purposes. It prints a horizontal line, followed by the target IP address and the current time when the scan begins. This information is useful for users to track which target is being scanned and when.

---

### 4. **Port Scanning**

python

Copy code

`try:     for port in range(50, 85):         s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)         socket.setdefaulttimeout(1)         result = s.connect_ex((target, port))                  if result == 0:             print(f"Port {port} is open")         s.close()`

- The `try` block contains the scanning logic. The `for` loop iterates over ports from 50 to 85 (a total of 35 ports).
- `socket.socket(socket.AF_INET, socket.SOCK_STREAM)` creates a new socket object using the **AF_INET** address family (IPv4) and **SOCK_STREAM** (TCP protocol).
- `socket.setdefaulttimeout(1)` sets a timeout of 1 second. If the connection attempt takes longer than that, it will fail.
- `s.connect_ex((target, port))` tries to connect to the specified `target` and `port`. If it returns `0`, the connection is successful, meaning the port is open.
    - If `result == 0`, it prints that the port is open.
    - After each attempt, `s.close()` is called to close the socket, whether the port is open or not.

---

### 5. **Exception Handling**

python

Copy code

`except KeyboardInterrupt:     print("\nExiting program.")     sys.exit()  except socket.gaierror:     print("Hostname could not be resolved.")     sys.exit()  except socket.error:     print("Couldn't connect to server.")     sys.exit()`

- **`KeyboardInterrupt`**: This catches the interrupt signal when the user presses `Ctrl+C`. It allows for a clean exit without error messages.
    
- **`socket.gaierror`**: This catches an error related to DNS resolution. If the program cannot resolve the hostname into an IP address (e.g., if a typo is made in the hostname), this error is raised.
    
- **`socket.error`**: This catches general socket errors, such as if the target server is unreachable or refuses the connection.



	
