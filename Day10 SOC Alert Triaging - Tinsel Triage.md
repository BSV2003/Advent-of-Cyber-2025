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

