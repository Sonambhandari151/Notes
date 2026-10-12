sudo nmap -p80 --script http-enum <IP>

- This command uses **Nmap** (Network Mapper) to **scan a web server** running on port **80** and identify directories, files, or services exposed via HTTP.

-  nmap -oA nmap-vuln -Pn -script vuln -p 80,135,139,445,3389 10.10.148.14 : to chec for vulnerability.