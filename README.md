# HackTheBox Penetration Testing Lab

# Information Security Module Overview

This module provides a comprehensive introduction to information security (infosec) fundamentals and penetration testing. Information security is an expansive field focused on protecting data from unauthorized access, modifications, and disruptions. The discipline encompasses multiple specializations including network security, application security, digital forensics, incident response, and business continuity planning. At its core, infosec maintains the **CIA triad** - confidentiality, integrity, and availability of data.

## Risk Management Process

Organizations implement a structured five-step risk management process:

1. **Identifying** potential risks (legal, environmental, market, regulatory)
2. **Analyzing** risks to determine impact and probability
3. **Evaluating** and prioritizing risks for appropriate response strategies
4. **Dealing** with risks through elimination or containment
5. **Monitoring** risks continuously for situational changes

## Red Team vs. Blue Team

The infosec community operates through two primary roles:

- **Red Team**: Acts as adversaries, conducting penetration testing and offensive security techniques to identify vulnerabilities
- **Blue Team**: Serves as defenders, comprising the majority of infosec positions focused on strengthening organizational defenses, policy development, and incident response

## Penetration Testing Role

Security assessors, including penetration testers and red teamers, help organizations identify external and internal network risks. They provide comprehensive assessments ranging from white-box penetration tests to targeted red team scenarios, delivering actionable guidance for vulnerability mitigation and remediation.

## Learning Objectives

This module focuses on hands-on infosec and penetration testing, covering:

- Selecting and navigating penetration testing distributions
- Common technologies and essential tools
- Penetration testing fundamentals and methodologies
- Practical exercises using the Hack The Box platform

While examples use HTB and vulnerable machines, the fundamental skills apply to any environment.

## Next Steps

After completing this module, proceed to the next module in the series to continue building your information security and penetration testing skills.
# Network Enumeration with Nmap	
# Enumeration

## Module Overview

This module focuses on enumeration, the most critical phase of penetration testing. Enumeration is the art of identifying all possible attack vectors against a target system through systematic information gathering. Rather than simply gaining access, the goal is to discover every potential way to attack a target through careful analysis and interaction with services.

## Key Concepts

### The Foundation of Success

Enumeration is not just about running tools - it requires understanding how to interpret and act upon the information gathered. Tools are merely instruments; knowledge, attention to detail, and active interaction with services are what make enumeration effective. The phase demands understanding service functionality, communication protocols, and syntax for meaningful interaction.

### Information Gathering Strategy

The core principle is simple: **more information equals easier attack vector identification**. Like finding misplaced keys, vague information ("in the living room") takes time to act upon, while specific details ("third drawer of the white shelf next to the TV") enable immediate action.

## Learning Objectives

This module covers:

- Active service interaction and information extraction techniques
- Understanding service technologies and communication protocols
- Identifying misconfigurations and security oversights
- Manual enumeration techniques that bypass automated security measures
- Developing systematic approaches to information gathering

## Attack Vector Categories

Most access methods fall into two categories:
1. **Functions and resources** that allow target interaction or provide additional information
2. **Information sources** that reveal critical access-enabling details

## Key Takeaways

- Enumeration is often misunderstood as simply trying more tools
- Success comes from understanding service functionality and relevant interaction methods
- Manual enumeration is critical - automated tools cannot always bypass security measures
- Investment in learning service fundamentals saves significant time in achieving objectives

# Footprinting Module

## Overview

This comprehensive module focuses on manual enumeration techniques, footprinting methodologies, and interaction protocols for a diverse range of network services. Designed for cybersecurity professionals and penetration testers, this module provides hands-on experience with essential reconnaissance and enumeration skills.

## Learning Objectives

Upon completion of this module, you will master manual techniques for:
- **Enumeration principles** - Core methodologies and systematic approaches
- **Infrastructure-based enumeration** - Network infrastructure discovery and mapping
- **Service-specific enumeration** across multiple protocols and platforms

## Covered Services and Protocols

### File and Network Services
- **FTP** (File Transfer Protocol)
- **SMB** (Server Message Block)
- **NFS** (Network File System)

### Communication and Mail Services
- **DNS** (Domain Name System)
- **SMTP** (Simple Mail Transfer Protocol)
- **IMAP/POP3** (Internet Message Access Protocol / Post Office Protocol)

### Management and Database Services
- **SNMP** (Simple Network Management Protocol)
- **MySQL / MSSQL** (Database management systems)
- **IPMI** (Intelligent Platform Management Interface)
- **Windows and Linux remote management protocols**

## Certification Alignment

This module aligns with multiple professional certification standards:

