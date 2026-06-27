# Project A: Network Penetration Auditing & Surface Hardening

## 📜 Executive Summary
This project involved a comprehensive audit of a virtualized network segment to identify active hosts and evaluate the attack surface. Following the audit, 
I implemented security hardening controls on the SSH service to mitigate unauthorized access

## 🛠️ Tools & Environments Used
* **Operating System:** Linux (Kali Environment)
* **Network Scanners:** Network Mapper (Nmap), Port Scanners
* **Protocols Hardened:** SSH (Port 22), TCP/UDP Transport Sockets
* **Core Concepts:** Network Enumeration, Port States, Cryptographic Remote Access

## Phase 1: Host Discovery
- **Objective:** Identify active assets within the 192.168.XX.X/24 subnet.
- **Tooling:** Ping sweep.
- **Result:** Identified 2 active hosts. Evidence attached in `/Evidence` folder.

## Phase 2: Vulnerability Analysis
- **Objective:** Evaluate the administrative attack surface.
- **Tooling:** Nmap.
- **Result:** Confirmed restrictive host-based firewall policy; host is effectively stealth.

## Phase 3: System Hardening
- **Objective:** Mitigate brute-force and credential-stuffing vectors.
- **Actions:** - Transitioned to Ed25519 cryptographic key-based authentication.
    - Disabled legacy password-based login.
    - Restricted root access.
- **Outcome:** Verified security posture via loopback authentication test.
---

## 🔬 Step-by-Step Walkthrough

### Phase 1: Network Discovery & Target Enumeration
To understand the baseline posture of the network, I conducted active host discovery to sweep the target subnet range, 
  identifying alive infrastructure assets without alerting host intrusion systems.

1. Executed a live host ping sweep across the target domain space:
   ```bash
   # to identify what IP addresses are actively online within the subnet
   nmap -sn 192.168.XX.X
   ```
  <img width="496" height="140" alt="image" src="https://github.com/user-attachments/assets/10bd64b0-8b08-425d-8120-c8d91af1b92d" />
  
### Phase 2: Port Scanning & Service Auditing
Once the active IP addresses were isolated, I performed a deep transport-layer scan to audit open TCP/UDP sockets, banner-grabbing the application versions to cross-reference them against known     public vulnerability lists.

* Audited target transport interfaces to evaluate running service daemons:
   ```bash
   # Initiates a deep transport layer scan
   nmap -sV -p- 192.168.XX.X
   ```
 *  To preserve time, I conducted a scan on the high-risk ports
   ```bash
   # focuses on the high risk ports
   nmap -sV --top-ports 2000 192.168.XX.X
   ```
<img width="478" height="346" alt="image" src="https://github.com/user-attachments/assets/15bbc72c-bf96-428b-b9f6-b93b87651704" />

  * I conducted a "SYN Stealth" Scan to test the firewall rules further
    ```bash
    # conducts a SYN Stealth scan on ports 22, 80, 443
    sudo nmap -sS -p 22,80,443 192.168.XX.X
    ```
    <img width="648" height="257" alt="image" src="https://github.com/user-attachments/assets/f47252cb-79a7-4438-b431-0b9f4103d8db" />

3. Analysis of Findings: Nmap scan results indicate that the target host is configured with a restrictive firewall policy (Host-based Filtering), as all targeted TCP ports returned 'filtered' status.
  This indicates a high security posture where the host does not respond to unsolicited packets.

### Phase 3: SSH Configuration Hardening
I am shifting the authentication from password to Asymmetric Cryptographic Authentication. This means that the system will no longer ask for a password; it will only grant access if 
it can verify the unique mathemtical key by the terminal.

1. Generated highly secure asymmetric key pairs to shift authentication mechanisms away from static passwords:
```bash
ssh-keygen -t ed25519
```
Note on Key Lifecycle Management: Rather than generating redundant key pairs, I leveraged an existing Ed25519 cryptographic identity. This aligns with industry best practices for 
Identity and Access Management (IAM), which dictates the minimization of orphaned credentials and the reuse of hardened, authorized identities across trusted environments.
```bash
# used to get the public key if you already have one generated
cat ~/.ssh/id_ed25519.pub
```
2. Hardened the server daemon's configuration file (/etc/ssh/sshd_config) to disable legacy password access and force cryptographic key authorization.
<img width="510" height="122" alt="image" src="https://github.com/user-attachments/assets/feddbcdf-d7d7-4bb0-a972-0a27bac5e4ef" />

Once I completed this step, I restarted the service and tested the new parameters to ensure that the PasswordAuthentication is disabled
<img width="513" height="104" alt="image" src="https://github.com/user-attachments/assets/4b12394e-72d8-4a18-97c7-5b27e138cae0" />
This shows that the PasswordAuthentication set to no longer working, I need to add my public key to authorized key list
```bash
# create a file to store the authorized key
nano ~/.ssh/authorized_keys
```
3. I set critical permissions
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
<img width="950" height="222" alt="image" src="https://github.com/user-attachments/assets/c3d900ba-2da5-49c3-81e6-4ad9c2ad7000" />
confirmation that the SSH is working properly

## Defensive Takeaways & Security Auditing Controls
Attack Surface Minimization: Continuous network enumeration ensures that unauthorized shadow IT or unpatched legacy services are discovered before threat actors locate them.

Administrative Isolation: Shifting from password authentication to key-based SSH controls entirely eliminates the threat vector of automated network brute-force tools, 
forcing zero-trust verification at the endpoint layer.


## Auditing Philosophy
