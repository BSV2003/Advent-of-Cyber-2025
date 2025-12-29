# Obfuscation - The Egg Shell File
McSkidy keeps her focus on a particular alert that caught her interest: an email posing as northpole-hr.

## Learning Objectives
- Learn about obfuscation, why and where it is used.
- Learn the difference between encoding, encryption, and obfuscation.
- Learn about obfuscation and the common techniques.
- Use CyberChef to recover plaintext safely.

**PowerShell:** PowerShell is a task automation and configuration management program from Microsoft, consisting of a command-line shell and the associated scripting language.

Malicious actors often hide code and data using a technique called **obfuscation**.

---

# 🔐 What Is Obfuscation?

**Obfuscation** is the practice of making data difficult to read or analyse.  
Attackers use it to:
- Bypass basic security tools  
- Delay forensic investigations  
- Hide malicious commands, URLs, or payloads  

Instead of storing readable text, attackers transform it into unreadable or misleading formats.

---

# 🔁 Simple Obfuscation Techniques

## ROT Ciphers

ROT (Rotate) ciphers shift letters forward in the alphabet.

### ROT1
- Shifts each letter by **1**
- **Example:** carrot coins go brr → dbsspu dpjot hp css

### ROT13
- Shifts each letter by **13**
- Common words become recognisable patterns  
- `the` → `gur`  
- `and` → `naq`

## 🔑 XOR Obfuscation

XOR is a byte-level operation that uses a **key**.

- Each byte is XOR’ed with the key
- Produces unreadable characters
- **Reversible**: XOR the output again with the same key to get the original text

**Example (via CyberChef):**
- **Input:** carrot supremacy
- **Key:** a (HEX)
- **Output:** ikxxe~*yzxogkis!

---

# 🧠 Detecting Obfuscation Patterns

| Technique | Visual Clue |
|----------|-------------|
| ROT1 | Looks one letter off |
| ROT13 | Familiar words slightly scrambled |
| Base64 | Alphanumeric strings, often end with `=` or `==` |
| XOR | Random symbols, same length as input |

Once identified, apply the **reverse operation** for deobfuscation.

---

## 🧪 Using CyberChef

CyberChef is known as the **Cyber Swiss Army Knife**.

### Key Components
- **Operations**: Available transformations (Base64, XOR, ROT, etc.)
- **Recipe**: Chain of operations
- **Input**: Obfuscated data
- **Output**: Decoded result

### Helpful Operations
- `From Base64`
- `ROT13`
- `XOR`
- `Magic` (auto-detection)
- `Gunzip`

---

# ✨ Magic Operation

When the obfuscation method is unknown:
- Use **Magic**
- Automatically tests common decoders
- Enable **Intensive Mode** for deeper analysis

⚠️ Magic is a helper, not a replacement for analysis.

---

# 🧅 Layered Obfuscation

Attackers often combine techniques.

## Example:
1. Gzip compression  
2. XOR with key  
3. Base64 encoding  

## How to Reverse:
Apply operations in **reverse order**: From Base64 → XOR → Gunzip

**Gunzip:** Decompresses data which has been compressed using the deflate algorithm with _gzip_ headers.

---

**API:** API, which stands for _Application Programming Interface_, is a set of rules and protocols for building software and applications. An API allows different software programs to communicate with each other. It defines methods of communication between various components, including the kinds of requests that can be made, how they're made, the data formats that should be used, and conventions to follow.
> An API is like a messenger between two programs. It lets different software talk to each other by telling them how to ask for data and how to receive it.

**XOR:** Is a binary operation that is commonly used for encryption and decryption of data. XOR operates on binary data (bits) and is based on the principles of Boolean algebra. The operation involves two bits. The result of the operation is "1" if the two bits are different, and "0" if they are the same.
