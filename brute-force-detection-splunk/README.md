# Brute Force Attack Detection using Splunk

## 📌 Project Overview
This project simulates and detects a brute force attack against a 
Windows 10 RDP service using Splunk SIEM. It demonstrates the complete 
detection workflow used by SOC L1 analysts — from attack simulation to 
log analysis, alerting, and dashboarding.

## 🏗️ Lab Architecture

- **Host:** Windows 11
- **Attacker:** Kali Linux (192.168.1.15) — Hydra
- **Target:** Windows 10 (192.168.1.16) — RDP enabled
- **SIEM:** Ubuntu Server (192.168.1.14) — Splunk Enterprise + 
  Universal Forwarder on Windows 10

## 🛠️ Tools & Technologies
- Splunk Enterprise & Universal Forwarder
- Kali Linux, Hydra
- Nmap (for service enumeration)
- VMware Workstation

## ⚙️ Steps Performed

1. Verified network connectivity between Kali and Windows 10 (ping test)
2. Scanned target with Nmap to confirm RDP (port 3389) was open
3. Enabled RDP on Windows 10 and confirmed Splunk Universal Forwarder 
   was already forwarding Windows Security logs
4. Launched a brute force attack from Kali using Hydra with the 
   rockyou.txt wordlist, targeting the RDP service
5. Verified failed login events (Event ID 4625) were received in 
   Splunk, correctly showing the attacker's source IP and workstation name
6. Built detection queries (SPL) to identify brute force patterns
7. Created a scheduled alert to trigger on repeated failed login attempts
8. Built a monitoring dashboard with failed login trends and attacker IP breakdown
9. Documented findings in an incident report

## 🔎 Splunk Detection Queries

**Top attacker IPs:**
```spl
index=windows EventCode=4625
| stats count by Source_Network_Address, Account_Name
| sort - count
```

**Brute force threshold detection (10+ failures in 5 min):**
```spl
index=windows EventCode=4625
| bin _time span=5m
| stats count by _time, Source_Network_Address
| where count > 10
```

**Failed vs successful login# Incident Report: Brute Force Attack Detection

## Summary
A brute force attack was simulated against a Windows 10 RDP service 
from an internal Kali Linux attacker machine using Hydra.

## Timeline
- Total failed attempts detected: [13]

## Source
- Attacker IP: [192.168.1.15]
- Target: Windows 10 (RDP), IP: [192.168.1.16]

## Detection
- Detected via Splunk using Event ID 4625 (failed logon)
- Alert triggered after threshold of 10+ failures in 5 minutes

## Impact
- No successful login occurred

## MITRE ATT&CK Mapping
- T1110 - Brute Force

## Recommendations
- Enable account lockout policy after 3-5 failed attempts
- Implement MFA for RDP access
- Restrict RDP access via VPN/firewall rules
- Monitor and alert on repeated failed logons

## Screenshots
See /screenshots folder
