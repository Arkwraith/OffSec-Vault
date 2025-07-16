---
name: VulnVersity
date: 2025-07-10 17:42
status:
  - Not Started
difficulty: 
category: CTF
platform: 
URL: 
tags:
  - ctf
  - draft
IP: 10.10.10.10
---

# 🧠 CTF Draft – VulnVersity

## 🪜 Attack Chain Table

| Step | Task          | Signals/Vulnerabilities | Techniques/Tools   | Description       | Command Used            | Output | Notes |
| ---- | ------------- | ----------------------- | ------------------ | ----------------- | ----------------------- | ------ | ----- |
| 1    | Initial Recon |                         | [[Tools/nmap]]     | Run full TCP scan | `nmap -sV -T5 -p- <IP>` |        |       |
| 2    |               |                         | [[Tools/Gobuster]] |                   | `gobuster @ip`          |        |       |
| 3    |               |                         |                    |                   |                         |        |       |
| 4    |               |                         |                    |                   |                         |        |       |
| 5    |               |                         |                    |                   |                         |        |       |

---

## 📍 IP / Scope

- Target IP: `xxx.xxx.xxx.xxx` TODO: @IP MAKE RED
- Network Range / Open Ports:
```shell

```
- Initial Access Method:

---

## 🚩 Flags

- User Flag:
- Root Flag:

---

## 🧩 Related Notes (Backlink)
- Vulnerabilities: [[Vulnerabilities/SSTI]]
- Techniques: [[Techniques/SUID Enumeration]]
- Tools: [[Tools/nmap]], [[Tools/LinEnum]]
- Signals: [[Signals/{{}}]], [[Signals/IDOR]]

---

## 🔁 To Do Later

- [ ] Convert to formal writeup?
- [ ] Pull payloads to `/Payloads & Cheatsheets/`
- [ ] Link all new tools or techniques used