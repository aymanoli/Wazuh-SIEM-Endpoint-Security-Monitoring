# Wazuh SIEM Endpoint Security Monitoring

## Overview
This project demonstrates a Wazuh SIEM home lab built for hands-on SOC Analyst training. It showcases endpoint security monitoring, log analysis, File Integrity Monitoring (FIM), vulnerability detection, custom rule creation, and brute-force attack detection using a Windows endpoint and Ubuntu-based Wazuh Manager. The lab simulates real-world security events to practice alert validation, investigation, and threat detection.

## Lab Architecture
| Components  | System  |
|-------------|---------|
| Wazuh Server | Ubuntu Server |
| Wazuh Agent  | Windows Agent |

## Main Project Objectives
* Built and configured a Wazuh SIEM home lab by deploying the Wazuh manager on Ubuntu and Wazuh agent on Windows endpoint for centralized log monitoring and security alert.
* Configured File Integrity Monitoring (FIM), Windows event log collection, vulnerability detection, and MITRE ATT&CK mapped alert visibility within the Wazuh dashboard.
* Simulated security events by performing file creation, modification, and deletion activities on the monitored Windows endpoint to validate alert generation and detection accuracy in Wazuh.
* implemented a custom rule in Wazuh (local_rules.xml) to identify brute-force authentication attempts, validated rule performance through simulated attack scenarios, and analyzed generated alerts

## Implemented Features
* Wazuh server installation and configuration in Ubuntu
* Wazuh agents deployed and connected in Windows
* Create custom rules to identify a brute-force attack
* File Integrity Monitoring (FIM)
* Windows endpoint monitoring
* Security event collection
* Basic alert investigation
* MITRE ATT&CK mapping

## ScreenShots
#### Windows Agent Overview
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-29-53" src="https://github.com/user-attachments/assets/d454a447-65c1-4e54-98c8-13d08f16eb08" />

### Windows event log collection
<img width="1920" height="955" alt="Screenshot From 2026-07-11 10-08-45" src="https://github.com/user-attachments/assets/9afe498a-5b96-4c7b-a65b-e4e622e62201" />

### File Integrity Monitoring
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-33-12" src="https://github.com/user-attachments/assets/2580e7fc-df2f-4bba-aaa1-dce49518a0dd" />

### MITRE ATT&CK mapped
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-32-10" src="https://github.com/user-attachments/assets/4962f535-1157-4bf1-83fe-c90e483c42e4" />

### Vulnerability detection
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-54-13" src="https://github.com/user-attachments/assets/ecab57c7-129c-4855-83b2-a98e8afdea37" />


