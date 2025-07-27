# Internal Server Penetration Test Report
## HTB User Credential Discovery Lab

---

### Executive Summary

This report documents a comprehensive penetration test conducted on an internal Windows server (10.129.202.41) with the objective of discovering credentials for a user named "HTB". The assessment successfully identified multiple attack vectors and ultimately recovered the target credentials through systematic enumeration and privilege escalation.

**Key Findings:**
- Multiple network services exposed with varying access controls
- Sensitive credential information stored in unprotected network shares
- SQL Server database containing developer account credentials
- Successful credential recovery for the HTB user

---

### Scope and Objectives

**Target:** Internal Windows Server (10.129.202.41)
**Primary Objective:** Locate and extract credentials for user "HTB"
**Methodology:** Black-box penetration testing approach
**Tools Used:** Nmap, SMB enumeration tools, NFS utilities, RDP clients, SQL Server tools

---

### Methodology and Findings

#### Phase 1: Network Discovery and Port Scanning

**Initial TCP Port Scan:**
```bash
nmap -sV -sC 10.129.202.41
```

**Key Ports Identified:**
- Port 111/tcp - RPC Bind
- Port 135/tcp - Microsoft Windows RPC
- Port 139/tcp - NetBIOS Session Service
- Port 445/tcp - Microsoft Directory Services (SMB)
- Port 2049/tcp - Network File System (NFS)
- Port 3389/tcp - Remote Desktop Protocol (RDP)

**System Information Discovered:**
- Hostname: WINMEDIUM
- Operating System: Windows Server (Build 10.0.17763)
- Domain: WINMEDIUM

#### Phase 2: Service Enumeration

**NFS Enumeration:**
```bash
showmount -e 10.129.202.41
```
**Result:** Discovered `/TechSupport` export accessible to everyone

**NFS Mount and Analysis:**
```bash
sudo mount -t nfs 10.129.202.41:/TechSupport /tmp/nfs_mount
```

**Critical Finding:** Located support ticket file `ticket4238791283782.txt` containing:
- SMTP configuration with embedded credentials
- Username: alex
- Password: lol123!mD
- Email domain: web.dev.inlanefreight.htb

#### Phase 3: SMB Share Analysis

**SMB Enumeration with Discovered Credentials:**
```bash
smbclient -L //10.129.202.41 -U alex%'lol123!mD'
smbmap -H 10.129.202.41 -u alex -p 'lol123!mD'
```

**Accessible Shares Identified:**
- `devshare` - READ/WRITE access
- `Users` - READ ONLY access
- `IPC$` - READ ONLY access

**devshare Analysis:**
Located `important.txt` containing additional credentials:
- Username: sa (System Administrator)
- Password: 87N1ns@slls83

#### Phase 4: UDP Service Discovery

**UDP Port Scan:**
```bash
sudo nmap -sU --top-ports 1000 10.129.202.41
```

**Additional Services Identified:**
- Port 123/udp - NTP (open|filtered)
- Port 161/udp - SNMP (open|filtered)

**SNMP Enumeration Attempts:**
Multiple SNMP community string tests performed but yielded no additional credential information.

#### Phase 5: Remote Access and Privilege Escalation

**RDP Access Attempts:**
Initial RDP connections faced display configuration challenges in the testing environment.

**Alternative Access Methods:**
- Attempted WinRM connections (port 5985 confirmed open)
- Tested various authentication methods with discovered credentials

**Successful RDP Connection:**
Successfully established RDP session using alex credentials after resolving display issues.

#### Phase 6: Internal System Analysis

**Local System Enumeration:**
From RDP session, conducted internal reconnaissance:
- Verified local user accounts
- Attempted privilege escalation with SA credentials
- Explored file system for additional credential stores

#### Phase 7: SQL Server Investigation

**Database Service Discovery:**
Identified Microsoft SQL Server running on localhost (not externally accessible).

