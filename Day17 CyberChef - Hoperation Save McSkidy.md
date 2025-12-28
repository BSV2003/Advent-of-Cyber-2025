# CyberChef - Hoperation Save McSkidy
The story continues, and the elves mount a rescue and will try to breach the Quantum Fortress's defenses and free McSkidy.

## Learning Objectives
- Introduction to encoding/decoding
- Learn how to use CyberChef
- Identify useful information in web applications through HTTP headers

---

# 🔐 Encoding vs Encryption

| Feature | Encoding | Encryption |
|------|--------|-----------|
| Purpose | Compatibility | Security |
| Confidentiality | ❌ No | ✅ Yes |
| Process | Standardised | Algorithm + Key |
| Speed | Fast | Slower |
| Examples | Base64 | TLS |

**Encoding** is meant to safely transfer or store data.  
**Encryption** is meant to protect data from unauthorised access.

🔁 **Decoding** simply converts encoded data back to its original form.

---

# 🧰 What is CyberChef?

CyberChef is a web-based tool that allows analysts to:
- Encode / decode data
- Analyse files
- Transform data formats
- Chain multiple operations into a **recipe**

It is widely used in:
- Malware analysis
- Digital forensics
- Threat hunting
- CTF challenges

## 🧩 CyberChef Interface Overview

| Section | Description |
|------|------------|
| Operations | List of available transformations |
| Recipe | Chain of selected operations |
| Input | Raw data provided by the user |
| Output | Final processed result |

## 🧪 Simple CyberChef Example

1. Open **CyberChef** (online or offline version)
2. Drag **To Base64** into the Recipe
3. **Enter the following input:** IamRoot
4. Add **From Base64** below it
5. Observe how chaining operations returns the original value

💡 Operations can be enabled or disabled using the toggle button.

<img width="960" height="433" alt="image" src="https://github.com/user-attachments/assets/823a8093-7c0f-4910-82d6-79b7b565c718" />

---

# 🌐 Inspecting Web Pages

Not all useful data is visible on a webpage. Browsers allow us to inspect:
- HTML source
- JavaScript
- Network requests
- Hidden values and clues

## How to Open Developer Tools


| Browser | Menu Path |
|------|----------|
| Chrome | More tools → Developer tools |
| Firefox | ☰ → More tools → Web Developer Tools |
| Edge | Settings → More tools → Developer tools |
| Opera | Developer → Developer tools |
| Safari | Develop → Show Web Inspector |

💡 Docking the console to the right improves visibility during analysis.

---

# 🏰 Lock 1 – Outer Gate (Walkthrough)


