# Network Mapping & Vulnerability Reconnaissance with Nmap

## Objective
Perform network reconnaissance, host discovery, port scanning, and OS fingerprinting against an isolated Windows target host within a VMware virtual lab environment.

---

## 1. Environment & Target Setup
- **Auditor Machine:** Kali Linux (x86_64, VMware)
- **Target Host:** Windows 7 Ultimate (`192.168.143.129`)
- **Networking Mode:** Isolated NAT / Host-Only Subnet

---

## 2. Reconnaissance & Scanning Methodology

### A. Host Discovery (Ping Sweep)
Verified that the target host is active without performing port scans:

B. Stealth SYN & Fast Port Scan
Conducted a fast TCP SYN scan on the most common ports:

nmap -sS -F 192.168.143.129
Discovered Open Ports:

135/tcp (Microsoft RPC Endpoint Mapper)

139/tcp (NetBIOS Session Service)

445/tcp (Microsoft-DS / SMB)

C. Service Version & Banner Grabbing
Enumerated running services and application versions:

nmap -sV 192.168.143.129
D. Aggressive Operating System Detection & Script Scan
Executed an aggressive scan to fingerprint the OS and evaluate SMB configurations:

Bash
nmap -A -T4 192.168.143.129
nmap -sn 192.168.143.129 <img width="988" height="471" alt="image" src="https://github.com/user-attachments/assets/99ccb397-e390-4da8-a3f8-8f6b4e012563" />
