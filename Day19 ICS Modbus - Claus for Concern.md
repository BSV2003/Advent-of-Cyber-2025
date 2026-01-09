# ICS/Modbus - Claus for Concern
Learn to identify and exploit weaknesses in ICS systems.

## Learning Objectives
- How SCADA (Supervisory Control and Data Acquisition) systems monitor industrial processes
- What PLCs (Programmable Logic Controllers) do in automation
- How the Modbus protocol enables communication between industrial devices
- How to identify compromised system configurations in industrial systems
- Techniques for safely remediating compromised control systems
- Understanding protection mechanisms and trap logic in ICS environments

---

# SCADA (Supervisory Control and Data Acquisition)

## What is SCADA?

SCADA (Supervisory Control and Data Acquisition) systems are the **command centers** of industrial environments.
They monitor sensors, control machines, log data, and allow humans to interact with physical systems.

**In this scenario**, SCADA manages:
- Drone loading
- Package selection
- Delivery routing
- Inventory verification
- Monitoring via CCTV

## Components of a SCADA System

A typical SCADA system has four main components:

**1. Sensors & Actuators**
- **Sensors** measure real-world data like:
-- Temperature
-- Pressure
-- Weight
-- Position
- **Actuators** perform actions like:
-- Moving motors
-- Opening valves
-- Controlling robotic arms

**Example:** Sensors detect a package on a conveyor belt, and actuators move a robotic arm to load it onto a drone.

**2. PLCs (Programmable Logic Controllers)**
- PLCs are the **brains** of the system
- They:
-- Read sensor data
-- Apply programmed rules
-- Send commands to actuators

**Example logic:**
```java
IF package weight = chocolate egg
AND delivery zone = 5
THEN load Drone 7
```
PLCs run continuously and respond in real time.

**3. Monitoring Systems**
- Visual interfaces for operators, such as:
-- CCTV cameras
-- Dashboards
-- Alarm panels

**TBFC example:** Security cameras on port 80 show live footage of the warehouse floor so operators can visually verify system behavior.

**4. Historians**
- Databases that store historical system data
- Record:
-- Package loading
-- Drone launches
-- Configuration changes
- Used for:
-- Troubleshooting
-- Trend analysis
-- Incident response and attack investigation

## SCADA in the Drone Delivery System

TBFC’s SCADA system controls:

### Package Type Selection

- Decides what item to load:
-- Christmas gifts
-- Chocolate eggs
-- Easter baskets
- Controlled using numeric values that activate specific conveyor belts

### Delivery Zone Routing

- Zones 1–9 → normal delivery areas
- Zone 10 → emergency disposal (ocean)
This makes Zone 10 a **high-risk sabotage target**.

### Visual Monitoring

- CCTV provides real-time feedback
- Operators can:
-- Verify correct items
-- Spot anomalies
-- Confirm recovery actions during incidents

### Inventory Verification

- Checks if requested items exist before loading
- If disabled, the system blindly follows commands—even malicious ones

### System Protection Mechanisms

- Security features that:
-- Monitor changes
-- Prevent unauthorized modifications
- In this case, attackers **weaponized** these protections as traps

### Audit Logging

- Records:
-- Configuration changes
-- Operator actions
-- System events
Attackers often disable logging to **hide their tracks**—which happened here.

## Why SCADA Systems Are Targeted

SCADA systems are attractive to attackers because:
- 🔓 **Legacy software** with known vulnerabilities
- 🔑 **Default credentials** often left unchanged
- ⚙️ Designed for **reliability, not security**
- 🏭 Control **physical processes**, causing real-world damage
- 🌐 Often connected to corporate networks
- 🚨 **Industrial protocols like Modbus lack authentication**
Anyone who can access **Modbus TCP (port 502)** can read or write system values without permission.

## Real-World Threat Example

In **early 2024**, malware called **FrostyGoop** was discovered:
- It directly interacted with industrial systems
- Used Modbus TCP
- Allowed arbitrary register read/write over **port 502**

---

# Answer the questions below

**What port is commonly used by Modbus TCP?**
`502`

---

# PLC & Modbus Protocol

## What is a PLC?

A PLC (Programmable Logic Controller) is a special-purpose industrial computer used to control machines and physical processes. Unlike regular computers, PLCs are built to work reliably in harsh industrial environments.

### Key Characteristics of PLCs
 
 - **Built for harsh environments**
Operate reliably in extreme temperatures, dust, vibration, moisture, and electrical noise.

- **Runs continuously**
Designed to run 24/7 for years without rebooting or crashing.

