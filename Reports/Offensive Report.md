# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

![nmao_1](https://github.com/crashandmayhem/Final-Project/blob/main/Images/nmap%20-sV.png)

![nmap_2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/nmap%20-sV_2.png)

This scan identifies the services below as potential points of entry:
- Target 1 _192.168.1.110_
![target1](https://github.com/crashandmayhem/Final-Project/blob/main/Images/target1%20points%20of%20entry.png)

  - **_Port 22_**: SSH (OpenSSH 6.7p1 Debian)
  - **_Port 80_**: HTTP (Apache httpd 2.4.10)
  - **_Port 111_**: rpcbind (2-4 RPC #100000)
  - **_Port 139_**: netbios-ssn (Samba smbd 3.X - 4.X)
  - _** Port 445_**: netbios-ssn (Samba smbd 3.X - 4.X)

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on each target:
- Target 1
  - List of
  - Critical
  - Vulnerabilities

_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
