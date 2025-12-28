# Forensics - Registry Furensics
Learn what the Windows Registry is and how to investigate it.

## Learning Objectives
- Understand what the Windows Registry is and what it contains.
- Dive deep into Registry Hives and Root Keys.
- Analyze Registry Hives through the built-in Registry Editor tool.
- Learn Registry Forensics and investigate through the Registry Explorer tool.

---

# Windows Registry

The Windows Registry is a hierarchical database that stores:
- System configuration
- Installed applications
- User preferences
- Startup programs
- Security policies
- Hardware and driver details

Unlike a human brain, the registry is **not stored in one place**. Instead, it is split into multiple files called **Registry Hives**.

## 🗂️ Registry Hives

| Hive Name | Contains | Location |
|---------|--------|---------|
| SYSTEM | Services, drivers, hardware, boot config | `C:\Windows\System32\config\SYSTEM` |
| SECURITY | Security policies, audit settings | `C:\Windows\System32\config\SECURITY` |
| SOFTWARE | Installed programs, OS info, autostarts | `C:\Windows\System32\config\SOFTWARE` |
| SAM | User accounts, password hashes, groups | `C:\Windows\System32\config\SAM` |
| NTUSER.DAT | User activity, preferences, Run history | `C:\Users\<username>\NTUSER.DAT` |
| USRCLASS.DAT | Shellbags, Jump Lists | `C:\Users\<username>\AppData\Local\Microsoft\Windows\USRCLASS.DAT` |

> ⚠️ Registry hive files contain **binary data** and cannot be read directly.

## 🧩 Registry Root Keys Mapping

| Hive on Disk | Registry Editor Location |
|-------------|-------------------------|
| SYSTEM | `HKEY_LOCAL_MACHINE\SYSTEM` |
| SECURITY | `HKEY_LOCAL_MACHINE\SECURITY` |
| SOFTWARE | `HKEY_LOCAL_MACHINE\SOFTWARE` |
| SAM | `HKEY_LOCAL_MACHINE\SAM` |
| NTUSER.DAT | `HKEY_USERS\<SID>` / `HKEY_CURRENT_USER` |
| USRCLASS.DAT | `HKEY_USERS\<SID>\Software\Classes` |

> HKEY_CLASSES_ROOT (HKCR) and HKEY_CURRENT_CONFIG (HKCC) are **dynamic** and not backed by standalone hive files.

---

## 🔍 Example Registry Artefacts

## 🔌 USB Devices Connected
**Path:** HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR.
**Contains:**
- USB make & model
- Device IDs
- Connection history

## ▶️ Programs Run via Run Dialog (Win + R)
**Path:** HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
**Shows:**
- Commands typed by the user
- Recently executed programs

## 🕵️ Registry Forensics
Registry forensics involves extracting evidence from registry hives to:
- Reconstruct user activity
- Identify persistence mechanisms
- Track program execution
- Determine system configuration changes

Registry analysis is **never done on the live system** to avoid modifying evidence.

### 🗝️ Important Registry Keys for Investigations

| Registry Key | What It Reveals |
|--------------|----------------|
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | GUI-launched applications |
| `HKCU\...\TypedPaths` | Paths typed in Explorer |
| `HKLM\...\App Paths` | Installed application paths |
| `HKCU\...\WordWheelQuery` | Explorer search history |
| `HKLM\...\Run` | Startup persistence |
| `HKCU\...\RecentDocs` | Recently accessed files |
| `HKLM\...\ComputerName` | Hostname |
| `HKLM\...\Uninstall` | Installed software |

---

# 🛠️ Registry Explorer Tool

Because:
- Registry Editor **cannot open offline hives**
- Many values are stored in unreadable binary form

We use **Registry Explorer**, an open-source forensic tool that:
- Parses offline hives
- Replays transaction logs
- Displays readable values
- Prevents evidence modification

---

# 🧪 Practical Investigation (dispatch-srv01)

## **Step 1: Launch Registry Explorer**
- Open Registry Explorer from the taskbar

<img width="3216" height="1844" alt="image" src="https://github.com/user-attachments/assets/e907d6fd-ad73-470a-af8c-0139fde8e8d8" />


## **Step 2: Load Registry Hives**
1. Click **File → Load Hive**
2. **Navigate to:** `C:\Users\Administrator\Desktop\Registry Hives`

<img width="2964" height="1826" alt="image" src="https://github.com/user-attachments/assets/bfd24ffc-f530-4f80-a1ce-8dfd6a9630d3" />

## **Step 3: Handle Dirty Hives**
To ensure consistency:
- Select a hive (e.g., SYSTEM)
- Hold **SHIFT**
- Click **Open**
- Allow transaction logs to replay

Repeat for all required hives.

<img width="2560" height="1616" alt="image" src="https://github.com/user-attachments/assets/2939a14b-ab86-42e2-b7d5-259f19c90a1a" />

<img width="2564" height="1610" alt="image" src="https://github.com/user-attachments/assets/0182a60c-5158-4d57-b3d1-7953f621a8dc" />

## **Step 4: Investigate Registry Keys**

### 🔎 Find Computer Name
**Path:** ROOT\ControlSet001\Control\ComputerName\ComputerName
**Result:** DISPATCH-SRV01
You can also:
- Use the search bar
- Use predefined bookmarks

<img width="3506" height="938" alt="image" src="https://github.com/user-attachments/assets/40a4d97c-ab0f-4150-b39b-2a47d9ed16f8" />

<img width="2456" height="1068" alt="image" src="https://github.com/user-attachments/assets/2031cdbc-c0cf-42dd-88ee-577becf010e8" />


## 🧠 Investigation Context

- Abnormal activity started on **21 October 2025**
- Registry artefacts help determine:
  - Installed applications
  - Execution paths
  - Startup persistence mechanisms

---

## ✅ Key Takeaways

- The Windows Registry is critical forensic evidence
- Offline analysis is mandatory for integrity
- Registry Explorer enables safe and detailed inspection
- Registry artefacts help reconstruct attack timelines
- Persistence, execution, and user activity leave traces
