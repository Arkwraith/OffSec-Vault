---
tags:
  - vulnerability
  - cve
  - linux
  - lpe
  - sudo
severity:
  - High
Vulnerability Category:
  - Local Privilege Escalation
CVE: CVE-2021-3156
CWE: CWE-122
OS:
  - Linux
---
# Brief
CVE-2021-3156, also known as “Baron Samedit,” is a heap-based buffer overflow vulnerability in `sudo` that allows any local user to escalate privileges to root. It affects default configurations and requires no special sudo permissions to exploit. This vulnerability has existed since 2011 and was patched in early 2021.

---
# Overview
Disclosed by Qualys in January 2021, Baron Samedit is a critical vulnerability found in `sudoedit`, a utility within the `sudo` package. The vulnerability lies in how `sudoedit` parses command-line arguments when the `-s` or `-i` flags are used. By crafting a specific command with unescaped backslashes and overflowing the heap, a local attacker can gain root access—even if they are not listed in the sudoers file.

Affected Versions:
- sudo 1.8.2 to 1.8.31p2
- sudo 1.9.0 to 1.9.5p1

---
# Signals
- sudo version within affected range (`sudo -V`)
- Able to trigger heap overflow using:
	```
	sudoedit -s '\' $(python3 -c 'print("A"*1000)')
	```
- Heap corruption or segmentation fault in `sudoedit`
- Can be detected by:
	- [linpeas](Tools/linpeas) -> checks sudo version and configs
	- `sudo -l` → confirms lack of sudo privileges
	- dmesg or `/var/log/syslog` → shows sudo crashes

---
# Exploitation

## Method
1. **Trigger Crash**
	```
	sudoedit -s '\' $(python3 -c 'print("A"*1000)')
	```
	- If vulnerable, this command crashes `sudoedit` due to heap overflow.
2. **Exploit with PoC**
    - Navigate to exploit directory on target machine (e.g., `cd ~/Exploit`)
    - Compile the exploit: `make`
    - Execute the binary with correct target (e.g., target 0 for Ubuntu 18.04): `./exploit 0`
    - If successful, this drops to a root shell

## Under the Hood
- The vulnerability occurs in the argument parsing logic of `sudoedit` when handling the `-s` or `-i` options.
- A crafted input containing unescaped backslashes and excessive characters causes a heap-based buffer overflow.
- This overflow can overwrite heap metadata and control the program flow, ultimately executing arbitrary code as root.

## Alternate Detection
- Run the trigger PoC and observe crash/segfault:
	```
	sudoedit -s '\' $(python3 -c 'print("A"*1000)')
	```
- Check logs:  `dmesg | grep sudo`
- Monitor with tools like `auditd`, `syslog`, or `journalctl` for anomalous sudo behavior.


---
# Pattern
- **Vulnerability Class:** Heap Buffer Overflow (`CWE-122`)
- **Privilege Escalation Vector:** Malformed argument input to `sudoedit`
- **Trigger Mechanism:** Unescaped backslashes + overflowed input length
- **Exploit Mechanism:** Heap corruption leading to arbitrary code execution as root

## Related Techniques
- [Heap Exploitation](Techniques/Heap Exploitation)
- [Local Privilege Escalation](Techniques/Local Privilege Escalation)
- [Sudo Misuse](Techniques/Sudo Misuse)

---
# Tools
- [linpeas](Tools/linpeas) → Check for vulnerable sudo versions
- [gcc](Tools/gcc) → Compile exploit manually if needed
- python3 → Used in basic PoC to trigger overflow
- [exploit-db](Tools/exploitdb) → Find relevant exploit sources
- GitHub PoCs:
    - [blasty/sudo_baronsamedit_exploit](https://github.com/blasty/CVE-2021-3156)
    - [lockedbyte/CVE-2021-3156](https://github.com/lockedbyte/CVE-2021-3156)

---
# Mitigation

## Permanent Fix
- Upgrade `sudo` to at least 1.8.32: `sudo apt update && sudo apt upgrade sudo`

## Temporary Workaround
- Restrict access to `sudoedit`: `chmod -855 /usr/bin/sudoedit`
- Use AppArmor or SELinux to limit `sudoedit` capabilities

## Verify
- Run PoC again; if system is patched, it will not crash or elevate
- Confirm patched version: `sudo -V`

---
# References
- Qualys Advisory: CVE-2021-3156
- [blasty GitHub Exploit](https://github.com/blasty/CVE-2021-3156)
- NVD: CVE-2021-3156
- [lockedbyte GitHub PoC](https://github.com/lockedbyte/CVE-2021-3156)
---
# Notes
- Unlike previous sudo CVEs, this one does not require any sudo privileges.
- Works even in default configurations—extremely dangerous in shared multi-user environments.
- First effective PoC exploit was from bl4sty; Qualys did not initially publish full PoC.
- Example of a vulnerability that persisted unnoticed for nearly a decade.

---
# Related CTFs
[TryHackMe – Baron Samedit](CTFs/Baron Samedit)
