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

## Step-by-Step Breakdown

1️⃣ **Identify the Guard Name**
- Found in the login form
- Encode the name using **Base64**
- Use this as the **username**

2️⃣ **Find the Magic Question**
- Inspect **Network → Headers**
- **Example:** What is the password for this level?
- Encode the question using **Base64**

3️⃣ **Use Chat (Bunnygram)**
- Chat messages are Base64 encoded
- Send the encoded question
- Guard responds with an encoded password

4️⃣ **Inspect Login Logic**
- Go to **Debugger**
- Discover password logic → Base64 encoded

5️⃣ **Decode the Password**
- Decode guard’s response in CyberChef
- This gives the **plaintext password**

6️⃣ **Login**
- Username → Base64 encoded guard name
- Password → Decoded plaintext password

✅ **First lock successfully bypassed**

## 🧠 Key Takeaways
- Encoding ≠ security
- Client-side logic can reveal authentication weaknesses
- Base64 is commonly abused to “hide” secrets
- Developer tools are critical for web security analysis
- CyberChef simplifies complex transformations

---

# 🧱 Lock 2 – Outer Wall

### Steps:
1. Reuse encoded guard name as username
2. **Extract new magic question:** Did you change the password?
3. Encode question → send via chat
4. Inspect login logic:
- Password is **double Base64 encoded**
5. Decode the response **twice**
6. Login successfully

✅ **Encoding used:** Base64 × 2

## 🔍 Tools Used
- **CyberChef (online/offline)**
- **Browser Developer Tools**
- Elements
- Network tab
- Debugger tab

## 🧠 Key Takeaways
- Encoding is not encryption
- Multiple encoding layers increase complexity but not security
- CyberChef simplifies complex transformations
- Client-side logic can reveal authentication flaws
- Inspecting headers and JavaScript is critical in web challenges

---

# 🔐 Lock 3 – Guard House

## New Concept: XOR

From this lock onward:
- No magic question
- Ask guard directly (e.g., *“Password please”*)
- Guard responses may be slow (2–3 minutes)

### XOR Basics
- XOR uses a **key**
- XOR is **reversible**
- Applying XOR twice with the same key restores original data

### Given
- XOR Key: `cyberchef`

### Login Logic
1. Password is:
   - XOR’ed with key
   - Then encoded to Base64

### Reverse Recipe (CyberChef)
1. **From Base64**
2. **XOR** with key `cyberchef`

### Result
- Plaintext password → Login successful

## 🧠 Key Takeaways

- CyberChef supports **chained operations**
- Always reverse transformations **in reverse order**
- XOR is symmetric (same operation encrypts & decrypts)
- Developer tools are essential for:
  - Logic discovery
  - Hidden hints
  - Authentication flow analysis
