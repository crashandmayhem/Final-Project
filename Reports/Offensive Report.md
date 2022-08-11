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
  - **_Port 445_**: netbios-ssn (Samba smbd 3.X - 4.X)

The following vulnerabilities were identified on _Target 1_:
  - _CVE-2021-28041_ Open SSH
  - _CVE-2017-15710_ Apache https 2.4.10
  - _CVE-2017-8779_ Open rpcbind port could lead to DDoS
  - _CVE-2017-7494_ Samba NetBios

Other Vulnerabilities Include:

  - WordPress User Enumeration
    - Attackers were able to obtain a list of Usernames and taylor the attacks
  - Weak User Passwords
    - Attackers were able to Brute-Force one password and John the Ripper another  
  - Sensitive Data Exposure
    - Attackers were able to locate and use wp-config.php to log into SQL Database to retrieve more information
  - Python sudo privilege escalation

### Exploitation

_User Enumeration_

![wpscan](https://github.com/crashandmayhem/Final-Project/blob/main/Images/wpscan_1.png)

![wpscan2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/wpscan_3.png)

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
