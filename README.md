# HTB Academy Footprinting Lab - Easy Report
## Internal DNS Server Enumeration and Flag Recovery

---

### Executive Summary

This report documents a comprehensive footprinting assessment conducted on an internal DNS server (10.129.227.207) belonging to Inlanefreight Ltd. The objective was to gather maximum information about the server while identifying potential attack vectors without aggressive exploitation. The assessment successfully recovered the flag.txt file through systematic enumeration and credential-based access.

**Key Findings:**
- Multiple network services exposed including DNS, FTP, and SSH
- Employee credentials discovered and validated across multiple services
- SSH private key recovered enabling privileged access
- Successful flag.txt file recovery demonstrating information disclosure risks

---

### Scope and Objectives

**Target:** Internal DNS Server (10.129.227.207)
**Company:** Inlanefreight Ltd
**Primary Objective:** Enumerate server services and locate flag.txt file
**Constraints:** No aggressive exploitation - services are in production
**Given Intelligence:** Employee credentials "ceil:qwer1234" and mention of SSH keys
**Methodology:** Passive footprinting and service enumeration

---

### Methodology and Findings

#### Phase 1: Network Discovery and Port Scanning

**Initial Reconnaissance:**
```bash
nmap -sV -sC 10.129.227.207
```

**Services Identified:**
- Port 22/tcp - SSH (OpenSSH)
- Port 53/tcp - DNS 
- Port 2121/tcp - FTP (ProFTPD Server - Ceil's FTP)
- Additional services discovered during comprehensive scanning

**Service Fingerprinting:**
ProFTPD Server (Ceil's FTP) exposed on port 2121, revealing the home directory of user ceil

#### Phase 2: DNS Enumeration

**DNS Service Analysis:**
- Confirmed DNS server functionality on port 53
- Attempted zone transfers and subdomain enumeration
- Gathered DNS configuration information for the inlanefreight.htb domain

**DNS Footprinting Commands:**
```bash
dig @10.129.227.207 inlanefreight.htb
dig @10.129.227.207 inlanefreight.htb ANY
dig @10.129.227.207 inlanefreight.htb AXFR
```

#### Phase 3: FTP Service Enumeration

**FTP Access with Provided Credentials:**
Using the provided employee credentials: username "ceil" and password "qwer1234" for FTP access

```bash
ftp 10.129.227.207 2121
# Username: ceil
# Password: qwer1234
```

**FTP Directory Analysis:**
- Successfully authenticated to Ceil's FTP server
- Enumerated home directory structure
- Located and downloaded accessible files
- **Critical Discovery:** SSH private key file (id_rsa) found in user directory

#### Phase 4: SSH Key Recovery and Analysis

**SSH Private Key Discovery:**
During FTP enumeration, an SSH private key was discovered in the user's directory:

```bash
# Downloaded from FTP server
get .ssh/id_rsa
```

**Key Validation:**
```bash
chmod 600 id_rsa
ssh-keygen -l -f id_rsa
```

#### Phase 5: SSH Access and System Enumeration

**SSH Connection with Private Key:**
SSH access achieved using the discovered id_rsa private key

```bash
ssh -i id_rsa ceil@10.129.227.207
```

**Post-Authentication Enumeration:**
Upon successful SSH login, conducted system reconnaissance:

```bash
# System information gathering
uname -a
id
pwd
ls -la

# File system enumeration
find / -name "flag.txt" 2>/dev/null
find /home -name "flag.txt" 2>/dev/null
find . -name "flag.txt" 2>/dev/null
```

#### Phase 6: Flag Recovery

**Flag.txt Location and Recovery:**
Through systematic file system enumeration, the flag.txt file was located and its contents retrieved:

```bash
# Flag location discovered
cat /path/to/flag.txt
```

**Objective Achieved:** Flag contents successfully extracted and submitted.

---

### Attack Vector Analysis

#### Primary Attack Path:
1. **Service Discovery** → Comprehensive port scanning revealed accessible services
2. **Credential Validation** → Given credentials tested across discovered services
3. **FTP Enumeration** → Authenticated FTP access provided file system visibility
4. **SSH Key Harvesting** → Private SSH key discovered in user's FTP directory
5. **Privilege Escalation** → SSH key enabled direct system access
6. **Information Disclosure** → System access allowed flag.txt recovery

#### Alternative Enumeration Paths:
- DNS zone transfer attempts (if misconfigured)
- SSH password authentication with provided credentials
- Additional FTP directory traversal techniques
- Service version exploitation research (within scope constraints)

---

### Technical Skills Demonstrated

#### Network Reconnaissance:
- Comprehensive port scanning and service identification
- Protocol-specific enumeration techniques
- Service version fingerprinting

#### Credential Management:
- Multi-service credential validation
- SSH key format recognition and usage
- Proper key permission management

#### File System Analysis:
- FTP directory traversal and file retrieval
- SSH-based system enumeration
- Systematic file search methodologies

#### Information Security:
- Understanding of service interdependencies
- Recognition of credential reuse patterns
- Identification of information disclosure vectors

---

### Security Vulnerabilities Identified

#### Critical Findings:
1. **Credential Exposure:** Employee credentials accessible and reusable across services
2. **SSH Key Storage:** Private SSH keys stored in accessible FTP directories
3. **Service Information Disclosure:** FTP banner revealing user information
4. **Excessive File Permissions:** Sensitive files accessible via multiple protocols

#### Medium Risk Issues:
1. **Service Enumeration:** Multiple services providing reconnaissance opportunities
2. **DNS Configuration:** Potential for information leakage through DNS queries
3. **FTP Configuration:** Home directory exposure through FTP service

---

### Recommendations

#### Immediate Security Improvements:
1. **Credential Management:** Implement password rotation policies and avoid credential reuse
2. **SSH Key Security:** Store private keys securely, not in web-accessible directories
3. **Service Hardening:** Review and restrict unnecessary service exposures
4. **Access Controls:** Implement principle of least privilege for service accounts

#### Long-term Security Enhancements:
1. **Multi-Factor Authentication:** Implement MFA for critical service access
2. **Network Segmentation:** Isolate internal services from broader network access
3. **Monitoring and Logging:** Enhanced logging for service access and file operations
4. **Security Awareness Training:** Employee education on secure credential handling
5. **Regular Security Assessments:** Periodic footprinting exercises to identify exposures

---

### Lessons Learned

#### Footprinting Effectiveness:
- **Service Interdependency:** Access to one service (FTP) provided credentials for another (SSH)
- **Information Cascading:** Each enumerated service provided additional attack surface
- **Credential Validation:** Systematic testing of provided credentials across all services
- **File System Awareness:** Understanding common file locations and permission patterns

#### Professional Development:
- **Methodology Importance:** Systematic enumeration prevents missed opportunities
- **Documentation Value:** Thorough documentation aids in understanding attack chains
- **Scope Compliance:** Maintaining assessment boundaries while maximizing information gathering
- **Client Communication:** Clear reporting of findings and recommendations

---

### Tools and Techniques Utilized

#### Reconnaissance Tools:
- **Nmap:** Port scanning and service enumeration
- **Dig:** DNS query and zone transfer attempts
- **FTP Client:** File transfer and directory enumeration
- **SSH Client:** Remote access and system enumeration

#### Analysis Techniques:
- **Banner Grabbing:** Service version identification
- **Credential Testing:** Multi-service authentication validation
- **File System Enumeration:** Systematic file discovery methods
- **Information Correlation:** Connecting discovered data points

---

### Conclusion

This footprinting assessment successfully demonstrated the information disclosure risks present in the target DNS server infrastructure. Through systematic enumeration and the strategic use of provided credentials, complete system access was achieved, resulting in the recovery of the target flag.txt file.

The assessment revealed that seemingly minor information exposures (employee credentials and SSH keys) can cascade into significant security compromises. The methodology employed showcases the importance of comprehensive footprinting in understanding an organization's security posture and identifying areas for improvement.

The successful completion of this lab demonstrates proficiency in:
- Network service enumeration
- Multi-protocol credential validation
- SSH key management and utilization
- Systematic information gathering
- Professional security assessment reporting

---

**Report Prepared By:** Robert Perez  
**Date:** July 20, 2025  
**Assessment Type:** HTB Academy Footprinting Lab - Easy  
