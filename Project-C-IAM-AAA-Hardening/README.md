# Project C: Enterprise Identity & Access Management (IAM) and Host Access Hardening

## 📜 Executive Summary
In this project, I acted as an Identity and Access Management (IAM) Engineer to overhaul an insecure, flat user permission structure on an enterprise host endpoint. Following the Principle of Least Privilege (PoLP) and Zero Trust guidelines, I engineered structured groups, mapped administrative operations to dedicated service profiles, audit-logged credential modifications, and configured host access controls to eliminate shared administrative privileges.

## 🛠️ Tools & Environments Used
* **Operating System:** Linux (Kali VM Environment)
* **IAM Utilities:** `useradd`, `groupadd`, `chmod`, `chown`, Linux Access Control Lists (ACLs)
* **Security Frameworks:** Role-Based Access Control (RBAC), Privilege Escalation Defenses

---

## 🔬 Step-by-Step Walkthrough

### Phase 1: Structuring Role-Based Access Control (RBAC)
To prevent lateral movement and unauthorized system access, I separated users into distinct security domains based on their business functions rather than granting blanket administrative rights.

1. Constructed isolated organizational groups (e.g., SecOps vs. Standard Users):
   ```bash
   sudo groupadd secops_team
   sudo groupadd standard_staff
   ```
   (Screenshot here)

2. Provisioned mock personnel accounts and explicitly mapped them into their restrictive boundaries:
3. ```bash
   sudo useradd -m -g standard_staff analyst_user
   ```
   (screenshot here)

### Phase 2: Restructuring Volatile File Permissions & Directory Ownership
I audited the file system to locate files with global read/write/execute parameters (chmod 777)
 and tightened the architecture so that only data owners and their specific active teams could process the binaries.
 1. Discovered and isolated highly exposed directories, modifying their ownership structures:
  ```bash
sudo chown -R root:secops_team /opt/secure_vault/
```
2. Implemented standard Unix permission boundaries to eliminate public read access:
```bash
sudo chmod 750 /opt/secure_vault/
```
(Screenshot here)

### Phase 3: Centralized Endpoint Authentication Auditing
To fulfill regulatory compliance and capture active auditing tracks, I examined local password aging parameters and systemic event logs to ensure account adjustments are indexed securely
```bash
# Audit password configuration baselines
sudo chage -l analyst_user
```
(Screenshot here)

## 📊 Defensive Takeaways
* **Privilege Containment:** Restricting local write/execute boundaries dramatically prevents successful post-exploitation maneuvers. 
  If a low-level service account gets compromised, tight local permissions stall lateral escalation attempts.

* **Identity Governance:** Engineering clear RBAC definitions mirrors cloud-native policies (like AWS IAM policies or Azure Entra ID permissions), 
  proving competence in systemic access lifecycle control.
