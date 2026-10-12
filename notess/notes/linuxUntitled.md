- SUDO- super user do
   temporary access as a root user
   e.g.- sudo cat /etc/shadow

- sudo su -  --> for switch user to switch into root  
- pwd- present working directory

- cd ..  -> for backward change directory(/)
- cd dir/ --> forward change
- ls - to list content of dir
    -eg -> ls Desktop/
- cd /etc/ --> to access folder of another dir
- or ls /etc/ --> to directly access
- mkdir dirname --> make a dir
- rmdir dirname/ --> to remove
- ls -la -->- `ls`: Lists directory contents.
   - `-l`: Displays the list in a long format, providing detailed information such as permissions, number of links, owner, group, file size, and modification date.
   - `-a`: Shows all files, including hidden ones (files that start with a dot `.`)
   - echo "hi" > test.txt --> to redirect output hi to a file name test.txt
   - cp test.txt sonam/   --> to copy to sonam dir
   - rm sonam/test.txt --> to remove
   - updatedb --> to update database
   - locate keyword --> to locate the file in database
   - passwd --> to change password
   - man --> manual for commands
   - In ls -la 
     if start with
     - - :  file
     -  d: dir
     - rwx : read, write, execute
  - chmod : change mode
  - chmod +xrw/ chmod 777 : to provide read, write, execute access to a file
  - adduser : to add another user to system
  - adduser name
  - passwd are on shadow file
  - cat /etc/passwd : to check user
  -
  # Common Network Commands                           
  - ifconfig /ip a : configure and display network interface information
  - iwconfig : configure wireless network interfaces and display their current status
  - ping : verifying network connectivity and determining if a specific IP address or hostname is reachable.
  - arp -a : (address resolution protocol)show ip address it talks to & mac address associated with it
  - eg : arp -a/ip n (n-neighbour): 
     ? (192.168.1.1) at 00:11:22:33:44:55 [ether] on eth0
     ? (192.168.1.100) at 00:aa:bb:cc:dd:ee [ether] on wlan0
     - **?**: A placeholder for the hostname (if resolved) or a question mark if the hostname is not available.
      - **192.168.1.1**: The IP address of the device on the local network.
     - **00:11:22:33:44:55**: The MAC (Ethernet) address of the device.
     - **[ether]**: Denotes that the MAC address belongs to an Ethernet device.
     - **eth0** or **wlan0**: The network interface (Ethernet or wireless) used for communication with the device.
 - netstat -ano : show active connections running  on your machine
     - **`-a`**: Shows **all** active connections (both listening and non-listening).
     - **`-n`**: Displays addresses and port numbers in **numerical** form (without resolving hostnames or service names, speeding up output).
    - **`-o`**: Shows the **timer** (or **ID of the owning process** in some versions of `netstat`, particularly on Windows)-  
- route/ ip r :determines how network traffic is 
  directed from one device to another, whether  
    on the same local network or across the internet.
     - **Destination**: The network or host destination for which the route is valid.
    - **Gateway**: The next-hop router's IP address used to reach the destination.
    - **Genmask**: The subnet mask associated with the destination.
    - **Flags**: Route flags such as `U` (up), `G` (gateway), or `H` (host).
    - **Metric**: A value used to prioritize routes (lower values are preferred).
    - **Iface**: The network interface through which packets for the destination will be sent (e.g., `wlan0`, `eth0`).
# Tools

- apt update && apt upgrade : command in   Linux is used to update the package lists and upgrade  installed packages
- apt install python-pip: to install python 
- pimpmykali
    - sudo su -
    - cd /opt
    - git clone link
    - cd pimpmykali/
    - ls
    - ./pimpmykali.sh
    - 0
    - N
- sudo apt install gedit: text editor
- gedit test.txt: text editor
# viewing,creating,editing
- echo "txt" > file: create file
- echo "txt" >> file : append
- touch file: create file
- nano file: create + text editor
- gedit file: text editor

# scripting with bash
- ping ipAdd -c 1: to ping once
- ping ipAdd -c 1 > ip.txt : to copy to txt file
- cat ip.txt: to read
- cat ip.txt | grep "64 bytes":reads the contents of the file `ip.txt`, and filters out only the lines that contain the text "64 bytes".
- |(pipe): takes output from left cmnd & input it to right cmnd.
- cat ip.txt | grep "64 bytes" | cut -d " " -f 4 | tr -d ":"
      **`| cut -d " " -f 4`**:  
        The `cut` command is used to split the lines based on a delimiter and extract specific fields.

     - -d " ": Sets the delimiter to a space character (`" "`).
     - **`-f 4`**: Extracts the 4th field (or column) from each line
     - **`| tr -d ":"`**:  
      The `tr` command translates or deletes characters.
     **`-d ":"`**: Deletes the colon (`":"`) character from the extracted field.

- mousepad ipsweep.sh: for editing shell script
- IN EDITOR
     - #!/bin/bash : tells the system to use the Bash shell to interpret the script.
     - if ["$1" == " "]
     - then
     - echo "You forgot an IP address!"
     - echo "Syntax : ./ipsweep.sh ipAdd"
     - else
     - for ip in `seq 1 254` ; do
     - ping -c 1 $1.sip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &:
          - `ping -c 1`: Sends only one ICMP echo request (a ping) to the target IP address.
          - `$1.$ip`: Constructs the full IP address by combining the argument `$1` (e.g., `192.168.1`) with the current loop value `$ip` (e.g., `1`, `2`, ..., `254`). So, it pings IPs like `192.168.1.1`, `192.168.1.2`, ..., `192.168.1.254`.
     - done
     - fi
    - ./ipsweep.sh ipAdd: sweep the range `192.168.1.1` to `192.168.1.254` and print out the IP addresses that respond to the ping.
    - ./ipsweep.sh ipAdd >ips.txt: to store in txt file
    - nmap ipAdd :network scanning tool. This command scans the IP address (`ipAdd`)
    - for ip in $(cat ips.txt); do nmap $ip; done: Runs `nmap` on each IP address found in the file.


- ### What this script does:

- The script checks if an IP address is provided. If not, it asks for one.
- If provided, it pings all IP addresses from `ipAdd.1` to `ipAdd.254`, extracts the responsive IPs, and displays them (or saves them to a file).
- It then allows you to scan the saved IP addresses using `nmap`, automating the process of identifying active hosts and scanning them for open ports.

This script can be useful in network administration or penetration testing to identify live hosts on a local network and further explore their vulnerabilities with `nmap`.