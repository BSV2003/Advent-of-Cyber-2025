# SOC Alert Triaging - Tinsel Triage
Investigate and triage alerts through Microsoft Sentinel.

## Learning Objectives
- Understand the importance of alert triage and prioritisation
- Explore Microsoft Sentinel to review and analyse alerts
- Correlate logs to identify real activities and determine alert verdicts

---

## What Is Alert Triaging?

Alert triaging is the process of **quickly reviewing and prioritising security alerts** to determine:
- Which alerts require immediate action
- Which can be investigated later
- Which are false positives or low-risk noise

When alerts flood in, treating every alert equally wastes time and resources.  
Triaging helps SOC analysts focus on **what truly matters**.

---

## Why Alert Triaging Is Important

- Not all alerts indicate real attacks
- Some alerts are false positives or benign activity
- Limited SOC resources must be used efficiently
- Fast triage can stop attacks before they escalate

Triaging turns **chaos into clarity**.

---

## Core Alert Triage Factors

When reviewing alerts, analysts should always consider these four dimensions:

| Factor | Description | Why It Matters |
|------|-------------|----------------|
| **Severity** | Informational → Critical | Indicates urgency and business risk |
| **Time** | When the alert occurred and how often | Helps detect ongoing or repeated attacks |
| **Attack Stage** | Reconnaissance, persistence, exfiltration, etc. | Shows how far the attacker has progressed |
| **Affected Asset** | User, system, or resource involved | Determines potential impact |

### Quick Summary
- **Severity:** How bad is it?
- **Time:** When did it happen?
- **Context:** Where in the attack lifecycle?
- **Impact:** Who or what is affected?

---

## Next Steps After Initial Triage

Once these factors are reviewed, decide on the appropriate action:
- Escalate to Incident Response (IR)
- Perform deeper investigation
- Close or suppress the alert if confirmed benign

---

## Diving Deeper Into an Alert

For alerts that require investigation:

### 1. Review Alert Details
- Detection logic
- Entities involved (users, IPs, devices)
- Event data and metadata

### 2. Check Related Logs
- Look for unusual or suspicious activity
- Identify patterns aligned with the alert

### 3. Correlate Alerts
- Search for other alerts tied to the same:
  - User
  - IP address
  - Host or resource

Correlation often reveals a **larger attack chain**.

### 4. Build a Timeline
- Combine timestamps, events, and actions
- Reconstruct what happened before and after the alert

### 5. Decide the Outcome
- **Escalate** if indicators of compromise exist
- **Investigate further** if evidence is incomplete
- **Close/Suppress** if it’s a confirmed false positive

### 6. Document Everything
- Analysis steps
- Decisions made
- Lessons learned

Good documentation improves future SOC operations.

---

## Why Microsoft Sentinel?

Microsoft Sentinel enables SOC teams to:
- Centralise security visibility
- Automate detection and response
- Investigate threats efficiently
- Reduce alert fatigue

It turns alert chaos into actionable intelligence.

---

## Key Takeaways

- Alert triaging is essential during high-alert situations
- Not all alerts deserve equal attention
- A structured triage approach saves time and reduces risk
- Correlation and context reveal real threats
- Clear documentation strengthens SOC maturity

---

**SIEM:** Security Information and Event Management system that is used to aggregate security information in the form of logs, alerts, artifacts and events into a centralized platform that would allow security analysts to perform near real-time analysis during security monitoring.

**SOAR:** SOAR stands for Security Orchestration, Automation, and Response. It is a solution that helps organisations to streamline and automate their security operations, including incident management, threat intelligence, and vulnerability response.

**Persistence:** Malware often tries to keep a footprint in the system such that it keeps running even after a system restart. This is called persistence. For example, If a malware adds itself to the startup registry keys, it will persist even after a system restart. (_one of the attack stages_)

---

# In-Depth Log Analysis with Sentinel

**KQL:** KQL can refer to Kusto Query Language in the context of Azure, and Kibana Query Language in the context of Elastic. Both are query languages used to explore and process data based on search terms and filters.

After triage, raw log analysis is performed using **KQL**.

### Switching to KQL Mode

- Open the alert
- Click **Events** under Evidence
- Switch from *Simple mode* to **KQL mode**
- Run custom queries for deeper visibility

---

## KQL Investigation Example

To inspect events from a specific host (`app-02`):

```kql
set query_now = datetime(2025-10-30T05:09:25.9886229Z);
Syslog_CL
| where host_s == "app-02"
| project _timestamp_t, host_s, Message
```
