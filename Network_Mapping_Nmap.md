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
```bash
nmap -sn 192.168.143.129