| Certification | Relevant Sections |
|---------------|-------------------|
| **CREST CPSA/CRT** | All sections |
| **CREST CCT APP** | All sections |
| **CREST CCT INF** | All sections |

## Module Structure

### Format
- **Difficulty Level**: Medium
- **Structure**: Sectioned approach with hands-on exercises
- **Assessment**: Practical skills assessment upon completion
- **Flexibility**: Start/stop capability with progress tracking

### Interactive Elements
- Hands-on exercises for each technique
- Real-world command examples with output
- Target hosts for practice environments
- Skills assessment for knowledge validation

## Prerequisites

### Required Knowledge
- Working proficiency with Linux command line
- Understanding of information security fundamentals

### Recommended Prerequisite Modules
1. **Linux Fundamentals**
2. **Network Enumeration with Nmap**
3. **Introduction to Networking**
4. **Windows Fundamentals**

## Getting Started

### Practice Environment
- Interactive sections with provided target hosts
- Compatible with personal virtual machines
- No time constraints or grading pressure

### Completion Requirements
- Complete all section exercises
- Pass the practical skills assessment
- Earn maximum cubes for pathway progression

## Best Practices

### Learning Approach
- Reproduce example commands in your practice environment
- Experiment with variations of demonstrated techniques
- Document your findings and methodologies
- Practice systematic enumeration approaches

### Skills Development
- Focus on understanding underlying protocols
- Develop efficient enumeration workflows
- Master both automated and manual techniques
- Build comprehensive service fingerprinting abilities

## Support and Resources

This module provides comprehensive coverage of manual enumeration techniques essential for cybersecurity professionals. The hands-on approach ensures practical skill development applicable to real-world penetration testing and security assessment scenarios.

# Information Gathering - Web Edition

## Module Overview

This repository contains my completed coursework, notes, and practical exercises from the comprehensive web reconnaissance module. The module equipped me with essential web reconnaissance skills crucial for ethical hacking and penetration testing, exploring both active and passive information gathering techniques.

## Module Description

In the vast landscape of the internet, knowledge is power. For security professionals, understanding the intricacies of a target's digital presence is paramount. This module provided comprehensive training in web reconnaissance - the art and science of gathering information about websites and web applications.

## Skills & Techniques Covered

### Core Topics Mastered

- **🔍 WHOIS Analysis** - Understanding WHOIS protocols and leveraging domain registration data for reconnaissance
- **🌐 DNS Investigation** - Deep dive into Domain Name System mechanics and reconnaissance applications  
- **📡 Subdomain Discovery** - Advanced techniques for discovering and analyzing subdomains across target infrastructure
- **🕷️ Web Crawling** - Automated website exploration and systematic information gathering methodologies
- **📚 Web Archives** - Utilizing Wayback Machine and historical data for intelligence gathering
- **🔎 Search Engine Discovery** - Advanced search techniques and OSINT gathering through search engines
- **🔧 Technology Fingerprinting** - Identifying and analyzing technologies powering websites and web applications

### Hands-On Techniques Practiced

- **WHOIS enumeration** and registration data analysis
- **DNS interrogation** using dig, nslookup, and specialized tools  
- **Subdomain bruteforcing** and enumeration techniques
- **DNS zone transfer** testing and exploitation
- **Virtual host** discovery and analysis
- **Certificate Transparency** log mining
- **Web crawling** with automated tools and manual techniques
- **robots.txt** analysis and exploitation
- **Well-known URI** discovery and reconnaissance
- **Search engine dorking** and advanced query techniques
- **Web archive** mining for historical intelligence
- **Automated reconnaissance** workflow development

## Module Structure Completed

### ✅ Core Sections
- [x] **Introduction** - Web reconnaissance fundamentals
- [x] **WHOIS** - Protocol understanding and applications  
- [x] **Utilizing WHOIS** - Practical WHOIS reconnaissance
- [x] **DNS** - Domain Name System deep dive
- [x] **Digging DNS** - Advanced DNS interrogation techniques
- [x] **Subdomains** - Subdomain discovery methodologies
- [x] **Subdomain Bruteforcing** - Automated subdomain enumeration
- [x] **DNS Zone Transfers** - Zone transfer attacks and testing
- [x] **Virtual Hosts** - Virtual host discovery techniques
- [x] **Certificate Transparency Logs** - CT log mining for reconnaissance

### ✅ Advanced Techniques  
- [x] **Fingerprinting** - Technology identification and analysis
- [x] **Crawling** - Web crawling fundamentals and applications
- [x] **robots.txt** - Robots file analysis and exploitation
- [x] **Well-Known URIs** - Standard URI discovery techniques
- [x] **Creepy Crawlies** - Advanced crawling methodologies
- [x] **Search Engine Discovery** - OSINT and search engine techniques
- [x] **Web Archives** - Historical data mining and analysis
- [x] **Automating Recon** - Workflow automation and tool chaining

