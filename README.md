# Blue Team Vs. Red Team  – Windows, Splunk, Kali

![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-mapped-blue) ![Splunk](https://img.shields.io/badge/SIEM-Splunk-orange) ![Windows 10](https://img.shields.io/badge/Endpoint-Windows%2010-blue) ![Kali Linux](https://img.shields.io/badge/Attacker-Kali%20Linux-red)

## Overview
This is one of my favorite projects that I've created. It essentially simulates a mini Security Operations Center (SOC) environment using:
- A **Windows 10** VM with event logging & Sysmon
- A **Kali Linux** machine to simulate attacks
- A **Splunk SIEM** for log ingestion, detection, and analysis

The goal is to detect simulated adversarial behaviors and align findings with the MITRE ATT&CK framework.

## Tools Used
- **Splunk Enterprise** (SIEM)
- **Splunk Universal Forwarder** (log shipper)
- **Windows 10** (victim endpoint)
- **Sysmon** (for enhanced endpoint logging)
- **Kali Linux** (attacker platform)
- **MITRE ATT&CK Framework** (tactic/technique mapping)

## Lab Setup
```plaintext
1. Three VMs: Windows 10, Kali Linux, and Splunk server (Ubuntu)
2. Configured log forwarding from Windows via Splunk UF
3. Installed Sysmon on Windows (SwiftOnSecurity config)
4. Enabled auditing for login events and PowerShell activity
5. Connected all VMs on isolated internal virtual LAN
```

## Simulated Attacks
- **Brute-force attack** on RDP via Hydra
- **Reverse shell execution** using PowerShell payload
- **Port scanning** with Nmap from Kali
- **Malware simulation** using EICAR

## Detection Methods
- Correlated login failures (Event ID 4625) using SPL:
  ```spl
  index=windows EventCode=4625 | stats count by Account_Name, Source_Network_Address
  ```
- Detected reverse shell via:
  ```spl
  index=windows EventCode=3 Image="*powershell.exe" DestinationIp="<Kali IP>"
  ```
- Alerted on high failed login volume (>5/min) with Splunk alert rule

## Findings
- All attacks were successfully detected using Splunk dashboards and queries
- Sysmon logs captured critical behaviors (process creation, network connections)
- Events were mapped to MITRE ATT&CK techniques (e.g., T1110 Brute Force, T1059 Command Execution)

## Takeaways
- Demonstrated hands-on experience with SIEM & log analytics
- Reinforced knowledge of endpoint telemetry and adversary behaviors
- Practiced detection engineering, alert writing, and event triage workflows
- Enhanced documentation, investigation, and communication skills

> 📁 Supporting artifacts: Splunk queries, screenshots, and MITRE mapping table are included in this folder
