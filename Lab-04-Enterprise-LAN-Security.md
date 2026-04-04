## Lab 04: Enterprise LAN Security Assessment

Name: Pauline Kabambi

Course: IT 520

Lab Title: Lab 04 Enterprise LAN Security Assessment

Date: April 3, 2026


Part 1: Network Architecture & Topology Design

1.1 Physical Layer (OSI Layer 1)
The physical layer represents the actual cables and hardware connections in the network. Copper straight-through cables were used to connect end devices to switches and switches to routers. Physical cabling represents Layer 1 because it deals with the actual transmission of raw bits over physical media such as copper cables.


1.2 Data Link Layer (OSI Layer 2)
Switches function at Layer 2 by using MAC addresses to forward frames between devices within the same network segment. This operation at Layer 2 is essential because it is responsible for transferring data between nodes within the same network segment.
Screenshot: 


1.3 Network Layer (OSI Layer 3)
IP addresses were assigned to all router interfaces and static routes were configured to enable communication between the IT, OT, and Remote segment







1.4 Connectivity Verification
Ping tests were performed from PC0, the OT Webserver, and the pump controller to verify connectivity across all three network segments.







Results:
Ping Test
Destination
Result
Same Subnet
10.1.10.11
Success
OT Web Server
192.168.50.10
Success
Remote Pump
172.16.5.10
Success


Part 2: Protocol Security Analysis
2.1 Transport Layer Security (OSI Layer 4)
TCP and UDP were analyzed for their security implications in the enterprise environment. TCP and UDP operate at Layer 4. TCP uses a three-way handshake (SYN, SYN-ACK, ACK) before transmitting data, providing connection reliability and security visibility. UDP is connectionless and sends packets without confirmation, making it faster but harder to monitor for unauthorized access.
Written Response: TCP's three-way handshake provides more security visibility than UDP's connectionless approach because every connection is explicitly established and logged. Security teams can monitor who connected, when, and whether the handshake completed successfully. UDP has no handshake meaning packets are sent with no confirmation, making it harder to detect spoofed traffic or unauthorized access. For OT and SCADA environments TCP's reliability and traceability make it preferable for critical communications.
2.2 Application Layer Security (OSI Layer 7)
HTTP vs HTTPS
Description: Both HTTP and HTTPS were configured on the OT web server and tested from PC0.


Written Response: HTTP sends all data including credentials in plaintext making it trivially interceptable via a man-in-the-middle attack. An attacker on the same network can use packet capture tools to read every byte of traffic. HTTPS wraps traffic in TLS encryption which operates at the Session and Presentation layers (5-6) and encrypts data before transmission. Even if an attacker intercepts the traffic they see only ciphertext. For WBMUD using HTTP to access OT web interfaces would expose SCADA credentials to any attacker with network access making HTTPS essential for all web-based management.
SSH vs Telnet
SSH was configured on Router0 and tested from PC0 to demonstrate secure remote management.


Written Response: Telnet transmits all data including usernames and passwords in cleartext meaning anyone with a packet capture tool can read credentials instantly. SSH uses asymmetric encryption for key exchange and symmetric encryption for the session meaning credentials and commands are never visible on the wire. In enterprise and critical infrastructure environments Telnet is considered unacceptable for remote management precisely because of this exposure. WBMUD should enforce SSH on all network devices and disable Telnet completely.
Part 3: Shellshock Vulnerability Assessment
3.1 Vulnerable LAMP Server Configuration
The OT web server (192.168.50.10) was configured with the Apache web server and CGI enabled, representing a vulnerable LAMP stack environment.



3.2 Exploit Simulation
From the Attacker node (203.0.113.100), a Shellshock attack was simulated against the OT web server.
Screenshot: 

Simulated Attack Command:
curl -A "() { :; }; echo 'Shellshock Vulnerable'" http://192.168.50.10/cgi-bin/test.cgi

