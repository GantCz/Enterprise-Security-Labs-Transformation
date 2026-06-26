# Project B: Cryptography & Secure Transport Layer Engineering

## 📜 Executive Summary
In this project, I acted as a Security Infrastructure Engineer to design and deploy data-in-transit protections across a simulated enterprise network zone. Utilizing OpenSSL on a Linux terminal, I established a localized Certificate Authority (CA) root of trust, generated cryptographically secure asymmetric key pairs, issued signed X.509 digital certificates to protect web servers via HTTPS, and configured secure parameters for encrypted corporate email infrastructure.

## 🛠️ Tools & Environments Used
* **Operating System:** Linux (Kali VM Environment)
* **Cryptographic Toolkit:** OpenSSL Engine
* **Standards & Protocols:** X.509 Certificates, Transport Layer Security (TLS/HTTPS), RSA/Symmetric Ciphers
* **Core Concepts:** Public Key Infrastructure (PKI), Asymmetric vs. Symmetric Encryption, Digital Signatures

---

## 🔬 Step-by-Step Walkthrough

### Phase 1: Establishing the Root Certificate Authority (CA)
To create a trusted ecosystem without relying on commercial third-party authorities, I configured a local Private Certificate Authority to sign internal network assets.

1. Generated a cryptographically strong private key for the Root CA:
   ```bash
   openssl genrsa -des3 -out rootCA.key 4096
   ```
   (Screenshot here)
   
3. Self-signed the root certificate to initialize the trust anchor:
 ```bash
   openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
   ```
   (screenshot here)


### Phase 2: Generating Key Pairs & Certificate Signing Requests (CSR)
Operating on behalf of a simulated corporate web services application, I generated an asymmetric key pair and drafted a request to be validated by the newly established Root CA.

1. Formulated the application server's private key:
```bash
openssl genrsa -out server.key 2048
```
(screenshot here)

2. Generated the Certificate Signing Request (CSR) defining the corporate domain attributes:
```bash
openssl req -new -key server.key -out server.csr
```
(screenshot here)

### Phase 3: Issuing X.509 Certificates & Implementing HTTPS/Secure Email
I finalized the PKI lifecycle by processing the request using the CA parameters, generating a signed production digital certificate capable of establishing secure transport tunnels.

1. Executed the final certificate signing operation:
```bash
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 500 -sha256
```
(Screenshot here)

2. Implementation Review: The resulting .crt and .key artifacts were verified as structurally ready to be bound to web daemons (Apache/Nginx) for strict HTTPS configurations or integrated into corporate mail loops to sign and encrypt communications


## 📊 Defensive Takeaways & PKI Governance
* **Trust Boundary Lifecycle:** Managing an internal CA highlights the critical importance of private key protection. If a threat actor gains unauthorized access to a root key, they can forge certificates to execute transparent Man-in-the-Middle (MitM) or credential interception attacks.

* **Encryption Enforcement:** Transitioning systems from unencrypted HTTP to authenticated TLS channels prevents data leakage over public networks, mitigating the impact of sniffers and protocol harvesters.
