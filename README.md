# 🔎 Nmap Network & Service Enumeration — Metasploitable 2

## Executive Summary

This project documents a controlled network reconnaissance and service enumeration assessment performed against a deliberately vulnerable **Metasploitable 2** virtual machine in an isolated cybersecurity laboratory environment.

The objective of the assessment was to identify the target host, enumerate exposed TCP services, determine service versions, and evaluate the potential security exposure created by unnecessary or outdated network services.

The assessment was conducted from a **Parrot Security OS** virtual machine using **Nmap**.

> **Target:** 192.168.41.130  
> **Target Platform:** Metasploitable 2  
> **Assessment Type:** Network Reconnaissance & Service Enumeration  
> **Tool:** Nmap  
> **Environment:** VMware Virtual Lab  
> **Purpose:** Authorized cybersecurity training and defensive analysis

---

## 🎯 Assessment Objectives

The primary objectives were to:

- Identify active hosts within the laboratory network.
- Determine whether the target host was reachable.
- Enumerate exposed TCP ports.
- Identify running network services.
- Perform service/version detection.
- Identify potentially outdated or insecure services.
- Document the attack surface for further vulnerability assessment.
- Develop remediation recommendations based on the discovered services.

---

## 🧪 Laboratory Environment

| Component | Configuration |
|---|---|
| Attacker/Assessment Machine | Parrot Security OS |
| Target Machine | Metasploitable 2 |
| Virtualization | VMware |
| Target IP | `192.168.41.130` |
| Network | `192.168.41.0/24` |
| Primary Tool | Nmap |
| Assessment Type | Authorized Lab Testing |

The assessment was performed against an intentionally vulnerable machine within a controlled laboratory environment.

---

# 🔍 Methodology

The assessment followed a basic reconnaissance workflow:

```text
Host Discovery
      ↓
Target Identification
      ↓
Port Scanning
      ↓
Service Enumeration
      ↓
Version Detection
      ↓
Attack Surface Analysis
      ↓
Risk Identification
      ↓
Remediation Recommendations

1. Host Discovery
The local network was examined to identify active systems.
Example command:
sudo nmap -sn 192.168.41.0/24
The discovery process identified multiple active VMware hosts, including the Metasploitable system.
The target used for the detailed assessment was:
192.168.41.130
2. Port and Service Enumeration
A service/version detection scan was performed against the target.
sudo nmap -sV 192.168.41.130
The scan successfully identified the target as an active host and returned a significant number of exposed services.
The screenshots document the results of the enumeration process.
📊 Key Findings
The Nmap scan identified numerous open TCP ports.
Port
Service
Version/Information
21/tcp
FTP
vsftpd 2.3.4
22/tcp
SSH
OpenSSH 4.7p1 Debian
23/tcp
Telnet
Linux Telnet
25/tcp
SMTP
Postfix
53/tcp
DNS
ISC BIND 9.4.2
80/tcp
HTTP
Apache httpd 2.2.8
111/tcp
RPC
RPCbind
139/tcp
NetBIOS-SSN
Samba
445/tcp
SMB
Samba
512/tcp
exec
Remote execution service
513/tcp
login
Remote login service
514/tcp
shell
Remote shell service
1099/tcp
Java RMI
GNU Classpath registry
1524/tcp
Shell
Metasploitable root shell
2049/tcp
NFS
Network File System
2121/tcp
FTP
ProFTPD
3306/tcp
MySQL
MySQL database
5432/tcp
PostgreSQL
PostgreSQL database
5900/tcp
VNC
VNC remote desktop
6000/tcp
X11
X Window System
6667/tcp
IRC
UnrealIRCd
8009/tcp
AJP13
Apache JServ Protocol
8180/tcp
HTTP
Apache Tomcat
Note: Service/version identification is based on the Nmap results captured during this laboratory assessment.
🚨 Security Observations
The enumeration results demonstrate a very large attack surface.
Several services expose significant security concerns when they are unnecessarily accessible or running outdated software.
1. FTP — Port 21
21/tcp open ftp vsftpd 2.3.4
FTP transmits authentication information without modern encryption and should generally be replaced with secure alternatives such as SFTP or FTPS.
The identified version is also associated with an intentionally vulnerable configuration in the Metasploitable 2 environment.
2. Telnet — Port 23
23/tcp open telnet
Telnet provides remote terminal access without adequate encryption.
Recommendation:
Disable Telnet and use SSH instead.
3. HTTP — Port 80
80/tcp open http Apache httpd 2.2.8
The web server is running an outdated Apache version.
Further assessment should include:
Web directory enumeration
Application fingerprinting
HTTP security header analysis
Web vulnerability scanning
Manual application testing
4. SMB / Samba — Ports 139 and 445
139/tcp open netbios-ssn Samba
445/tcp open microsoft-ds Samba
SMB exposure can provide attackers with information about:
Network shares
Users
Host information
Authentication mechanisms
Potentially sensitive files
SMB should be restricted to authorized networks and properly hardened.
5. Remote Shell Services
The scan identified:
512/tcp
513/tcp
514/tcp
These services represent legacy remote-access functionality and can significantly increase security risk.
They should be disabled unless there is a specific operational requirement.
6. Database Services
The assessment identified:
3306/tcp  MySQL
5432/tcp  PostgreSQL
Database services should not normally be directly exposed to untrusted networks.
Recommended controls include:
Network segmentation
Firewall restrictions
Strong authentication
Least-privilege database accounts
Encryption
Regular patching
7. VNC — Port 5900
5900/tcp open vnc
VNC provides remote graphical access.
If required, it should be:
Restricted by firewall rules
Protected by strong authentication
Accessible only through trusted networks or VPN
Properly encrypted
8. Java RMI — Port 1099
1099/tcp open java-rmi
Java RMI can expose remote Java functionality and should not be unnecessarily accessible from untrusted networks.
9. Metasploitable Root Shell — Port 1524
1524/tcp open ingreslock
The service identification corresponds to the deliberately vulnerable shell service included in Metasploitable 2.
This finding demonstrates why exposed administrative or shell services should be carefully controlled in production environments.
🖥️ Nmap Service Detection
The following command was used for service enumeration:
sudo nmap -sV 192.168.174.128
For additional information about the target operating system, the following can be used in the authorized lab:
sudo nmap -O 192.168.41.130
A more comprehensive authorized lab scan can be performed with:
sudo nmap -sV -O 192.168.41.130
📸 Evidence
Screenshots captured during the assessment demonstrate:
Local interface configuration
Target host discovery
Nmap port enumeration
Service/version detection
Identification of the Metasploitable environment

🛡️ Recommended Remediation
From a defensive security perspective, the following controls are recommended:
Network Security
Implement network segmentation.
Restrict unnecessary inbound connections.
Apply host-based firewall rules.
Limit administrative services to trusted management networks.
Service Hardening
Disable unnecessary services.
Remove legacy protocols such as Telnet.
Replace insecure FTP with SFTP/FTPS.
Restrict SMB access.
Secure remote administration services.
Patch Management
Maintain supported versions of operating systems and applications.
Apply security patches regularly.
Remove obsolete software from production systems.
Authentication
Enforce strong passwords.
Use key-based SSH authentication where appropriate.
Disable unnecessary default accounts.
Apply least-privilege principles.
Monitoring
Monitor exposed services.
Centralize authentication and system logs.
Alert on unusual network connections.
Periodically perform vulnerability assessments.
📈 Risk Summary
Finding
Potential Risk
FTP exposed
High
Telnet exposed
High
Legacy remote shell services
High
SMB exposed
High
Outdated web server
High
Database services exposed
High
VNC exposed
High
Java RMI exposed
High
Large number of open services
High
The overall attack surface is intentionally high, which is expected because Metasploitable 2 is designed as a vulnerable penetration-testing target.
🧠 Skills Demonstrated
This project demonstrates practical knowledge of:
Network reconnaissance
Host discovery
TCP/IP networking
Port scanning
Nmap
Service enumeration
Version detection
Network attack-surface analysis
Security risk identification
Vulnerability assessment methodology
Linux command-line operations
VMware virtual laboratory environments
Cybersecurity documentation
Defensive remediation planning
🔐 Ethical Considerations
This assessment was performed against an intentionally vulnerable system in a controlled laboratory environment.
All scanning and security testing should only be performed against systems for which explicit authorization has been obtained.
Unauthorized scanning or exploitation of systems and networks may violate organizational policies and applicable laws.
📌 Conclusion
The Nmap assessment successfully identified 192.168.41.130 as an active Metasploitable 2 host and enumerated a large number of exposed network services.
The results demonstrate how service enumeration can provide security professionals with an understanding of a system's attack surface before conducting deeper vulnerability assessment.
The assessment also demonstrates the importance of:
Reducing unnecessary exposed services
Eliminating legacy protocols
Applying security patches
Restricting network access
Implementing strong authentication
Continuously monitoring exposed infrastructure
This project forms part of a practical cybersecurity portfolio demonstrating hands-on experience with network reconnaissance, enumeration, vulnerability assessment, and security analysis.ls