Note: This command was documented as a simulation since Packet Tracer does not support real curl execution. In a real environment this command would exploit the Bash vulnerability to execute arbitrary commands on the server.
3.3 Attack Analysis & Mitigation
Exploitation Path: The attacker sends an HTTP request to the Apache web server with a maliciously crafted User-Agent header containing () { :; }; followed by a shell command. Apache configured with CGI passes HTTP headers as environment variables to Bash when invoking the CGI script. Bash's vulnerability causes it to execute any commands appended after a function definition in an environment variable giving the attacker remote code execution.
LAMP Stack Components Involved:
Linux: The underlying OS whose Bash shell contains the vulnerability
Apache: The web server that passes attacker-controlled headers to Bash via CGI
Bash: The vulnerable interpreter that executes injected commands
MySQL and PHP are not directly involved in this specific attack chain
Impact Assessment: With remote code execution on the OT web server an attacker could read SCADA configuration files, pivot laterally to SCADA devices on the 192.168.50.0/24 subnet, install persistent backdoors, exfiltrate operational data, or issue commands to industrial control systems potentially disrupting water treatment operations for over a million residents.
Mitigation Strategies:
Patch Management: Update Bash immediately to a version beyond 4.3 patch 25. Implement automated vulnerability scanning to catch unpatched CVEs within 30 days
WAF Rules: Deploy a Web Application Firewall with rules to block requests containing () { patterns in any HTTP header
Network Segmentation: Restrict external access to the OT network entirely. The attacker node should never be able to reach the OT web server directly
CERT/CC Role: SEI CERT Coordination Center (VU#252743) received disclosure of the Shellshock vulnerability and coordinated with Bash maintainers, Linux distributions, and major vendors to simultaneously release patches. This coordinated disclosure prevents attackers from having advanced knowledge of a vulnerability while ensuring defenders get patches as quickly as possible.




Part 4: Incident Response
4.1 Attack Path Diagram
Screenshot: I used AI to create this diagram

Attack Path Description:
Step
Action
Details
1
Initial Access
Attacker (203.0.113.100) sends Shellshock payload to OT-WebServer (192.168.50.10)
2
Remote Code Execution
Bash executes injected commands via Apache CGI
3
Lateral Movement
Attacker SSH's from web server to SCADA-1 (192.168.50.20) and SCADA-2 (192.168.50.21)
4
Data Exfiltration
2.3 GB of SCADA configuration data sent back to attacker (203.0.113.100)


4.2 Root Cause Analysis
Vulnerability Category
What Went Wrong
Missing Security Control
Unpatched Software
The OT web server was running an outdated version of Bash, vulnerable to Shellshock (CVE-2014-6271), allowing remote code execution through CGI scripts
Automated patch management system with vulnerability scanning to identify and remediate known CVEs within 30 days of disclosure
Network Segmentation
The external attacker had direct network access to the OT subnet with no firewall blocking this path
A properly configured firewall or NGFW should block all unsolicited inbound traffic to OT systems from external networks
Access Controls
Once on the web server the attacker could SSH directly to SCADA devices without additional authentication barriers
Implement jump servers for SCADA access, enforce MFA, apply principle of least privilege
Monitoring & Detection
The attack began at 2:47 AM and 2.3 GB was exfiltrated before detection, logs were also tampered with
Deploy a SIEM with real-time alerting on anomalous outbound transfers and unauthorized SSH sessions









4.3 Remediation Plan
1. Immediate Containment (0-24 hours)
- Immediately isolate the OT network segment by disabling the router interface that connects it to external networks.
- Revoke and rotate all credentials for the OT web server and SCADA devices.
- Preserve forensic evidence by capturing memory dumps and disk images of the compromised web server before any remediation takes place.
- Notify relevant stakeholders, including management, legal counsel, DHS CISA, and the EPA, given the critical infrastructure context.

2. Short-Term Fixes (1-7 days)
- Patch Bash on all Linux systems to a version that is not vulnerable to Shellshock immediately.
- Disable CGI on the OT web server unless it is strictly necessary.
- Implement firewall access control lists (ACLs) that block all traffic from external IPs to the OT subnet.
- Enable comprehensive logging on all devices, with logs forwarded to a centralized tamper-resistant Security Information and Event Management (SIEM) system.
- Conduct a full credential audit and enforce password rotation across all systems.

3. Long-Term Improvements (1-3 months)
- Redesign the network with a defense-in-depth strategy by adding a dedicated firewall between the IT and OT networks, placing the OT web server in a true DMZ.
- Require all SCADA access through a hardened jump server with multi-factor authentication (MFA).
- Adopt zero trust principles so that no device is automatically trusted based on its network location.
- Apply the principle of least privilege to ensure that the web server process cannot initiate SSH connections to SCADA systems.

4. Monitoring & Detection
- Deploy a SIEM with rules to alert on outbound data transfers exceeding 100 MB, SSH connections from the web server to SCADA devices, and modifications of Apache logs.
- Implement network-based intrusion detection at the boundary of the OT segment. These controls would have detected this incident within minutes rather than hours.

5. Organizational Improvements
- Establish a formal patch management policy requiring the remediation of critical Common Vulnerabilities and Exposures (CVEs) within 14 days.
- Implement change control processes so that no configurations are altered without documented approval.
- Conduct annual tabletop exercises simulating incidents similar to this one.
- Provide cybersecurity awareness training and specialized ICS/SCADA security training for all staff.
- Engage a third party to conduct annual penetration testing.
Part 5: OSI Model Mapping Summary
Lab Activity
OSI Layer
Layer Name
Protocol/Technology
Physical cabling
Layer 1
Physical
Copper straight-through cables
Switch MAC tables
Layer 2
Data Link
MAC addressing, Ethernet
IP addressing & routing
Layer 3
Network
IP, Static routing
TCP/UDP ports
Layer 4
Transport
TCP port 80, 443, 22 / UDP 161
TLS/SSL encryption
Layer 5-6
Session/Presentation
TLS, SSL
HTTP, HTTPS, SSH
Layer 7
Application
HTTP, HTTPS, SSH, Telnet








Reflection


In this lab, I designed and implemented a three-tier enterprise network for the Winslow Bay Municipal Utility District, gaining practical experience with network segmentation, protocol security, and incident response.

A key lesson learned is that network architecture decisions significantly affect security outcomes. Segregating IT, OT, and remote networks into separate subnets creates boundaries that limit an attacker's movement. However, the Shellshock simulation demonstrated that improper firewall configuration can compromise this segmentation, allowing attackers direct access to the OT web server.

Understanding protocols at different OSI layers emphasized the importance of encryption. While HTTP exposes traffic in plaintext, HTTPS uses TLS encryption for protection, and SSH secures credentials that Telnet would expose. These concepts have real implications for critical infrastructure like water treatment systems.

The incident response exercise highlighted how a single unpatched vulnerability can lead to a full network compromise. The Shellshock vulnerability allowed for initial access and lateral movement to SCADA devices, culminating in data exfiltration. This underscored the necessity of a defense-in-depth strategy to prevent total compromise.

Overall, this lab reinforced that securing critical infrastructure requires both technical controls, such as patching and firewalls, and organizational measures like patch management policies. As a security engineer for WBMUD, I would prioritize improvements in network segmentation and continuous monitoring.
