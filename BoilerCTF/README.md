# 🚩 BoilerCTF Writeup

## 🏴 Overview
- **Platform:** TryHackMe
- **Difficulty:** Medium
- **Tools Used:** Nmap, Gobuster, FTP, SSH, Metasploit, Exploit-DB
- **Techniques:** Enumeration, Exploitation, Privilege Escalation
- **Relevant CVEs:** N/A for Webmin 1.930

---

## 🔍 Enumeration

### Nmap Scan
```bash
nmap -sV -sC -A -T5 -oN firstScan.txt 10.10.248.204
```
#### Results:
- **Port 21 (FTP)**: Anonymous login enabled
- **Port 80 (HTTP)**: Apache2 Default Page
- **Port 10000 (MiniServ Webmin 1.930)**
- **Port 55007 (SSH)**

### Gobuster Scan
```bash
gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.248.204 dir
```
#### Results:
- **/joomla/**
- **/_files/** (Suspicious directory)

---

## 🎯 Exploitation

### FTP Anonymous Login
- Discovered hidden file: `.info.txt`

### Web Enumeration
- Found **sar2html** in **/_files/**
- Exploited using Python RCE script from **Exploit-DB**

### Credential Discovery
- `log.txt` contained: `user: basterd | pass: supersuperp@$$`
- SSH login successful on port **55007**

---

## 🔝 Privilege Escalation

### User Escalation
- Discovered `.secret` file with user credentials

### Root Privilege Escalation
- Used `find` binary with **SUID** bit enabled:
```bash
find . -exec cat /root/root.txt \;
```

---

## 🛠️ Techniques Used (MITRE ATT&CK)
- **T1046**: Network Service Scanning (Nmap, Gobuster)
- **T1078**: Valid Accounts (FTP Anonymous Login)
- **T1059.004**: Command and Scripting Interpreter: Unix Shell
- **T1087.001**: Local Account Discovery
- **T1069.001**: Permission Groups Discovery
- **T1548.001**: SUID Privilege Escalation

---

## 🔚 Conclusion
This was a **fun and challenging CTF**, requiring thorough **enumeration, directory brute-forcing, and privilege escalation techniques**. The **Webmin** service turned out to be a red herring, but the **sar2html RCE** provided a way in. The **use of hidden credentials and misconfigured permissions** allowed for a smooth escalation to root.

✅ **Key Takeaways:**
- Always check **robots.txt** and hidden files
- Use **gobuster** to find directories
- **SUID binaries** can be leveraged for privilege escalation

🚀 **On to the next challenge!**
