# Network Mapping & Vulnerability Reconnaissance with Nmap

## Objective

Perform network reconnaissance, host discovery, port scanning, service enumeration, OS fingerprinting, and basic vulnerability assessment against an isolated Windows target host within a VMware virtual lab environment.

---

## 1. Environment & Target Setup

| Component           | Details                         |
| ------------------- | ------------------------------- |
| **Auditor Machine** | Kali Linux (x86_64, VMware)     |
| **Target Host**     | Windows 7 Ultimate              |
| **Target IP**       | `192.168.143.129`               |
| **Networking Mode** | Isolated NAT / Host-Only Subnet |
| **Scanning Tool**   | Nmap                            |

> **Lab Scope:** All scans were performed against an isolated virtual machine in a controlled VMware lab environment.

---

## 2. Reconnaissance & Scanning Methodology

### A. Host Discovery — Ping Sweep

The target host was first checked for availability using Nmap host discovery. This verifies whether the host is online before performing detailed port enumeration.

```bash
nmap -sn 192.168.143.129
```

**Result:** Target host was detected as online with minimal latency (~0.0036s).

---

### B. TCP SYN Scan — Common Ports

A TCP SYN scan was performed against the most common ports to identify exposed TCP services.

```bash
nmap -sS -F 192.168.143.129
```

**Discovered Open Ports:**

| Port      | Service       | Description             |
| --------- | ------------- | ----------------------- |
| `135/tcp` | Microsoft RPC | RPC Endpoint Mapper     |
| `139/tcp` | NetBIOS-SSN   | NetBIOS Session Service |
| `445/tcp` | Microsoft-DS  | SMB / Microsoft-DS      |

---

### C. Specific Port Inspection

The scan was restricted to the previously identified administrative and file-sharing ports for focused enumeration.

```bash
nmap -p 135,139,445 192.168.143.129
```

**Purpose:**
Reduces unnecessary scanning by focusing specifically on ports relevant to RPC, NetBIOS, and SMB services.

---

### D. Service Version Detection

Nmap service detection was used to identify the services and obtain available version/banner information.

```bash
nmap -sV 192.168.143.129
```

**Discovered Services:**

* **135/tcp:** Microsoft Windows RPC
* **139/tcp:** Microsoft Windows `netbios-ssn`
* **445/tcp:** Microsoft Windows 7–10 `microsoft-ds` (Workgroup)

---

### E. Operating System Fingerprinting

Nmap OS detection was performed to identify the operating system based on TCP/IP stack characteristics and network responses.

```bash
nmap -O 192.168.143.129
```

**Fingerprint:**
Windows 7 / Windows Server 2008 R2 — Kernel Version `6.1`

---

### F. UDP Port Auditing

UDP scanning was performed against NetBIOS-related ports to identify connectionless services that may not appear during TCP scanning.

```bash
nmap -sU -p 137,138 192.168.143.129
```

**Purpose:**
Helps identify additional UDP-based network services and potential exposure that could be missed by TCP-only reconnaissance.

---

### G. Reverse DNS Resolution Suppression

Reverse DNS resolution was disabled to reduce unnecessary DNS queries and improve scan speed.

```bash
nmap -sS -n 192.168.143.129
```

**Purpose:**
The `-n` option prevents Nmap from performing DNS resolution for discovered hosts.

---

### H. Timing Template & Scan Optimization

The `-T4` timing template was used to accelerate scanning within the stable, low-latency virtual network.

```bash
nmap -T4 -F 192.168.143.129
```

**Performance Note:**
`-T4` increases scan speed and is generally suitable for reliable internal networks and controlled lab environments.

---

### I. Comprehensive Aggressive Scan

A comprehensive scan was performed using Nmap's aggressive detection options.

```bash
nmap -A -T4 192.168.143.129
```

The `-A` option enables multiple detection capabilities, including:

* OS detection
* Service/version detection
* Default NSE scripts
* Traceroute

**Host Identifier:**

```text
WIN-0KTJ97389VK
```

**Network Distance:**
`1 hop` — Direct virtual network connection.

---

### J. SMB Security Configuration Audit

Nmap NSE scripts were used to examine SMB security configuration on port 445.

```bash
nmap --script smb-security-mode,smb2-security-mode -p 445 192.168.143.129
```

**Finding:**
SMB message signing was reported as disabled.

**Security Impact:**
When SMB signing is not enforced, certain network attack scenarios, including NTLM relay and man-in-the-middle attacks, may become possible depending on the surrounding network configuration and authentication setup.

**Defensive Recommendation:**

* Enable SMB signing where appropriate.
* Prefer enforcing SMB signing on critical systems.
* Disable legacy SMB protocols where they are no longer required.
* Restrict SMB access to trusted network segments.
* Apply current security updates and hardening policies.

---

### K. Exporting Scan Results

The service/version scan output was saved to a text file for documentation and later analysis.

```bash
nmap -sV 192.168.143.129 -oN nmap_scan_results.txt
```

**Output File:**

```text
nmap_scan_results.txt
```

This provides a persistent record of the reconnaissance results for project documentation and auditing.

---

## 3. Summary of Findings

| Area             | Result                              |
| ---------------- | ----------------------------------- |
| Host Discovery   | Target host online                  |
| TCP 135          | Open — Microsoft RPC                |
| TCP 139          | Open — NetBIOS Session Service      |
| TCP 445          | Open — SMB / Microsoft-DS           |
| OS Detection     | Windows 7 / Windows Server 2008 R2  |
| UDP 137/138      | Audited for NetBIOS exposure        |
| Hostname         | `WIN-0KTJ97389VK`                   |
| Network Distance | 1 hop                               |
| SMB Signing      | Reported as disabled                |
| Scan Report      | Exported to `nmap_scan_results.txt` |

---

## 4. Security Recommendations

Based on the reconnaissance results, the following defensive measures are recommended for the Windows target:

1. **Enable and enforce SMB message signing** where required.
2. **Restrict SMB ports (139/445)** to trusted hosts and network segments.
3. **Disable unnecessary legacy protocols and services.**
4. **Keep the operating system and network services patched.**
5. **Use host-based and network firewalls** to limit unnecessary inbound connections.
6. **Monitor RPC, NetBIOS, and SMB traffic** for unusual activity.
7. **Segment sensitive systems** from untrusted networks.
8. **Repeat vulnerability assessments periodically** after security configuration changes.

---

## 5. Conclusion

This lab demonstrated a structured Nmap-based network reconnaissance workflow against an isolated Windows virtual machine. The assessment progressed from host discovery to TCP/UDP port scanning, service enumeration, OS fingerprinting, aggressive detection, SMB security auditing, and report generation.

The exercise demonstrated how network enumeration can identify exposed services and security configuration issues in a controlled environment. The results can be used to apply appropriate hardening measures and improve the security posture of the Windows host.

All scanning and security testing described in this project was performed against an intentionally isolated Windows virtual machine within a controlled VMware laboratory environment. No unauthorized external 


<img src="./screenshots/nmap.org.jpg" width="700">
