## Hunting subdomains
### domain

A **domain** is the primary address of a website or network resource. It identifies a specific organization or entity on the internet.  
**Example**:

- `example.com` is a domain.
### subdomain

A **subdomain** is a prefix added to a domain to organize or separate sections of a website or service, functioning as a subset of the main domain.  
**Example**:

- `blog.example.com` (`blog` is the subdomain of `example.com`).
#### tools for subdomain hunting
- `sublist3r`: open-source tool designed for **subdomain enumeration**
    - apt install sublist3r: installation
    - sublist3r -d eg.com: to search subdomain
  - `crt.sh`:**Subdomain Enumeration**:
    - crt.sh reveals subdomains that have SSL/TLS certificates issued, even if they are not directly visible or publicly listed.
 - `OWASP AMASS`: **Subdomain Enumeration**
 - tomnomnom httprobe:  **Subdomain Enumeration** ( list of subdomains or URLs  alive and responding via HTTP or HTTPS.)
 - `builtwith`: **BuiltWith** is a powerful web tool used for identifying the technologies and services powering a website.
wappalyzer: identify the technologies used by websites.
whatweb : identify the technologies used by websites.

## scanning and enumeration
- arp-scan -l: tool to discover devices on a local network by sending ARP requests.
 - l(localnetwork)
- netdiscover -r : discover devices on a network using ARP requests(sudo netdiscover -r <IP-range>)