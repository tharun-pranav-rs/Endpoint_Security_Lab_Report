1\. Executive Summary

This session bridged the gap between theoretical security concepts and operational tradecraft. I successfully utilized "Living off the Land" (LotL) techniques—using built-in system tools—to perform a security audit of a Windows host. The audit covered four key domains: Network Reconnaissance, Threat Hunting, Digital Forensics, and System Hardening.



2\. Domain: Security Operations \& Incident Response

Mapped Concept: Indicators of Compromise (IoCs) \& Process Analysis.



Activity: Threat Hunting with netstat and tasklist

Theory: Malware often establishes Command \& Control (C2) channels over standard ports to blend in with normal traffic. Identifying "beacons" requires correlating network sockets with system processes.



Practical Application:



Executed netstat -ano to inspect the Network Socket Layer.



Identified active TCP connections in the ESTABLISHED state.



Correlated PID 16376 (identified as the "Chatty" process) with multiple external connections.



Used tasklist | findstr 16376 to positively identify the process as Google Chrome (chrome.exe), ruling it out as a false positive.



Key Skill Demonstrated: Process-to-Port Correlation for identifying suspicious network activity.



3\. Domain: Network Architecture \& Reconnaissance

Mapped Concept: Layer 2 Discovery (ARP) \& Asset Inventory.



Activity: Local Network Mapping with arp

Theory: Attackers use ARP scanning to discover active hosts on a local subnet (Lateral Movement). Defenders use it for asset inventory.



Practical Application:



Executed arp -a to dump the Address Resolution Protocol (ARP) cache.



Identified the Default Gateway (192.168.0.1) and 3 active dynamic peers on the subnet.



Forensic Analysis: Analyzed MAC OUI (Organizationally Unique Identifier) to fingerprint devices.



Identified LG Electronics device (10:76:36) at IP 192.168.0.102.



Identified mobile devices using randomized MAC addresses (Privacy/Anti-tracking features).



Key Skill Demonstrated: Passive Network Discovery and Device Fingerprinting.



4\. Domain: Vulnerability Management (Red Team)

Mapped Concept: Port Scanning \& Service Enumeration.



Activity: Service Discovery with Test-NetConnection

Theory: Open ports represent the "attack surface" of a device. Unnecessary open ports should be closed to reduce risk.



Practical Application:



Utilized PowerShell to perform a targeted TCP Connect Scan against the identified LG device (192.168.0.102).



Test 1 (Port 80): Result False (Closed). Confirmed no web administration interface was exposed (Good Security Hygiene).



Test 2 (Port 8009): Result True (Open). Confirmed the device functions as a Google Cast receiver, validating the asset classification as a Smart TV/Streaming device.



Key Skill Demonstrated: Internal Vulnerability Scanning and Service Validation.



5\. Domain: Digital Forensics

Mapped Concept: Artifact Analysis \& DNS Cache Snooping.



Activity: Analyzing User History with ipconfig /displaydns

Theory: DNS resolution logs persist in volatile memory (RAM) even after browser history is cleared. These artifacts can prove user intent or malware beaconing attempts.



Practical Application:



Dumped the local DNS Resolver Cache.



Evidence Found:



download.mcafee.com (Confirmed Antivirus software update activity).



login.live.com (Confirmed Microsoft Account authentication activity).



host.docker.internal (Confirmed presence of Docker/Containerization software).



Key Skill Demonstrated: Volatile Memory Forensics (DNS Artifacts).



6\. Domain: Governance \& Compliance (Blue Team)

Mapped Concept: Endpoint Hardening \& Firewall Configuration.



Activity: Security Posture Audit with netsh

Theory: Host-based firewalls act as the final line of defense (Zero Trust) when the network perimeter is breached. Policies must default to "Deny All Inbound."



Practical Application:



Audited the active firewall configuration using netsh advfirewall show allprofiles.



Compliance Check:



State: ON (Passed).



Inbound Policy: BlockInbound (Passed).



Enhancement: Enabled logging for "Dropped Connections" to improve visibility into failed attack attempts.



Key Skill Demonstrated: Security Configuration Auditing and System Hardening.



Conclusion

This session successfully transitioned theoretical knowledge into verifiable technical skills. I have demonstrated the ability to use native command-line interface (CLI) tools to assess the security posture of a Windows endpoint without relying on third-party GUI utilities.



Next Steps:



Automate the ARP scanning process using a Python script.



Analyze the newly created Firewall Drop Logs after 24 hours to identify background noise/scanning attempts.

