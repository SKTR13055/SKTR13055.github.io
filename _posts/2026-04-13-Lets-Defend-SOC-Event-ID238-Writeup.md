---
title: "Lets Defend - SOC Event ID 238"
date: 2026-04-13 13:20:00 +0530
categories: [LetsDefend,SOC]
tags: [LetsDefend,SOC,Security_Analyst,Triage,EventID_238,PowerShell_Script,MITRE]
image:
  path: /assets/img/headers/Lets_defend_Banner.avif
  alt: Lets-Defend-Banner
---

# SOC153 -Suspicious Powershell Script Executed -Malware

---

# **Security Incident Report – Suspicious PowerShell Execution (SOC153)**

![Screenshot 2026-04-13 at 2.09.05 PM.png](/assets/img/Lets-Defend-SOC153-Photos/Screenshot_2026-04-13_at_2.09.05_PM.png)

## **1. Executive Summary**

On **March 14, 2024, at 05:23 PM**, a security alert (**SOC153 – Suspicious PowerShell Script Executed**) was triggered on host **Tony**. The alert indicates execution of a potentially malicious PowerShell script (`payload_1.ps1`) from a user download directory.

Analysis confirms that the script is **malicious**, associated with external command-and-control (C2) communication, and was **not quarantined** by endpoint protection. Immediate remediation actions are required to prevent further compromise.

---

## **2. Incident Details**

| Field | Value |
| --- | --- |
| **Event ID** | 238 |
| **Alert Rule** | SOC153 – Suspicious PowerShell Script Executed |
| **Severity Level** | Medium (Security Analyst Review Required) |
| **Hostname** | Tony |
| **IP Address** | 172.16.17.206 |
| **Event Time** | Mar 14, 2024 – 05:23 PM |
| **File Name** | payload_1.ps1 |
| **File Path** | C:\Users\LetsDefend\Downloads\payload_1.ps1 |
| **File Hash (SHA-256)** | db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0 |
| **EDR Status** | Detected (Not Quarantined) |

---

## **3. Threat Indicators (IOCs)**

### **File Indicators**

![Screenshot 2026-04-13 at 2.38.36 PM.png](/assets/img/Lets-Defend-SOC153-Photos/Screenshot_2026-04-13_at_2.38.36_PM.png)

- **File Name:** payload_1.ps1
- **SHA-256 Hash:** db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0

### **Network Indicators**

- **Malicious URL:**
    
    ```
    http://91.236.116.163/INDEX.PHP?ID=90059C37-1320-41A4-B58D-2B75A9850D2F&SUBID=9G6CLLE6
    ```
    
- **Command & Control (C2) IPs:**
    - 3.5.130.147
    - 161.22.46.148:443 (kionagranada.com)

---

## **4. Analysis Summary**

### **4.1 Behavioral Analysis**

- The PowerShell script was executed from a **user download directory**, a common attacker technique.
- PowerShell is frequently abused for:
    - Downloading secondary payloads
    - Establishing persistence
    - Communicating with remote C2 servers

### **4.2 Malware Analysis Findings**

Using third-party tools such as:

- VirusTotal
    
    ![Screenshot 2026-04-13 at 2.35.34 PM.png](/assets/img/Lets-Defend-SOC153-Photos/Screenshot_2026-04-13_at_2.35.34_PM.png)
    
- Hybrid Analysis
- Any.Run

The file was flagged as **malicious/suspicious**, confirming:

- External communication attempts
- Indicators of downloader/backdoor behavior
- Possible data exfiltration or remote command execution capability
    
    ![Screenshot 2026-04-13 at 3.07.18 PM.png](/assets/img/Lets-Defend-SOC153-Photos/Screenshot_2026-04-13_at_3.07.18_PM.png)
    

### **4.3 Network Activity**

- The script attempted communication with known malicious infrastructure.
- HTTPS traffic over port 443 suggests **encrypted C2 communication**, making detection harder.

---

## **5. Impact Assessment**

| Category | Assessment |
| --- | --- |
| **System Compromise** | Likely |
| **Data Exposure** | Possible |
| **Persistence Mechanism** | Unknown (needs further investigation) |
| **Lateral Movement Risk** | Medium |
| **Detection Evasion** | Possible (PowerShell abuse + encryption) |

---

## **6. MITRE ATT&CK Mapping**

![Screenshot 2026-04-13 at 3.07.36 PM.png](/assets/img/Lets-Defend-SOC153-Photos/Screenshot_2026-04-13_at_3.07.36_PM.png)

| Technique | Description |
| --- | --- |
| **T1059.001** | PowerShell Execution |
| **T1105** | Ingress Tool Transfer |
| **T1071.001** | Web Protocol (HTTP/HTTPS C2 Communication) |
| **T1027** | Obfuscated/Encrypted Payloads |

---

## **7. Root Cause Analysis**

The infection likely originated from:

- User downloading a malicious script from an untrusted source
- Lack of PowerShell execution restrictions
- Endpoint protection detecting but not blocking the threat

---

## **8. Response Actions Taken**

- Alert reviewed and validated as **true positive**
- Malware identified via hash and behavioral analysis
- Indicators extracted for further containment

---

## **9. Recommended Actions**

### **Immediate Actions**

- Isolate the affected host (**Tony**) from the network
- Manually remove the malicious file:
    
    ```
    C:\Users\LetsDefend\Downloads\payload_1.ps1
    ```
    
- Block all identified IOCs:
    - IP addresses
    - Domain (kionagranada.com)
    - Malicious URL

### **Short-Term Actions**

- Perform full antivirus/EDR scan
- Check for persistence mechanisms:
    - Scheduled tasks
    - Registry run keys
- Review PowerShell logs (Event ID 4104)

### **Long-Term Actions**

- Enforce PowerShell execution policies (e.g., Restricted/AllSigned)
- Enable advanced logging:
    - Script Block Logging
    - Module Logging
- Conduct user awareness training
- Deploy network-level threat blocking (IDS/IPS)

---

## **10. Conclusion**

The alert represents a **confirmed malicious PowerShell execution attempt** involving external command-and-control communication. The lack of automatic quarantine increases the risk of compromise.

Prompt containment and remediation are critical to prevent:

- Further system compromise
- Potential lateral movement
- Data exfiltration
