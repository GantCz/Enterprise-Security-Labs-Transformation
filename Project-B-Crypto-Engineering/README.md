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