- **Real-time control**
Responds to sensor inputs within milliseconds to ensure safety and efficiency.

- **Direct hardware control**
Connects directly to sensors (temperature, pressure, weight) and actuators (motors, valves, robotic arms).

In TBFC’s warehouse, PLCs control conveyor belts, robotic arms, and drone-loading mechanisms.

## What is Modbus?

Modbus is a communication protocol that allows industrial devices to talk to each other. It was created in 1979 and is still widely used today because of its simplicity and reliability.

### How Modbus Works

Modbus follows a request–response model:
Client: _"What is the value of register 0?"_
PLC: _"Register 0 = 1"_

### Why Modbus Is Insecure

Modbus was designed for trusted environments and does not include security features:
- No authentication
- No encryption
- No authorization
- No integrity verification

Anyone who can reach the Modbus port can read or modify values.

## Modbus Data Types

Modbus organizes data into four main types:
| Type              | Purpose                  | Values  | Example Uses                     |
| ----------------- | ------------------------ | ------- | -------------------------------- |
| Coils             | Digital outputs          | 0 or 1  | Motor ON/OFF, alarm status       |
| Discrete Inputs   | Digital inputs           | 0 or 1  | Button pressed, sensor triggered |
| Holding Registers | Writable numeric values  | 0–65535 | Speed, temperature, zone number  |
| Input Registers   | Read-only numeric values | 0–65535 | Sensor readings                  |

⚠️ **Important:**
**Coils & Holding Registers** → writable
**Discrete Inputs & Input Registers** → read-only

## TBFC Drone Control System – Key Modbus Values

**Holding Registers**
- **HR0** – Package Type
-- `0` = Christmas Gifts
-- `1` = Chocolate Eggs
-- `2` = Easter Baskets
- **HR1** – Delivery Zone
-- `1–9` = Normal zones
-- `10` = Ocean dump zone
- **HR4** – System Signature
-- Used to identify system version or attacker signature

**Coils**
- **C10** – Inventory Verification
- **C11** – Protection / Override
- **C12** – Emergency Dump
- **C13** – Audit Logging
- **C14** – Christmas Restored Flag
- **C15** – Self-Destruct Status

## Modbus Addressing

- Modbus addresses start at **0**, not 1.
- Example:
-- Register 0 → first register
-- Coil 10 → 11th coil

### Example Addresses

- HR0 → Package Type
- HR1 → Delivery Zone
- HR4 → System Signature
- C10 → Inventory Verification
- C11 → Protection Flag
- C15 → Self-Destruct Flag

## Modbus TCP vs Serial Modbus

**TCP:** Transmission Control Protocol (TCP) is a connection-oriented protocol requiring a TCP three-way-handshake to establish a connection. TCP provides reliable data transfer, flow control and congestion control. Higher-level protocols such as HTTP, POP3, IMAP and SMTP use TCP.

**Serial Modbus (Legacy)**
- Uses RS-232 / RS-485 cables
- Requires physical access
- Naturally isolated

**Modbus TCP (Modern)**
- Runs over TCP/IP
- Listens on **port 502**
- Accessible over networks
- Vulnerable to remote attacks

### The Security Problem

Modbus TCP exposes industrial systems because it has:
- No identity verification
- No encryption (plaintext traffic)
- No permission controls
- No protection against command tampering

Security must be added externally using:
- Firewalls
- VPNs
- Network segmentation
- Modbus security gateways

## Connecting the Dots

King Malhare bypassed the web interface entirely and:
- Connected directly to **Modbus TCP (port 502)**
- Read and modified registers and coils
- Disabled logging and verification
- Set traps to prevent remediation

The crumpled maintenance note documented these exact Modbus addresses and warned about the trap logic.

---

# Practical

**Investigation Steps**
1. **Service discovery** using Nmap
2. **Visual confirmation** via CCTV feed
3. **Direct Modbus** access using PyModbus
4. Read registers and coils
5. Identify trap mechanism

## Safe Remediation Order

1. Disable protection mechanism (C11 = False)
2. Restore package type (HR0 = 0)
3. Enable inventory verification (C10 = True)
4. Enable audit logging (C13 = True)
5. Verify self-destruct never armed (C15 = False)

Order matters — incorrect steps trigger system failure.

### Tools Used

- `nmap`
- `python3`
- `pymodbus`
- Browser (CCTV monitoring)

---

# Key Takeaways

- ICS protocols assume trusted environments
- Modbus is highly insecure if exposed
- Small logic changes can cause large physical impact
- Understanding system behavior is critical before remediation
- Order of operations matters in industrial systems
