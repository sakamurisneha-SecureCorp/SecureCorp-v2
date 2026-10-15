# SecureCorp-v2 - B2B SaaS Security Assessment

## Overview
Security assessment of SecureCorp's Ubuntu web server (10.10.10.10) hosting DVWA on Metasploitable2 in an isolated lab environment. This project was completed as part of B.Tech Cyber Security curriculum at SV College of Engineering, Tirupati.

## Lab Architecture (Isolated VirtualBox - No Internet)
- **10.10.10.10** - Ubuntu Web Server (Target) - DVWA + weak credentials, Apache2
- **10.10.10.5** - Kali Linux (Attacker) - Nmap, Hydra, Metasploit, Burp
- **10.10.10.20** - Ubuntu Log Server - rsyslog central logging, log analysis
- Network: Host-only adapter, isolated, no internet access

## Methodology & Tools
1. **Reconnaissance:** `nmap -sV -sC 10.10.10.10` - Found ports 22(SSH), 21(FTP), 80(HTTP)
2. **Brute-force:** `hydra -l admin -P rockyou.txt ssh://10.10.10.10` & FTP brute-force
3. **Exploitation:** Metasploit, DVWA low security level, achieved reverse shell
4. **Logging:** Configured rsyslog to forward all logs to 10.10.10.20
5. **Incident Detection:** Analyzed central logs for brute-force patterns

## Seeded Incident Detected
- Cron persistence job added by attacker at /etc/crontab (every 5 mins)
- Outbound connection to suspicious IP (C2 simulation) - 198.51.100.10:4444
- Multiple brute-force attempts from 10.10.10.5 visible in auth.log
- Evidence collected in /var/log/auth.log and central log server

## Mitigations Proposed
- Enforce strong passwords + MFA for all users
- Key-based SSH, disable password auth
- Install Fail2Ban to block brute-force, Wazuh SIEM for alerting
- Update DVWA, patch Metasploitable2, enforce HTTPS
- Regular log monitoring on central server

## Student Info
**Sakamuri Sneha** - B.Tech Cyber Security (2024-2028) - 9.14 GPA
SV College of Engineering, Tirupati (JNTU-A)
GitHub: https://github.com/sakamurisneha-SecureCorp/SecureCorp-v2
