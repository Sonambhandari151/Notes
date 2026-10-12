- 1st windows: Graphical shell for MS-DOS.
- **Windows 95**: Full integration of windows & DOS(with internet).
- **Windows 2000**: AD + MMC.
---
## Windows Versions
- `Get-WmiObject cmdlet` : query **WMI classes** and retrieve information about a Windows system — hardware, OS details, processes, services, network info,etc.
- `win32_OperatingSystem` : Version & BuildNumber.
- `Win32_Process`: Process listing.
- `Win32_Service`: Services.
- `Win32_Bios`: BIOS info.
- `ComputerName`: info remote computers.
- `Get_WmiObject`: Can be used to start/stop services.

---
## **Windows File Systems**

### **Main File System Types**

1. **FAT12** – Legacy, not used in modern Windows.
    
2. **FAT16** – Legacy, not used in modern Windows.
    
3. **FAT32 (File Allocation Table 32)**
    
    - **Use:** Common on USB drives, SD cards, portable storage.
        
    - **Name meaning:** 32-bit addressing for clusters.
        
    - **Pros:**
        
        - High device compatibility (PCs, cameras, consoles, mobile).
            
        - Cross-platform (Windows, MacOS, Linux).
            
    - **Cons:**
        
        - Max file size **< 4 GB**.
            
        - No built-in data protection or compression.
            
        - No built-in encryption (needs third-party tools).
            
4. **exFAT**
    
    - Designed for flash drives and SDXC cards.
        
    - Removes FAT32 4GB limit.
        
    - Cross-platform support.
        
5. **NTFS (New Technology File System)**
    
    - Default since **Windows NT 3.1**.
        
    - **Pros:**
        
        - Reliable – can recover from failures.
            
        - **Granular permissions** (per file/folder).
            
        - Very large partition support.
            
        - Journaling – logs file changes.
            
    - **Cons:**
        
        - Limited mobile/older device support.
            
    - Used in most **modern Windows installations**.
        

---

## **NTFS Permissions**

### **Key Permission Types**

| Permission Type          | Description                                               |
| ------------------------ | --------------------------------------------------------- |
| **Full Control**         | Read, write, change, delete.                              |
| **Modify**               | Read, write, delete.                                      |
| **List Folder Contents** | View/list folders & execute files (folders only inherit). |
| **Read & Execute**       | View/list files/folders + execute files.                  |
| **Write**                | Add files/folders & write to files.                       |
| **Read**                 | View folder structure & file contents.                    |
| **Traverse Folder**      | Pass through folders without listing contents.            |

**Note:**

- **Inheritance** means files/folders get parent’s permissions automatically.
    
- Can be **disabled** for explicit permissions.
    

---

## **Managing NTFS Permissions – `icacls`**

### **List Permissions**

```cmd
icacls C:\path
```

**Inheritance Flags:**

- `(CI)` = Container inherit (folders)
    
- `(OI)` = Object inherit (files)
    
- `(IO)` = Inherit only
    
- `(NP)` = No propagation
    
- `(I)` = Inherited from parent
    

**Basic Access Rights:**

- `F` = Full control
    
- `M` = Modify
    
- `RX` = Read & Execute
    
- `R` = Read-only
    
- `W` = Write-only
    
- `D` = Delete
    
- `N` = No access
    

---

### **Grant Permission**

```cmd
icacls C:\users /grant joe:F
```

_(Grants `joe` full control over `C:\users` only — no `(OI)(CI)` means no subfolder/file rights.)_

---

### **Remove Permission**

```cmd
icacls C:\users /remove joe
```

---

### **Domain Use**

- Can be applied to **specific users/groups** in Active Directory.
    
- Supports:
    
    - Explicit allow/deny
        
    - Enabling/disabling inheritance
        
    - Changing ownership
        

---

## NTFS vs Share

Windows uses **two layers** of permissions for network file access:

1. **NTFS permissions** – Control access at the filesystem level (applies whether the resource is accessed locally or remotely).
    
2. **Share permissions** – Apply **only** when a folder is accessed over a network share (`\\server\share`).
    

Final access rights are the **most restrictive combination** of both.

---

## **2. NTFS Permissions**

- Stored in the **file system metadata**.
    
- **Always enforced** — whether user is logged in locally or accessing over the network.
    
- Can be applied to:
    
    - **Files**
        
    - **Folders** (and inherited by subfolders/files unless inheritance is disabled)
        
- **Granularity:** Very fine-grained.
    
- **Permission types** (common ones):
    
    - **Full Control** – Read, write, modify, delete, change permissions, take ownership.
        
    - **Modify** – Read, write, delete.
        
    - **Read & Execute** – Open/run files.
        
    - **List Folder Contents** – View folder structure.
        
    - **Read** – View contents.
        
    - **Write** – Create files/folders, modify attributes.
        
- **Advanced permissions** allow controlling specific actions like `Delete`, `Traverse Folder`, `Write Attributes`, etc.
    

---

## **3. Share Permissions**

- Set on a **shared folder**, not on individual files.
    
- Only evaluated **when accessed via SMB/UNC path**.
    
- **Granularity:** Coarse (broad control levels):
    
    1. **Read** – View files and run programs.
        
    2. **Change** – Read + write + delete.
        
    3. **Full Control** – Change + manage permissions.
        

---

## **4. How They Combine**

- **Effective Permission** = **NTFS permission** ∩ **Share permission**  
    (intersection — most restrictive wins)
    
- Example:
    
    - NTFS: **Full Control**
        
    - Share: **Read**
        
    - Result: **Read**
        
- Another example:
    
    - NTFS: **Read**
        
    - Share: **Full Control**
        
    - Result: **Read**
        

---

## **5. Best Practices**

- Set **Share permissions** to `Everyone: Full Control`.
    
- Rely on **NTFS permissions** for actual access control (more granular, more secure).
    
- Document permission changes — improper setup can cause privilege escalation.
    
- In pentesting, check **both layers** when investigating access.
    

---

## **6. Security & Pentesting Notes**

- Misconfigured **share permissions** with lax NTFS permissions = **data exposure risk**.
    
- Anonymous or guest access on shares + weak NTFS = often exploited for:
    
    - Credential harvesting (loose .PST, .config, KeePass files)
        
    - Pivoting (uploading tools/scripts to writable shares)
        
- Tools to enumerate:
    
    - **`net view \\target`**
        
    - **`net share`**
        
    - **PowerView** (`Get-NetShare`, `Invoke-ShareFinder`)
        
    - **SMBMap** in Linux
        

---
