---
title: "Sysmon & MITRE ATT&CK - Endpoint Detection Lab"
date: 2025-10-25 18:00:00 +0530
categories: [Documentation, Blue Team]
tags: [sysmon, mitre att&ck, log analysis, windows, endpoint security]
image:
  path: /assets/img/headers/Sysmon.jpg
  alt: Sysmon and MITRE Lab Banner
---
# Experiment 3: Sysmon & MITRE ATT&CK

# Summary

Successful beginner-level endpoint detection lab. Demonstrated installing and using Sysmon (and fallback to native Windows auditing where necessary), generating benign PowerShell activity, locating and extracting telemetry in Event Viewer, mapping the activity to MITRE ATT&CK (T1059.001), and verifying service management. Next steps: add logging ingestion to a SIEM (Kibana/Splunk), craft Sigma rules, and run a small set of additional ATT&CK-mapped experiments

# Aim of the Experiment

---

To map a Cyber attack to the MITRE ATT&CK Matrix.

# Objective

---

**Learning objective**

- To install “Sysmon” tool to detect a cyber attack and then map it to the MITRE ATT&CK Matrix/Framework

# Prerequisites

---

- Windows Operating System (With admin Privileges) ( Windows 10/11)
- Stable Network Internet Connection.

# Description

---

**“Sysmon”** also known as the “SystemMonitor” is a Windows tool from Microsoft SysInternals that records detailed system activity such as process creation, network connections, and file changes. It helps detect and investigate suspicious behavior by providing decent quality logs that can be mapped to MITRE ATT&CK Matrix/Framework,

**MITRE ATT&CK Framework** is a globally recognized knowledge base that describes the tactics, Techniques, and Procedures (TTPs) used by cyber attackers. It helps security teams to understand, detect and defend from cyber against real-world threats by mapping attackers behavior across different stages of an attack.

# Procedure

## Hypothesis 1:-

Installing in the Virtual Machine.

1.Install and configure  Windows 11 from Appstore ( Previous Experiment)

1. Install Sysmon from windows website. ([https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon))
2. Create a temp file and extract the sysmon file in the temp file.
3. Open Windows power shell as Admin and then change the location to the path where sysmon is located.
4. To install the Sysmon type in the following command “.\Sysmon64.exe -i “   (Where  “i” is install with a config file.).
5. In Windows VM virtualization security it disabled hence unable to install “sysmon” (using windows 11 latest version)

---

error: 

Loading configuration file with schema version 4.90
Configuration file validated.
Sysmon64 installed.
SysmonDrv installed.
StartService failed for SysmonDrv:
This driver has been blocked from loading
Failed to start the driver:
This driver has been blocked from loading

Stopping the service failed:
The service has not been started.
SysmonDrv removed.
Stopping the service failed:
The system cannot find the file specified.
DeleteService failed:
Access is denied.

---

**Result: Experiment failed on Virtual Machine. (***Unsuccessful.***)

Sysmon’s driver was blocked due to system protection features, preventing it from starting.)

## Hypotheses 2:

**Installing sysmon in the original windows desktop**

1. Installed the sysmon tool with ease and no problems occurred. 
2. download the sysmon config file from the githhub(Olafhartong) {https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml}
3. Verify if the sysmon service is installed in the regedit. 
4. If not installed, open up the “Powershell” as “admin” and then change your directory to the where is Sysmon file is downloaded.
5. Install the Sysmon using the following command {For 64-bit use this command} ( ./Sysmon64.exe -accepteula  -i sysmonconfig.xml), {For 32-bit use this command} (./Sysmon.exe  -accepteula -i sysmonconfig.xml)
6. The output will display as “Sysmon64 Started” {Or whatever version you have installed}
7. To test it out ,open up the command prompt and type in benign Malicious command.
8. To ensure it has been logged open up the Event-viewer→Applications and Service Logs → Microsoft → Windows → Sysmon.
9. On the Actions side{Which is on the right side} Operational Section click on the “Filter Current Log”, this opens up a dialogue box in the “filter” tab there is a section which says <All Event  IDs> in that you should include “1” where  1 means “Process Create (most useful for validating process creation logging). 