**SQL Server Access:**
```sql
sqlcmd -S localhost -E
```
Successfully connected using Windows Authentication.

**Database Enumeration:**
```sql
SELECT name FROM sys.databases;
GO
```

**Databases Identified:**
- master (system database)
- tempdb (system database)
- model (system database)
- msdb (system database)
- accounts (custom database - TARGET)

**Target Database Analysis:**
```sql
USE accounts;
GO
SELECT name FROM sys.tables;
GO
```

**Critical Table Discovered:** `devsacc` (developer accounts)

**Final Credential Recovery:**
```sql
SELECT * FROM devsacc;
GO
```

**OBJECTIVE ACHIEVED:** HTB user credentials successfully extracted from the devsacc table.

---

### Attack Chain Summary

1. **Initial Reconnaissance** → Port scanning revealed multiple services
2. **NFS Exploitation** → Mounted TechSupport share, found support ticket
3. **Credential Harvesting** → Extracted alex user credentials from ticket
4. **SMB Enumeration** → Used alex creds to access additional shares
5. **Privilege Discovery** → Found SA (sysadmin) credentials in devshare
6. **Remote Access** → Established RDP session for internal access
7. **Internal Enumeration** → Discovered SQL Server running internally
8. **Database Compromise** → Accessed accounts database via SQL Server
9. **Target Achievement** → Extracted HTB credentials from developer accounts table

---

### Security Vulnerabilities Identified

#### Critical Vulnerabilities:
1. **Unprotected NFS Export:** TechSupport share accessible without authentication
2. **Credential Exposure:** Plaintext credentials stored in support ticket files
3. **Excessive SMB Permissions:** User accounts with unnecessary write access to shares
4. **Insecure Credential Storage:** Administrator credentials stored in plaintext files
5. **Database Security:** Developer credentials stored without encryption in SQL database

#### Medium Vulnerabilities:
1. **Service Exposure:** Multiple unnecessary services exposed to network
2. **SNMP Configuration:** SNMP service potentially misconfigured
3. **Authentication Methods:** Weak password policies evidenced by discovered credentials

---

### Recommendations

#### Immediate Actions:
1. **Secure NFS Exports:** Implement proper access controls on NFS shares
2. **Credential Management:** Remove plaintext credentials from file shares
3. **Database Security:** Encrypt sensitive data in SQL Server databases
4. **Access Control Review:** Audit and restrict SMB share permissions
5. **Service Hardening:** Disable unnecessary network services

#### Long-term Improvements:
1. **Privileged Access Management:** Implement PAM solution for administrative accounts
2. **Network Segmentation:** Isolate internal services from broader network access
3. **Monitoring and Logging:** Enhanced logging for credential access and database queries
4. **Security Awareness:** Training for secure credential handling in support processes
5. **Regular Security Assessments:** Periodic penetration testing to identify vulnerabilities

---

### Technical Skills Demonstrated

- **Network Reconnaissance:** Comprehensive port scanning and service identification
- **Protocol Analysis:** NFS, SMB, RDP, and SQL Server protocol knowledge
- **Credential Harvesting:** Multiple techniques for discovering stored credentials
- **Privilege Escalation:** Systematic approach to gaining elevated access
- **Database Exploitation:** SQL Server enumeration and data extraction
- **Tool Proficiency:** Expert use of penetration testing tools and techniques

---

### Conclusion

This engagement successfully demonstrated a complete attack path from initial network discovery to target credential extraction. The assessment revealed multiple security vulnerabilities that, when chained together, allowed unauthorized access to sensitive credential information. The systematic methodology employed showcases advanced penetration testing capabilities and thorough understanding of Windows infrastructure security.

The HTB user credentials were successfully recovered, demonstrating the real-world impact of the identified vulnerabilities and the importance of implementing comprehensive security controls across all system components.

---

**Report Prepared By:** Robert Perez  
**Date:** July 27, 2025  
**Assessment Type:** HTB Academy Footprinting Lab - Medium
