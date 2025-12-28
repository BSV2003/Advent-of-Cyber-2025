# Web Attack Forensics - Drone Alone
Explore web attack forensics using Splunk.

## Learning Objectives
- Detect and analyze malicious web activity through Apache access and error logs
- Investigate OS-level attacker actions using Sysmon data
- Identify and decode suspicious or obfuscated attacker payloads
- Reconstruct the full attack chain using Splunk for Blue Team investigation

**HTTP:** Hypertext Transfer Protocol (HTTP) is the protocol that specifies how a web browser and a web server communicate. Your web browser requests content from the TryHackMe web server using the HTTP protocol as you go through this room.
**Splunk:** Splunk is a platform for collecting, storing, and analysing machine data. It provides various tools for analysing data, including search, correlation, and visualisation. It is a powerful SIEM tool that organisations of all sizes can use to improve their IT operations and security posture.
**Apache:** Apache is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free.
**Sysmon:** Sysmon refers to System Monitor, which is a Windows system service and device driver developed by Microsoft that is designed to monitor and log various events happening within a Windows system.
**Blue Team:** A blue team comprises cyber security and technology professionals whose aim is to protect an information system from impending cyber threats by performing and implementing defensive actions.

In this lab, we'll be using Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

---

# Environment Setup
- Platform: Splunk
- Log Sources:
  - `windows_apache_access`
  - `windows_apache_error`
  - `windows_sysmon`

Make sure the **time range** is set to **All time** or **Last 7 days**, otherwise logs may not appear.

---

# Investigation Workflow

## 1. Detect Suspicious Web Commands (Apache Access Logs)

Attackers often attempt **command injection** via web requests.  
We search for suspicious keywords such as `cmd.exe` and `powershell`.

```spl
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status
```

<img width="2920" height="626" alt="image" src="https://github.com/user-attachments/assets/049112ed-875e-4063-a100-d85d6be7cec1" />

**What this shows:**
- Malicious HTTP requests attempting to execute system commands
- Signs of command injection via CGI scripts (e.g., hello.bat)

## 2. Decode Base64-Encoded Payloads

Attackers commonly encode payloads to hide intent.

**Example Base64 string:**
VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA

<img width="1070" height="831" alt="image" src="https://github.com/user-attachments/assets/111eecf4-153d-4ca4-b78c-4d2a19043723" />

Decoded result reveals attacker intent (e.g., test payloads or messages).

**Why this matters:**
Encoded PowerShell commands are a strong indicator of malicious activity.

**PowerShell:** PowerShell is a task automation and configuration management program from Microsoft, consisting of a command-line shell and the associated scripting language.

## 3. Analyse Apache Error Logs

Errors can indicate backend execution attempts.

```spl
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
```

<img width="2924" height="908" alt="image" src="https://github.com/user-attachments/assets/768b40b3-6a4c-4a6b-b780-a8f8a8e198f4" />

**Key insight:**
- A 500 Internal Server Error often means attacker input reached backend execution
- Confirms the attack progressed beyond simple probing

## 4. Trace Suspicious Process Creation (Sysmon)

We check if Apache spawned system processes.

```spl
index=windows_sysmon ParentImage="*httpd.exe"
```

**Red flag example:**

```ini
ParentImage = C:\Apache24\bin\httpd.exe
Image       = C:\Windows\System32\cmd.exe
```

<img width="2928" height="1184" alt="image" src="https://github.com/user-attachments/assets/120d6251-bfbc-4bab-9f52-476b61d6bd17" />

**Why this is critical:**
Apache should never launch cmd.exe or powershell.exe
→ Strong evidence of successful command injection

## 5. Confirm Attacker Reconnaissance

Attackers often check their privileges using `whoami`.

```spl
index=windows_sysmon *cmd.exe* *whoami*
```

<img width="2922" height="1288" alt="image" src="https://github.com/user-attachments/assets/1933e717-3f51-493a-a7f2-c661c9fe6c56" />

**Meaning:**
- Confirms post-exploitation activity
- Shows attacker verified execution context on the server

## Identify Base64-Encoded PowerShell Payloads

We search for encoded PowerShell commands.

```spl
index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
```

**Outcome:**
- No results → encoded payload did not execute
- Results present → decode to inspect attacker actions
