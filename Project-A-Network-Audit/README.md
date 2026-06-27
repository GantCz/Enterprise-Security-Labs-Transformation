# Project A: Network Penetration Auditing & Surface Hardening

## 📜 Executive Summary
In this project, I stepped into the role of an Internal Security Auditor to assess the external attack surface of a simulated corporate network zone.
I utilized advanced network enumeration techniques to map active assets, identified exposed application daemons, evaluated open network sockets, 
and implemented host-level configurations to secure administrative Remote Code Execution (RCE) vectors via Secure Shell (SSH).

## 🛠️ Tools & Environments Used
* **Operating System:** Linux (Kali Environment)
* **Network Scanners:** Network Mapper (Nmap), Port Scanners
* **Protocols Hardened:** SSH (Port 22), TCP/UDP Transport Sockets
* **Core Concepts:** Network Enumeration, Port States, Cryptographic Remote Access

---

## 🔬 Step-by-Step Walkthrough

### Phase 1: Network Discovery & Target Enumeration
To understand the baseline posture of the network, I conducted active host discovery to sweep the target subnet range, 
  identifying alive infrastructure assets without alerting host intrusion systems.

1. Executed a live host ping sweep across the target domain space:
   ```bash
   # Emulated command layout
   nmap -sn [Target_Subnet_Range]
   ```
   <img width="662" height="189" alt="image" src="https://github.com/user-attachments/assets/d08812c0-dde0-41a6-a436-b8628d5fb1ec" />

   ### Phase 2: Port Scanning & Service Auditing
   Once the active IP addresses were isolated, I performed a deep transport-layer scan to audit open TCP/UDP sockets, banner-grabbing the application versions to cross-reference them against known public vulnerability lists.

1. Audited target transport interfaces to evaluate running service daemons:
   ```bash
   # Emulated command layout
   nmap -sV -p- [Target_IP]
   ```
2. Analysis of Findings: The infrastructure scan revealed a critical administrative remote access port (Port 22 - SSH) exposed to the network segment,
  requiring immediate access control configurations.

### Phase 3: Administrative Access Hardening (SSH Protocol)
To mitigate the risk of password interception, credential stuffing, or brute-force compromise against the exposed remote administration channel, 
  I audited and reconfigured the server's cryptographic access conditions.

1. Generated highly secure asymmetric key pairs to shift authentication mechanisms away from static passwords:
```bash
ssh-keygen -t ed25519
```
2. Hardened the server daemon's configuration file (/etc/ssh/sshd_config) to disable legacy password access and force cryptographic key authorization.
(Screenshot here)

## Defensive Takeaways & Security Auditing Controls
Attack Surface Minimization: Continuous network enumeration ensures that unauthorized shadow IT or unpatched legacy services are discovered before threat actors locate them.

Administrative Isolation: Shifting from password authentication to key-based SSH controls entirely eliminates the threat vector of automated network brute-force tools, 
forcing zero-trust verification at the endpoint layer.
