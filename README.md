# STOP/Djvu Ransomware (.wrui) Forensics & Partial Data Recovery

A technical exploration and data carving project demonstrating a vulnerability/flaw in the payload delivery of the **STOP/Djvu (.wrui) ransomware variant (2021)**. This project highlights a method to bypass server-side encryption limits and reconstruct partially destroyed document structures.

## 📌 Project Overview
When dealing with online/server-side cryptographic keys, universal decryption is mathematically impossible without the private key. However, this project focuses on a structural flaw in the malware's efficiency optimization strategy: **Partial File Encryption**. 

To maximize execution speed, the ransomware only encrypts the initial block (typically the first 150KB to 5MB) of larger files, leaving the remaining payload completely raw but structurally orphaned. This repository contains the methodology used to carve, reconstruct, and salvage up to 50% of document data from infected `.wrui` files.

---

## 🛡️ Featured Research: STOP/Djvu (.wrui) Forensics & Data Carving

**Repository:** [shubhamd211/ransomware-incident-response-wrui](https://github.com/shubhamd211/ransomware-incident-response-wrui)  
**Domain:** Digital Forensics, Malware Behavioral Analysis, Binary Data Carving

* **The Problem:** The STOP/Djvu ransomware family leverages server-side cryptographic keys, making mathematical decryption impossible once deployed.
* **The Vulnerability:** To accelerate execution, the payload employs partial file encryption, scrambling only the initial block (150 KB to 5 MB) while leaving subsequent document byte streams untouched.
* **The Solution:** Engineered a raw binary extraction pipeline using:
  * **Entropy Mapping:** Pinpointing exact transition points between high-entropy (encrypted) headers and low-entropy (raw) payloads.
  * **PDF Stream Reconstruction:** Bypassing stripped XREF tables and forcing tolerant parsers to render surviving object streams.
  * **OpenXML Schema Carving:** Extracting surviving compressed XML packets directly from corrupted ZIP container envelopes.
* **Result:** Salvaged up to 50% of compromised enterprise documents without ransom interaction.

---

## 🔬 Technical Insight & Flaw Exploitation

The recovery methodology relies on analyzing how the malware interfaces with different document structures:

### 1. File-Specific Encryption Profiles
* **High-Volume / Large Documents (.pdf):** The ransomware scrambles the file header and cross-reference tables (XREF) at the beginning of the file. However, the body text streams, fonts, and object compressions remain unencrypted in the latter half of the byte stream.
* **OpenXML Archives (.docx, .xlsx, .pptx):** Modern Microsoft Office assets are zipped XML infrastructures. While the main ZIP header is corrupted, individual internal XML structural fragments (containing the actual raw text strings) survive the attack if they reside past the encryption boundary.

### 2. The Recovery Pipeline
The extraction process bypasses the operating system's default file parsers (which throw corruption errors due to missing headers) by treating the data as raw binary streams:
1. **Entropy Analysis:** Mapping the file to identify where the high-entropy (encrypted) block ends and low-entropy (unencrypted raw data) begins.
2. **Binary Carving:** Stripping the corrupted encryption block from the file.
3. **Container Rebuilding:** 
   * For **PDFs**: Re-indexing surviving object streams and forcefully rendering pages via tolerant layout engines.
   * For **Office Docs**: Re-assembling fragmented XML data blocks into clean, readable text schemas.

---

## 🛠️ Skills Demonstrated
* **Digital Forensics & Incident Response (DFIR)**
* **Data Carving & Binary Stream Manipulation**
* **Malware Behavior Analysis** (STOP/Djvu family profiles)
* **File System Structural Reconstructions** (Hex editing, OpenXML schema manipulation, PDF XREF structural repair)

---

## 📈 Future Scope / Contributions
* Automation scripts to automatically detect the encryption boundary based on byte entropy.
* Automated extraction pipelines for batch-repairing salvaged `.wrui` documentation.

*Disclaimer: This repository is intended strictly for educational, data forensics, and security research purposes.*
