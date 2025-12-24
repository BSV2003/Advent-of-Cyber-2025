# Passwords - A Cracking Christmas
Learn how to crack password-based encrypted files.

## Learning Objectives
- How password-based encryption protects files such as PDFs and ZIP archives.
- Why weak passwords make encrypted files vulnerable.
- How attackers use dictionary and brute-force attacks to recover passwords.
- A hands-on exercise: cracking the password of an encrypted file to reveal its contents.
- The importance of using strong, complex passwords to defend against these attacks.

- The strength of protection depends almost entirely on the password. Short or common passwords can be guessed; long, random passwords are far harder to break.
- Different file formats use different algorithms and key derivation methods. For example, PDF encryption and ZIP encryption differ in details (how the key is derived, salt use, number of hash iterations). That affects how easy or hard cracking is.
- Many consumer tools still support legacy or weak modes (particularly older ZIP encryption). That makes some encrypted archives much easier to attack than modern, well-implemented schemes.
- Encryption protects data confidentiality only. It does not prevent someone with access to the encrypted file from trying to guess the password offline.

To make it simple, encryption makes the contents unreadable unless the correct password is known. If the password is weak, an attacker can simply try likely passwords until one works.

---

# How Attackers Recover Weak Passwords

Attackers don't usually try to "break" the encryption itself because that would take far too long with modern cryptography. Instead, they focus on guessing the _password_ that protects the file. The two most common ways of doing this are **dictionary attacks** and **brute-force (or mask) attacks**.

## Dictionary Attacks

In a dictionary attack, the attacker uses a predefined list of potential passwords, known as a _wordlist_, and tests each one until the correct password is found. These wordlists often contain leaked passwords from previous breaches, common substitutions like password123, predictable combinations of names and dates, and other patterns that people frequently use. Because many users choose weak or common passwords, dictionary attacks are usually fast and highly effective.

## Mask Attacks

Brute-force and mask attacks go one step further. A brute-force attack systematically tries every possible combination of characters until it finds the right one. While this guarantees success eventually, the time it takes grows exponentially with the length and complexity of the password. Mask attacks aim to reduce that time by limiting guesses to a specific format. 

By narrowing the search space, mask attacks strike a balance between speed and thoroughness, especially when the attacker has some idea of how the password might be structured.

Practical tips attackers use (and defenders should know about):
- Start with a wordlist (fast wins). **Common lists:** rockyou.txt, common-passwords.txt.
- If the wordlist fails, move to targeted wordlists (company names, project names, or data from the target).
- If that fails, try mask or incremental attacks on short passwords (e.g. ?l?l?l?d?d = _three lowercase letters + two digits_, which is used as a password mask format by password cracking tools).
- Use GPU-accelerated cracking when possible; it dramatically speeds up attacks for some algorithms.
- Keep an eye on resource use: cracking is CPU/GPU intensive. That behaviour can be detected on a monitored endpoint.

---

1. Confirm the File Type
2. Tools to Use (pick one based on file type)
3. Try a Dictionary Attack First (fast, often successful)

If it's a PDF, proceed with _PDF tools_. If it's a ZIP, proceed with _ZIP tools_.
- **PDF:** pdfcrack, john (via pdf2john)
- **ZIP:** fcrackzip, john (via zip2john)
- **General:** john (very flexible) and hashcat (GPU acceleration, more advanced)

---

# Detection of Indicators and Telemetry

Offline password cracking does not interact with login systems.
Because of this, there are no failed login attempts, no account lockouts, and nothing obvious in authentication logs.

Instead, detection must focus on where the cracking happens — on endpoints, servers, or jump boxes.

## Key Indicators to Monitor

1. Process Creation

Password cracking tools use a small and well-known set of programs.
We can detect them by monitoring:
- Process execution events
- Command-line arguments
- File access patterns
- GPU usage
- Light network activity

Common cracking tools and helpers include:
- john
- hashcat
- fcrackzip
- pdfcrack
- zip2john
- pdf2john.pl
- 7z, 7za, unzip
- qpdf
- perl (used to run pdf2john.pl)

2. Command-Line Indicators
Cracking tools often use recognizable command flags and files, such as:
- --wordlist, -w
- --rules
- --mask
- -a 3, -m (Hashcat attack modes)
- References to:
-- rockyou.txt
-- SecLists
-- zip2john
-- pdf2john

These patterns are strong indicators of cracking activity.

3. Potfiles and State Files
Cracking tools store results and progress locally. Watch for:
- ~/.john/john.pot
- .hashcat/hashcat.potfile
- john.rec

Their presence usually means active or previous cracking attempts.

## Platform-Specific Telemetry

**Windows**
- Sysmon Event ID 1 captures:
-- Process name
-- Full command line
- Ideal for detecting cracking tools and parameters

**Linux**
- Captured using:
-- auditd
-- execve
-- EDR sensors
- Logs executed binaries and arguments

## GPU and Resource Indicators
GPU-based cracking is very noisy and easy to spot:
- Sudden, sustained high GPU usage
- Increased power draw
- Loud fan activity
- _nvidia-smi_ showing long-running processes like hashcat or john

Loaded GPU-related libraries:
- nvcuda.dll
- OpenCL.dll
- libcuda.so
- amdocl64.dll

## Network Indicators (Light but Useful)

Offline cracking does not require the network, but attackers often download tools first:
- Downloads of large wordlists (e.g., rockyou.txt)
- Git clones of wordlist repositories
- Package installations:
-- _apt install john hashcat_
- GPU driver or runtime downloads

## Unusual File Access

Repeated reads of:
- Wordlists
- Encrypted files
- Archive files

This behavior should be reviewed closely.

---

# Example Detection Rules

## Sysmon (Windows)

ProcessName="C:\Program Files\john\john.exe" OR
ProcessName="C:\Tools\hashcat\hashcat.exe" OR
CommandLine="*pdf2john.pl*" OR
CommandLine="*zip2john*"

## Linux auditd (temporary investigation rules)

auditctl -w /usr/share/wordlists/rockyou.txt -p r -k wordlists_read
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/john -k crack_exec
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/hashcat -k crack_exec

## Sigma Rule (Windows – Process Creation)

title: Password Cracking Tools Execution
status: experimental
logsource:
  product: windows
  category: process_creation
detection:
  selection_name:
    Image|endswith:
      - '\john.exe'
      - '\hashcat.exe'
      - '\fcrackzip.exe'
      - '\pdfcrack.exe'
      - '\7z.exe'
      - '\qpdf.exe'
  selection_cmd:
    CommandLine|contains:
      - '--wordlist'
      - 'rockyou.txt'
      - 'zip2john'
      - 'pdf2john'
      - '--mask'
      - ' -a 3'
  condition: selection_name or selection_cmd
level: medium

## Response Playbook (What to Do)

When cracking activity is detected:
1. **Isolate the host**
- If it’s a lab system, tag and suppress alerts
2. **Collect evidence**
- Process list
- Memory dump
- nvidia-smi output
- Open files
- Encrypted samples
3. **Preserve artifacts**
- Working directory
- Wordlists
- Hash files
- Shell history
4. **Assess impact**
- Identify decrypted files
- Look for lateral movement or data exfiltration
5. **Determine intent**
- Was this authorized or malicious?
- Escalate if needed
6. **Remediate**
- Rotate passwords and keys
- Enforce MFA
7. **Prevent recurrence**
- Educate users
- Restrict cracking tools to approved sandboxes

