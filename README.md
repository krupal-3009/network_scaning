# network_scaning

## Overview
This document outlines the network reconnaissance performed on the Kioptrix VM as part of a penetration testing exercise. The objective was to identify open ports, running services, and gather system information about the target machine.

## Information
- Target IP: 192.168.128.129
- VM Type: Kioptrix (Vulnerable Linux Distribution)
- Testing Environment: Controlled lab environment
- Date: June 23, 2025

## Methodology
#### **1. Network Discovery**
The reconnaissance began with network discovery to identify live hosts in the network segment.
- Command Used:
   ```nmap -sS 192.168.128.0/24```

 **Purpose:** Performed a SYN stealth scan to identify active hosts in the network range without completing the TCP handshake, making it less detectable.\
 **Results:** Successfully identified the Kioptrix VM at IP address 192.168.128.129 along with other network devices
![](https://github.com/krupal-3009/network_scaning/blob/a2e03f740527e5fa0056cc47dcdb2f015c07dc10/Screenshot%202025-06-23%20165846.png)

 #### **2. Port Enumeration**
 Conducted comprehensive port scanning to identify all open ports on the target system.
 - Command Used:
   ```nmap -p- 192.168.128.129```
   
**Purpose:** Scanned all 65535 TCP ports to ensure no services were missed during the initial reconnaissance.

**Key Findings:**
- Port 22/tcp: SSH (OpenSSH 2.9p2)
- Port 80/tcp: HTTP (Apache 1.3.20)
- Port 111/tcp: RPCbind
- Port 139/tcp: NetBIOS-SSN (Samba)
- Port 443/tcp: HTTPS (Apache 1.3.20 with SSL/TLS)
- Port 32768/tcp: RPC Status

![](https://github.com/krupal-3009/network_scaning/blob/a2e03f740527e5fa0056cc47dcdb2f015c07dc10/Screenshot%202025-06-23%20170414.png)

#### **3. Service Version Detection**
Performed aggressive scanning to gather detailed information about services and the operating system.
- Command Used:
  ```nmap -A 192.168.128.129```

**Purpose:** Combined OS detection, version detection, script scanning, and traceroute to gather comprehensive intelligence about the target.
#### Detailed Findings:

***Web Services***
- Apache 1.3.20 running on ports 80 and 443
- SSL/TLS Configuration: Multiple cipher suites detected including weak encryption methods
- HTTP Methods: Standard HTTP methods available
- SSL Certificate: Self-signed certificate with expired validity

***SSH Service***
- OpenSSH 2.9p2 (outdated version with known vulnerabilities)
- Host Key Types: RSA, DSA, and RSA1 keys detected
- Encryption: Multiple cipher options available

***Samba/NetBIOS***
- Samba SMB service running on port 139
- Workgroup: MYGROUP identified
- NetBIOS: Available for file sharing enumeration

***RPC Services***
- RPCbind on port 111
- Status service on port 32768
- Program versions: Multiple RPC program versions detected

![](https://github.com/krupal-3009/network_scaning/blob/a2e03f740527e5fa0056cc47dcdb2f015c07dc10/Screenshot%202025-06-23%20170021.png)
![](https://github.com/krupal-3009/network_scaning/blob/a2e03f740527e5fa0056cc47dcdb2f015c07dc10/Screenshot%202025-06-23%20170043.png)

### 4. Network Topology Analysis
Traceroute analysis revealed the network path to the target.

**Key Findings:**
- Network Distance: 1 hop (same network segment)
- Response Time: 0.66ms average latency
- Network Path: Direct connection within local subnet

![](https://github.com/krupal-3009/network_scaning/blob/a2e03f740527e5fa0056cc47dcdb2f015c07dc10/Screenshot%202025-06-23%20170043.png)

## Security Assessment Summary
### Critical Vulnerabilities Identified
- _Outdated Apache Version (1.3.20):_ Multiple known CVEs associated with this version
- _Legacy OpenSSH (2.9p2):_ Susceptible to various authentication and encryption attacks
- _Weak SSL/TLS Configuration:_ Uses deprecated cipher suites and protocols
- _Exposed RPC Services:_ Potentially vulnerable to enumeration attacks
- _Samba Service:_ NetBIOS exposure could lead to information disclosure
