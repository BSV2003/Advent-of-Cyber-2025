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



