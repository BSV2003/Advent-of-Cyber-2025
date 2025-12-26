# YARA Rules - YARA mean one!
Learn how YARA rules can be used to detect anomalies.

## Learning Objectives
- Understand the basic concept of YARA.
- Learn when and why we need to use YARA rules.
- Explore different types of YARA rules.
- Learn how to write YARA rules.
- Practically detect malicious indicators using YARA,

---

**Blue Team:** A blue team comprises cyber security and technology professionals whose aim is to protect an information system from impending cyber threats by performing and implementing defensive actions.

YARA is a powerful detection tool used by defenders to identify and classify malware based on patterns found in files, memory, or processes.  
Instead of relying only on file names or hashes, YARA searches for **digital fingerprints** left behind by attackers.

---

# Why YARA Matters and When to Use It

Cyber defenders deal with a huge number of alerts, strange files, and unusual activity every day. Not all threats are obvious—some malware hides inside files that look normal, like documents or scripts. This is where YARA becomes useful.

YARA helps defenders find malware by looking for patterns and behaviour, not just by checking file names. Instead of waiting for antivirus updates, defenders can write their own rules to decide what looks suspicious. They can also reuse and improve rules shared by other security teams, making detection faster and stronger.

Because of this, YARA helps defenders:
- Detect threats more quickly
- Hunt for hidden malware
- Reduce the chances of attacks going unnoticed

### When Do Defenders Use YARA?

Defenders commonly use YARA in situations like:
- **After an incident:** To check if malware found on one system also exists on other systems.
- **Threat hunting:** To actively search systems and endpoints for known malware families or related variants.
- **Using shared intelligence:** To apply YARA rules created by other security researchers to detect new threats.
- **Memory analysis:** To scan memory dumps and running processes for malicious code that isn’t saved as a file.

In short, **YARA helps defenders turn scattered clues into clear answers**, making it easier to spot and stop attackers before they cause more damage.

---

# YARA Values

In cyber security, speed and accuracy are very important. YARA helps defenders quickly make sense of a messy situation.

Instead of waiting for antivirus updates or alerts from other tools, YARA lets defenders **write their own rules** to detect suspicious files and malware patterns. This means they can spot new or modified threats early, before they spread.

YARA is valuable because it:
- **Works fast** – It can scan many files or systems quickly.
- **Is flexible** – It can look for simple text, binary patterns, or complex conditions.
- **Gives control** – Analysts decide what counts as malicious, not just the tool.
- **Can be shared** – Rules can be reused, improved, and shared with other defenders.
- **Improves visibility** – It helps connect small clues into a clear picture of an attack.

In short, YARA helps defenders move from just watching alerts to **actively hunting threats**, allowing them to respond faster and more confidently before attackers cause damage.

---

# YARA Rules

A **YARA rule** defines *what to look for* and *when to raise an alert*.  
Each rule is built using three main components:

## 1. Metadata

**Metadata** provides information about the rule itself.  
It helps analysts understand the purpose and origin of the rule.

Typical metadata fields include:
- Author of the rule  
- Date created or updated  
- Description of what the rule detects  

> Metadata is optional, but strongly recommended for rule management and collaboration.

## 2. Strings

**Strings** are the actual clues that YARA searches for in files, memory, or processes.

They can include:
- **Text strings** (keywords, commands, URLs)
- **Hexadecimal byte patterns**
- **Regular expressions (regex)**

These strings represent indicators of suspicious or malicious behavior.

## 3. Conditions

The **condition** section defines the logic that decides **when the rule should trigger**.

Conditions can specify:
- If **any** string matches
- If **all** strings must match
- Logical combinations using `and`, `or`, and `not`
- File properties such as size or entry point

This logic ensures YARA only flags content that truly matches the intended threat pattern.

---

A YARA rule works by:
- **Describing the threat** (Metadata)
- **Identifying suspicious indicators** (Strings)
- **Deciding when to alert** (Conditions)

Together, these components allow defenders to precisely detect malware and suspicious activity across systems.

---

# Strings

