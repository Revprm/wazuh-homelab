# Scenario [Number]: [Scenario Title]

## Objective

[Provide a 1-2 sentence summary of what this lab exercise demonstrates. Example: "Demonstrating the detection of an automated SSH dictionary attack against a Linux endpoint using Wazuh."]

## Incident Overview

| Attribute                  | Detail                      |
| :------------------------- | :-------------------------- |
| **MITRE ATT&CK Tactic**    | [e.g., Credential Access]   |
| **MITRE ATT&CK Technique** | [e.g., T1110 - Brute Force] |
| **Severity Level**         | [e.g., 10 - High]           |
| **Primary Rule ID**        | [e.g., 40111]               |
| **Target OS**              | [e.g., Debian 12]           |

---

## 1. Attack Execution (Red Team)

[Explain how you generated the event. This proves you understand the offensive side of the alert.]

**Command / Tool Used:**
`[Insert your attack command here, e.g., hydra -l soclab...]`

---

## 2. Detection & Evidence (Blue Team)

[Describe how Wazuh caught the activity and what specific indicators tipped you off.]

**Visual Evidence:**
![Before Attack](./assets/Events_Before_Attack.png)
![After Attack](./assets/Events_After_Attack.png)

**Log Artifacts:**
The raw SIEM alert data containing the precise timestamp, source IP, and triggered rules has been exported and documented.

> Review the raw JSON artifact: [scenario_artifact.json](./scenario_artifact.json)

---

## 3. Root Cause Analysis

[Briefly explain the underlying vulnerability or misconfiguration that allowed this event to occur, or explain the mechanics of the attack.]

---

## 4. Remediation & Response

[Detail the actionable steps a SOC analyst would take to contain the threat and prevent future occurrences.]

- **Immediate Action:** [e.g., Block IP via Active Response / Firewall]
- **Long-term Fix:** [e.g., Disable password authentication and enforce SSH keys]