### ✅ Assessment
- [x] **Skills Assessment** - Comprehensive practical evaluation

## Prerequisites Completed

This module built upon foundational knowledge from:
- **Linux Fundamentals** - Command line proficiency and system navigation
- **Web Requests** - HTTP/HTTPS protocol understanding  
- **Introduction to Web Applications** - Web application architecture and security basics

## Key Learning Outcomes

Through completing this module, I developed expertise in:

### Technical Skills
- **Reconnaissance Methodology** - Systematic approach to information gathering
- **OSINT Techniques** - Open-source intelligence collection and analysis
- **DNS Security Assessment** - DNS infrastructure analysis and vulnerability identification
- **Web Technology Analysis** - Comprehensive web application fingerprinting
- **Automation Development** - Creating efficient reconnaissance workflows

### Operational Knowledge
- **Active vs Passive Techniques** - Strategic selection of reconnaissance approaches
- **Stealth Considerations** - Minimizing detection during information gathering
- **Data Correlation** - Synthesizing information from multiple sources
- **Target Profiling** - Building comprehensive target intelligence profiles

## Tools & Technologies Utilized

### DNS Reconnaissance
- **dig** - DNS lookup and interrogation
- **nslookup** - DNS query and resolution
- **dnsenum** - DNS enumeration tool
- **fierce** - DNS scanner and subdomain discovery

### Web Analysis
- **Burp Suite** - Web application security testing platform
- **OWASP ZAP** - Web application scanner
- **Nikto** - Web server vulnerability scanner
- **curl/wget** - HTTP client utilities

### OSINT & Discovery
- **theHarvester** - Email and subdomain gathering
- **Sublist3r** - Subdomain enumeration
- **Google Dorking** - Advanced search techniques
- **Wayback Machine** - Web archive analysis

### Automation & Scripting
- **Custom bash scripts** - Automated reconnaissance workflows
- **Python tools** - Custom reconnaissance applications
- **Tool chaining** - Integrated reconnaissance pipelines

## Practical Applications

The skills gained from this module apply directly to:
- **Penetration Testing** - Initial reconnaissance and attack surface mapping
- **Bug Bounty Hunting** - Target analysis and vulnerability discovery
- **Security Assessments** - Comprehensive security posture evaluation
- **Threat Intelligence** - Digital footprint analysis and monitoring
- **Red Team Operations** - Advanced persistent threat simulation

## Repository Structure

```
information-gathering-web/
├── notes/                  # Detailed module notes and theory
├── exercises/             # Hands-on exercise solutions
├── tools/                 # Custom scripts and tool configurations  
├── examples/              # Practical examples and case studies
├── automation/            # Reconnaissance automation scripts
├── resources/             # Additional references and materials
└── skills-assessment/     # Skills assessment documentation
```

## Ethical Use Statement

All techniques and methodologies learned in this module are intended for:
- **Authorized security testing** with proper written permission
- **Educational purposes** and skill development
- **Defensive security** improvement and awareness
- **Legal bug bounty** and responsible disclosure programs

## Certification & Completion

- ✅ **All exercises completed** with hands-on practice
- ✅ **Skills assessment passed** demonstrating practical competency  
- ✅ **Maximum cubes earned** for comprehensive module completion
- ✅ **Applicable to learning paths** for continued cybersecurity education

# Vulnerability Assessment (in-progress)

# File Transfers (to be completed)	

# Shells & Payloads	(to be completed)

# Using the Metasploit Framework (to be completed)	

# Password Attacks (to be completed)	

# Attacking Common Services (to be completed)	

# Pivoting, Tunneling, and Port Forwarding (to be completed)	

# Active Directory Enumeration & Attacks (to be completed)	
	
# Using Web Proxies (to be completed)	
	
# Attacking Web Applications with Ffuf (to be completed)	

# Login Brute Forcing (to be completed)	

# SQL Injection Fundamentals (to be completed)	

# SQLMap Essentials (to be completed)	

# Cross-Site Scripting (XSS) (to be completed)	

# File Inclusion (to be completed)	

# File Upload Attacks (to be completed)	

# Command Injections (to be completed)	

# Web Attacks (to be completed)	
	
# Attacking Common Applications (to be completed)	
	
# Linux Privilege Escalation (to be completed)	

# Windows Privilege Escalation (to be completed)	

# Documentation & Reporting (to be completed)	

# Attacking Enterprise Networks (to be completed)