Strings are the indicators (clues) that YARA searches for when scanning files, memory, or other data sources.
They represent fragments of text, bytes, or patterns that can reveal malicious activity.

YARA supports three main types of strings:
1. Text strings
2. Hexadecimal strings
3. Regular expression (regex) strings

## Text Strings

Text strings are the most common and simplest type of YARA strings.
They match words or short text fragments found in files, scripts, or memory.

By default:
- ASCII
- Case-sensitive

**Example: Simple Text String Rule**
rule TBFC_KingMalhare_Trace
{
    strings:
        $TBFC_string = "Christmas"

    condition:
        $TBFC_string 
}

### Text String Modifiers

Attackers often try to hide strings using encoding or obfuscation.
YARA provides **modifiers** to counter this.

- **Case-Insensitive (`nocase`)**
Matches text regardless of letter casing.
``` yara
strings:
    $xmas = "Christmas" nocase
```

- **Unicode / Wide Characters (`wide`, `ascii`)**
Useful for Windows binaries using Unicode (2-byte characters).
```yara
strings:
    $xmas = "Christmas" wide ascii
```

- **XOR-Encoded Strings (`xor`)**
Automatically tests all single-byte XOR variations.
```yara
strings:
    $hidden = "Malhare" xor
```

- Base64-Encoded Strings (`base64`, `base64wide`)
Detects strings hidden using Base64 encoding.
```yara
strings:
    $b64 = "SOC-mas" base64
```

These modifiers make rules more resilient against obfuscation.

## Hexadecimal Strings

Hex strings match raw byte patterns, useful when malware doesn’t contain readable text.

Common use cases:
- File headers
- Shellcode
- Binary signatures

**Example: Hex String Rule**
```yara
rule TBFC_Malhare_HexDetect
{
    strings:
        $mz = { 4D 5A 90 00 }   // MZ header of a Windows executable
        $hex_string = { E3 41 ?? C8 G? VB }

    condition:
        $mz and $hex_string
}
```

## Regular Expression (Regex) Strings

Regex strings match patterns, not exact values.
They are useful when malware changes URLs, filenames, or commands slightly.

**Example: Regex Rule**
```yara
rule TBFC_Malhare_RegexDetect
{
    strings:
        $url = /http:\/\/.*malhare.*/ nocase
        $cmd = /powershell.*-enc\s+[A-Za-z0-9+/=]+/ nocase

    condition:
        $url and $cmd
}
```

⚠️ Regex is powerful but should be used carefully, as overly broad patterns can slow scans.

---

# Conditions

The **condition** section defines **when a rule triggers**.
It combines strings and logic into a final decision.

## Match a Single String
This is the simplest condition.  
The rule triggers if **one specific string** is found in the scanned file or memory.
```yara
condition:
    $xmas
```
Triggers if $xmas is found.

## Match Any String
When multiple strings are defined, the rule can be configured to trigger as soon as **any one** of them is found.  
This approach is useful for detecting early signs of compromise, where even a single indicator may be suspicious.
```yara
condition:
    any of them
```
Triggers if **at least one** string matches.

## Match All Strings
To make the rule stricter, you can require that **all defined strings** appear together.
This reduces false positives, as YARA will only flag a file when every indicator is present.
```yara
condition:
    all of them
```
Triggers **only if every string matches**.

## Logical Operators (`and`, `or`, `not`)
Logical operators let you combine multiple checks into a single condition.
```yara
condition:
    ($s1 or $s2) and not $benign
```
This condition triggers when either $s1 or $s2 is found, but excludes matches that contain a known benign pattern ($benign).
This helps reduce false positives while still catching suspicious activity.

## File Property Checks
YARA can also check file properties, not just file contents.
This allows rules to evaluate attributes such as file size, entry point, or hashes.
```yara
condition:
    any of them and (filesize < 700KB)
```
This is useful for detecting lightweight loaders or droppers, which are often small in size.

---

# Useful YARA Command Flags

- **-r** → Recursively scans directories and follows symlinks
- **-s** → Displays the strings that matched inside detected files
