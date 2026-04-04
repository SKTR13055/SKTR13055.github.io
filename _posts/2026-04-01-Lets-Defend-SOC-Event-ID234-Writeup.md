# 🚨 SOC Incident Report – Brute Force Attack (Event ID 234)

> ⚡ Hands-on SOC investigation involving brute force detection, threat intelligence correlation, and MITRE ATT&CK mapping.

> Note: Some techniques may overlap across tactics depending on attacker intent and environment context.

## 🎯 Why This Project Matters

This project demonstrates how weak credentials and exposed RDP services can lead to full system compromise. It highlights the importance of monitoring authentication logs and implementing strong access controls in enterprise environments.

## 📌 Incident Summary

A brute force attack was detected targeting a Windows host over RDP (port 3389). The attacker attempted multiple failed logins using different usernames and successfully compromised a valid account (`Matthew`). Post-compromise, the attacker executed enumeration commands to assess privileges.

---

## 🕒 Timeline of Events

- **Mar 07, 2024 – 11:44 AM**
    - Multiple failed login attempts detected (**Event ID 4625**)
    - Source IP: `218.92.0.56`
    - Target Host: `Matthew (172.16.17.148)`
    - Protocol: RDP (3389)
- **Brute Force Activity**
    - 30 login attempts observed
    - Usernames attempted:
        - Guest
        - Admin
        - Sysadmin
        - Matthew
- **Successful Compromise**
    - **Event ID 4624** (Successful login)
    - Username: `Matthew`
    - Logon Type: 10 (Remote Interactive – RDP)
- **Mar 07, 2024 – 11:45:18**
    - Attacker accessed command prompt
    - Executed privilege enumeration commands
    
    ![Screenshot 2026-04-04 at 1.41.41 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_1.41.41_PM.png)
    

---

## 🎯 Attack Details

### 🔍 Indicators of Compromise (IOCs)

- **Source IP:** `218.92.0.56`
- **Destination IP:** `172.16.17.148`
- **Protocol:** RDP (3389)
- **Event IDs:**
    - 4625 – Failed logins
    - 4624 – Successful login

---

## 🌐 Threat Intelligence

The attacker IP (`218.92.0.56`) was analyzed using multiple threat intelligence platforms:

### 🔍 Sources Checked

- VirusTotal
    
    ![Screenshot 2026-04-04 at 2.12.50 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_2.12.50_PM.png)
    
- AbuseIPDB
    
    ![Screenshot 2026-04-04 at 4.07.27 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_4.07.27_PM.png)
    
- LetsDefend Threat Intelligence

![Screenshot 2026-04-04 at 4.03.51 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_4.03.51_PM.png)

---

### Results

| Attribute | Value |
| --- | --- |
| IP Address | 218.92.0.56 |
| Country | China 🇨🇳 |
| ISP | CHINANET Jiangsu Province Network |
| ASN | AS4134 |
| Malicious Reports | 453,000+ (AbuseIPDB) |
| Detection Ratio | 10/94 (VirusTotal) |
| Verdict | Malicious |

### Conclusion:

**The attacker IP is highly malicious and associated with repeated malicious activity**

---

## 📊 Traffic Analysis

- Repeated RDP login attempts observed from attacker IP
- No SSH activity detected

![Screenshot 2026-04-04 at 2.23.33 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_2.23.33_PM.png)

- Attack focused solely on RDP service (port 3389)

---

## ⚠️ Attack Outcome

✅ **Brute force attack was successful**

Evidence:

- Multiple failed login attempts followed by a successful login from the same IP
- Immediate post-login command execution

![Screenshot 2026-04-04 at 4.10.41 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_4.10.41_PM.png)

---

## 🖥️ Post-Exploitation Activity

The attacker performed local enumeration:

![Screenshot 2026-04-04 at 1.58.00 PM.png](/assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_1.58.00_PM.png)

- `whoami`
    
    → Displays the currently logged-in user.
    
- `net user letsdefend`
    
    → Displays detailed information about the specified user account (e.g., account status, group membership, password info).
    
- `net localgroup administrators`
    
    → Lists all users and groups with administrative privileges on the system.
    

### Assessment:

The attacker performed **account and privilege enumeration**, likely preparing for privilege escalation or persistence

---

## 🔐 Root Cause

- Weak or easily guessable password for user `Matthew`
- No account lockout policy enforced
- RDP exposed to external network

---

## 🛡️ Response Actions Taken

- ✅ Identified malicious external IP
- ✅ Confirmed successful compromise
- ✅ Analyzed logs (Event IDs 4624 & 4625)
- ✅ Contained the affected endpoint via EDR
- ✅ Investigated attacker activity

---

## 🚧 Containment & Remediation

### Immediate Actions:

- Isolated compromised host
- Disabled/secured affected account
- Blocked attacker IP at firewall

### Recommended Actions:

- Enforce strong password policies
- Implement account lockout mechanism
- Restrict RDP access (VPN / allowlist)
- Enable Multi-Factor Authentication (MFA)
- Monitor login anomalies

---

## 🔁 Lessons Learned

### What Happened?

The attacker gained access via brute force due to weak credentials.

### What Went Well?

- Attack was detected via log monitoring
- Quick containment limited further damage

### What Needs Improvement?

- Password security policies
- Exposure of RDP to internet
- Lack of account lockout controls

---

## 🔎 Future Detection Recommendations

Monitor for:

- Multiple failed login attempts (Event ID 4625)
- Successful login after failures (Event ID 4624)
- Logon Type 10 (RDP access)
- Suspicious command execution (`net localgroup`)
- Unusual external IP connections

---

![Screenshot 2026-04-04 at 2.53.11 PM.png](assets/img/Lets-Defend-SOC-Photos-234/Screenshot_2026-04-04_at_2.53.11_PM.png)

## 📌 Final Verdict

This incident is classified as a **successful brute force attack with post-compromise enumeration activity**. Immediate remediation and stronger access controls are required to prevent recurrence, particularly securing RDP access and enforcing strong authentication policies.

---

| Tactic | Technique ID | Technique Name | Description |
| --- | --- | --- | --- |
| Initial Access | T1110 | Brute Force | Attacker performed multiple login attempts over RDP using different usernames and passwords. |
| Execution | T1059 | Command-Line Interface | Attacker used command prompt to execute system commands after gaining access. |
| Persistence | T1078 | Valid Accounts | Legitimate credentials (`Matthew`) were used to maintain access. |
| Privilege Escalation | T1069.001 | Permission Groups Discovery (Local) | `net localgroup administrators` used to identify admin users. |
| Discovery | T1087 | Account Discovery | Attacker attempted to identify valid accounts through login attempts and commands. |
| Lateral Movement | T1021.001 | Remote Services (RDP) | RDP was used as the primary method of access and potential movement. |

This incident covers multiple stages of the attack lifecycle, including:

- Initial Access
- Execution
- Discovery
- Persistence
- Lateral Movement (Potential)

This demonstrates how a single weak point (RDP exposure + weak password) can lead to full attack chain execution.

---

## 🧩 Skills Demonstrated

- Log Analysis (Windows Event Logs)
- Brute Force Detection
- Threat Intelligence Analysis
- Incident Response & Containment
- MITRE ATT&CK Mapping
- RDP Attack Investigation
