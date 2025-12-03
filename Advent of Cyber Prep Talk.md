# Advent of Cyber 2025 – Prep Talk

The Prep Track introduces foundational defensive skills across Linux, Windows, file analysis, breach checking, app permissions, and log review.

---

## 1. Password Pandemonium

**Goal:**  
Create a password that passes all system checks and isn't found in leaked password databases.

**Steps:**
- Use at least **12 characters**
- Include **uppercase**, **lowercase**, **numbers**, and **symbols**
- Ensure the password does **not** appear in breach databases

**Why it matters:**  
Strong passwords protect against brute-force attacks and credential stuffing.

---

## 2. The Suspicious Chocolate.exe

**Scenario:**  
A USB labelled *“SOCMAS Party Playlist”* contains a file named `chocolate.exe`. You must scan it using a simulated VirusTotal tool.

**Goal:**  
Determine if `chocolate.exe` is safe or infected.

**Approach:**
1. Upload/scan the file using a malware analysis tool  
2. Review detection engines and behaviors  
3. Classify the file as safe or malicious  

**Why it matters:**  
File reputation and malware scanning are essential defender skills.

---

## 3. Welcome to the AttackBox

The AttackBox is a secure virtual environment designed for hands-on cybersecurity training.

**Goal:**  
Find and read the hidden welcome message inside the AttackBox.

**Steps:**
- `ls` — list files  
- `cd challenges/` — move into the challenges directory  
- `cat welcome.txt` — read the file  

**Why it matters:**  
Linux navigation is foundational for cybersecurity operations.

---

## 4. The CMD Conundrum

Windows CMD provides visibility into areas a GUI may hide. Command-line fluency is key in incident response.

**Goal:**  
Use Windows Command Prompt to find a hidden flag file.

**Steps:**
- `dir` — list visible files  
- `dir /a` — show hidden files  
- `type hidden_flag.txt` — read the file  

**Other Useful Commands:**
- `cd <path>` — change directory  
- `type <filename>` — print file contents  
- `cls` — clear terminal  

**Why it matters:**  
Windows forensic work often relies on command-line visibility.

---

## 5. Linux Lore

Linux powers most backend servers, making comfort with its filesystem essential.

**Goal:**  
Find McSkidy’s hidden message in his Linux home directory.

**Steps:**
- `cd /home/mcskidy`  
- `ls -la` — show all files  
- `cat .secret_message`  

**Other Useful Commands:**
- `pwd` — show current directory  
- `ls`, `ls -a` — list files / show hidden items  
- `cd <path>` — move directories  
- `clear` — clear the terminal  

**Why it matters:**  
Understanding hidden files and directory structure is critical during investigations.

---

## 6. The Leak in the List

Tools like *Have I Been Pwned* help detect compromised accounts.

**Goal:**  
Check whether McSkidy’s email has been leaked.

**Steps:**
1. Enter `mcskidy@tbfc.com` into the breach checker  
2. Review results  
3. Identify the domain marked **“Compromised”**  

**Why it matters:**  
Breach intelligence prevents credential-reuse attacks and account takeover.

---

## 7. WiFi Woes in Wamville

Routers using default credentials are highly vulnerable.

**Goal:**  
Secure a router that still uses its default login.

**Steps:**
1. Log in using `admin / admin`  
2. Open **Security Settings**  
3. Set a strong, unique password  

**Why it matters:**  
Default credentials are a common entry point for attackers.

---

## 8. The App Trap

Misconfigured or malicious third-party apps can leak sensitive data.

**Goal:**  
Identify and revoke a malicious connected app.

**Steps:**
1. Review the connected apps list  
2. Identify suspicious permissions (e.g., password vault access)  
3. Revoke the app  

**Why it matters:**  
OAuth abuse and insecure integrations are common attack vectors.

---

## 9. The Chatbot Confession

AI systems may unintentionally reveal sensitive information if not monitored.

**Goal:**  
Identify chatbot messages that expose sensitive data.

**Steps:**
1. Read all chatbot responses  
2. Flag messages containing private or internal details  
3. Submit findings  

**Why it matters:**  
AI-driven data leaks are a modern security concern.

---

## 10. The Bunny’s Browser Trail

User-Agent strings can reveal suspicious or automated web activity.

**Goal:**  
Find a suspicious User-Agent in HTTP logs.

**Steps:**
1. Read the log entries  
2. Compare User-Agents to common browsers (Chrome, Firefox, Edge)  
3. Identify the abnormal one  

**Why it matters:**  
Anomalous User-Agents often indicate bots, scanners, or automated attack tools.

---

## Summary

The Prep Track reinforces key foundational skills:

- Linux & Windows command-line fundamentals  
- Password security best practices  
- Malware and file reputation analysis  
- Breach detection awareness  
- Router/WiFi security hygiene  
- App permission auditing  
- Sensitive-data identification  
- Log and User-Agent anomaly analysis  

These skills prepare you for the full **Advent of Cyber 2025** challenge series.