![image.png](/assets/img/sysmon-detection-lab-photos/image.png)

---

Other various Events such as.

- **Event ID 1:** Process creation
- **Event ID 3:** Network connection
- **Event ID 11:** File creation
- **Event ID 6:** Driver loaded

***Note:*** For more Events Ids visit the website: [https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

---

1. Click on the event which contains the “command”, underneath there will be 2 tabs “General” and “Details”.
2. I observed the details which includes “Command line”, Rule name, technique name, User, parent Process Guid, Parent Process ID etc.
3. Opened MITRE ATT&CK website and mapped it to the particular attack and diagnosed it. 
4. Mitigations are also available for the specific attacks.

***Conclusion:  Successfully detected the attacked using the sysmon and then diagnose it using the MITRE ATT&CK website.***

### T1059.003- Windows Command Shell

Technique: **T1059.003 — Windows Command Shell.** In your notes, record: victim ran a Windows Command Line process with suspicious flags (benign in lab), captured in Sysmon Event ID 1, command line visible.

![Windows Command Getting Logged in the Sysmon](/assets/img/sysmon-detection-lab-photos/Capture.png)

Windows Command Getting Logged in the Sysmon

![Mapping the Attack using the MITRE Framework](/assets/img/sysmon-detection-lab-photos/Screenshot_2025-10-26_at_12.53.17_PM.png)

Mapping the Attack using the MITRE Framework

![Mitigations For the Attack](/assets/img/sysmon-detection-lab-photos/Screenshot_2025-10-26_at_1.03.27_PM.png)

Mitigations For the Attack

![Execution Prevention Technique to mitigate the particular attack.](/assets/img/sysmon-detection-lab-photos/Screenshot_2025-10-26_at_1.03.36_PM.png)

Execution Prevention Technique to mitigate the particular attack.

### T1059.001-Power Shell

Technique: **T1059.001- Power Shell. I**n your notes, record: victim ran a PowerShell process with suspicious flags (benign in lab), captured in Sysmon Event ID 1, command line visible.

![Power Shell Command Getting Detected in the Sysmon Tool](/assets/img/sysmon-detection-lab-photos/PowerShell_Experiment.png)

Power Shell Command Getting Detected in the Sysmon Tool

![Screenshot 2025-10-27 at 12.31.56 PM.png](/assets/img/sysmon-detection-lab-photos/Screenshot_2025-10-27_at_12.31.56_PM.png)

![Screenshot 2025-10-27 at 12.32.17 PM.png](/assets/img/sysmon-detection-lab-photos/Screenshot_2025-10-27_at_12.32.17_PM.png)

| Tactic | Technique | ID |
| --- | --- | --- |
| Execution | Windows Command Execution | T1059.003 |
| Execution | Power Shell Command Execution | T1059.001 |

# Results

---

| S.no | Installing Sysmon | Outcome | Observation/Analysis |
| --- | --- | --- | --- |
| 1. | Virtual machine | Failed | - Sysmon failed to install due to virtualization security restrictions  inside the virtual machine. |
| 2. | Physical Desktop | Success | - Sysmon was successfully installed and able to detect and log the attack activity. |

**Additional results**

# Learnings

---

- Learned how to install the System monitor(Sysmon) in a windows machine.( ./Sysmon64.exe -i sysmonconfig.xml)
- Able to detect the malicious command line through “Event Viewer”. (Event ID 1)
- Handled the cyber attack by mapping it to the MITRE ATT&CK Matrix.
- Check which services are running using the “Services” tool.
- Able to remove the “Sysmon” tool using the “regedit”(Registry Editor). (You can remove it using ./Sysmon64.exe -u) asvwell.

# Resources Used

---

1. Sysmon Installation:- [https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) (More information about Event IDs)
2. Sysmon Config file(Olafhartong):-[https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml)
3. MITRE ATT&CK Framework:- [https://attack.mitre.org/](https://attack.mitre.org/)

