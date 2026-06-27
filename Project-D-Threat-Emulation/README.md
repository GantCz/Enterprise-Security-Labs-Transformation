# Project D: Threat Emulation & Application Vulnerability Analysis

## 📜 Executive Summary
In this project, I executed controlled threat emulation workflows to analyze modern web application and network exploit mechanics. 
Acting as an offensive security engineer, I reverse-engineered application-layer vulnerabilities, including Cross-Site Scripting (XSS), 
simulated lateral movement footprints (Pass-the-Hash vectors), and analyzed malicious code signatures. 
The resulting intelligence was utilized to configure proactive web application defenses and host indicators.

## 🛠️ Tools & Environments Used
* **Operating System:** Linux (Kali Environment)
* **Exploit Scenarios:** Cross-Site Scripting (XSS), Cryptographic Hash Exploitation (Pass-the-Hash)
* **Analytical Frameworks:** MITRE ATT&CK Mapping, Volatile Input Sanitization

---

## 🔬 Step-by-Step Walkthrough

### Phase 1: Input Validation Failure & Cross-Site Scripting (XSS) Emulation
I simulated an application compromise by injecting a malicious payload into an unauthenticated web text input field to demonstrate how an attacker 
  can hijack user sessions or execute browser-side code due to missing input sanitization.

1. Emulated payload delivery string via browser parameters:
   ```html
   <script>alert('Indicator of Compromise: XSS Verified');</script>
(screenshot)

### Phase 2: Lateral Movement Analysis (Pass-the-Hash Vectors)
To understand how modern ransomware spreads across directory domains, I simulated a Pass-the-Hash behavior—demonstrating how an adversary extracts a pre-computed 
  cryptographic password hash from memory to authenticate to adjacent assets without ever needing the cleartext password.

1. Examined the flow of a hash relay or offline credential capture mechanism:
```bash
sha256sum /etc/shadow
```
(Screenshot here)

### Phase 3: Constructing Web Application Firewalls (WAF) & Remediations
To defend the application architecture against the emulated attacks, I drafted defensive engineering remediation steps to filter web requests before they parse into memory.

* **Remediation 1 (XSS Defense):** Enforce strict Context-Aware HTML Encoding and implement a rigid Content Security Policy (CSP) header to disable inline scripts entirely.

* **Remediation 2 (Credential Defense):** Transition authentication architectures to modern tokens (OAuth 2.0 / OIDC) and 
  deploy Local Administrator Password Solution (LAPS) parameters to change credentials dynamically.

## 📊 Defensive Takeaways
* **Proactive Signature Mapping:** By understanding the technical execution paths of modern attacks, 
  security engineers can write precise detection signatures (YARA or Sigma rules) to intercept malicious behavior automatically at scale.

* **Secure Coding Integration:**  This project bridges the gap between pure development and cloud-native architecture security, 
  demonstrating how application flaws directly translate into infrastructure compromise.
