# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology
![Network Topology](https://github.com/crashandmayhem/Final-Project/blob/main/Images/Final%20Project.drawio.png)

The following machines were identified on the network:
- Name of VM 1 Kali
  - **Operating System**: Linux 5.4.0
  - **Purpose**: Attacking Machine
  - **IP Address**: 192.168.1.90
- Name of VM 2 Capstone
  - **Operating System**: Linux (Ubuntu 18.04.1.LTS)
  - **Purpose**: Used as testing system for alerts
  - **IP Address**: 192.168.1.105
- Name of VM 3 ELK
  - **Operating System**: Linux (Ubuntu 18.04.1 LTS)
  - **Purpose**: Used for gathering information from the Target 1 machine using Metricbeat, Filebeats, and Packetbeats
  - **IP Address**: 192.168.1.100
- Name of VM 4 Target 1
  - **Operating System**: Linux 3.2 - 4.9
  - **Purpose**: The Victim Machine with Wordpress as a vulnerable server
  - **IP Address**: 192.168.1.110

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Alert 1 is implemented as follows:
  - **Metric**: Packetbeat: http.response.status_code > 400
  - **Threshold**: grouped http response status codes above 400 every 5 minutes
    - **When count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes**
  - **Vulnerability Mitigated**:
    - **IPS would block any suspicious IP's**
    - **Used intrusion detection/prevention for attacks**
    - **Filter and disable or close Port 22**
    - **Utilize Account Management to lock or request user accounts to change the passwords every 60 days**
  - **Reliability**: This alert will not generate excessive amounts of false positives identifying brute force attacks. Medium Reliability 
  
  ![Excessive HTTP](https://github.com/crashandmayhem/Final-Project/blob/main/Images/HTTP%20Errors.png)

#### HTTP Request Size Monitor

Alert 2 is implemented as follows:
  - **Metric**: Packetbeat: http.request.bytes
  - **Threshold**: The sum IS ABOVE 3500
    - **When sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute** 
  - **Vulnerability Mitigated**: Code Injections in HTTP requests (XSS) or DDOS attacks
  - **Reliability**: This alert doesn't generate an excessive amount of false positives because DDOS attacks submit requests within seconds and not minutes. Medium Reliability

![HTTP SIZE](https://github.com/crashandmayhem/Final-Project/blob/main/Images/HTTP%20Request%20Size%20Monitor_2.png)

#### CPU Usage Monitor

Alert 3 is implemented as follows:
  - **Metric**: Metricbeat: system.process.cpu.total.pct
  - **Threshold**: Maximum cpu total percentage is over .5 in 5 minutes
    - **WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes**
  - **Vulnerability Mitigated**: Malicious software and/or programs (malware or viruses) taking up resources
  - **Reliability**: Highly Reliable...This alert will generate some false positives due to CPU spikes even if not malicious activity. 

![CPU Monitor](https://github.com/crashandmayhem/Final-Project/blob/main/Images/CPU%20Usage.png)

### Suggestions for Going Further
 
- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1: _Excessive HTTP Errors_
  - **Patch**: WordPress Hardening
    - Regular updates should be implemented
      - WordPress Core
      - PHP Versions
      - Plugins
    - Installing security plugins
    - Disabling unused WordPress features and settings
    - Remove WordPress logins from being publicly accessible
  - **Why It Works**:
    - Regular updates is a simple way to implement patches or fixes to the vulnerabilities/exploits
    - Depending on which _Security Plugins_ installed, we can get great features like malware scans and firewall protection
    - Disabling _REST API_ in WordPress will help mitigate WPScan or enumeration in general. Even though attackers do not get passwords this way, they can still get usernames and attempt Brute-Force-Attacks.
- Vulnerability 2
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 3
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
