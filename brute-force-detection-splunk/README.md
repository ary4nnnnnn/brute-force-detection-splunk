# Incident Report: Brute Force Attack Detection

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