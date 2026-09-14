# Scenario [Number]: [Scenario Title]

## Objective

[1-2 sentences. Example: "Demonstrating the detection of an automated SSH dictionary attack against a Linux endpoint using Wazuh."]

## Incident Overview

| Attribute                  | Detail                      |
| :------------------------- | :-------------------------- |
| **MITRE ATT&CK Tactic**    | [e.g., Credential Access]   |
| **MITRE ATT&CK Technique** | [e.g., T1110 - Brute Force] |
| **Severity Level**         | [e.g., 10 - High]           |
| **Primary Rule ID**        | [e.g., 40111]               |
| **Target OS**              | [e.g., Debian 12 VM (`debian-agent`, `001`, `192.168.122.25`)] |

---

## 1. Attack Execution (Red Team)

[Explain how you generated the event. This proves you understand the offensive side. Include baseline/setup + each modification as separate subsections.]

**Setup / Baseline:**

```bash
[commands to create baseline state, e.g., mkdir, echo baseline, stat, sha256sum]
```

**Modification 1 - [description]:**

```bash
[attacker command, e.g., hydra -l soclab ...]
```

**Configuration (Manager - Arch Linux Docker host):**

[If config was needed. Dashboard has no `Edit centralized configuration` button in current versions — edit `/var/ossec/etc/shared/default/agent.conf` in the Manager container instead.]

```xml
<agent_config>
  [shared config snippet, e.g., syscheck / active-response]
</agent_config>
```

Then on Debian:

```bash
sudo systemctl restart wazuh-agent
```

> Note: [Document any gotchas here, e.g., `whodata` fails on `/tmp` (tmpfs) with `unable to audit rule` — use `realtime` only for `/tmp`, or move to `/opt/...` for full attribution.]

---

## 2. Detection & Evidence (Blue Team)

[Describe how Wazuh caught it: log source (`journald` / `syscheck` / `auditd`), decoder, rule logic.]

**Visual Evidence:**

Before attack (quiet baseline):<br>
![Before Attack](./assets/Events_Before_Attack.png)

After attack (alerts firing):<br>
![After Attack](./assets/Events_After_Attack.png)

Threat Hunting correlation (`[your query, e.g., agent.name: debian-agent AND syscheck.path: "..."]`):<br>
![Threat Hunting](./assets/ThreatHunting.png)

Endpoint terminal proof:<br>
![Logs](./assets/Logs.png)

**Log Artifacts:**

1. [Event 1 — timestamp, key fields like `srcip`, `size_before -> size_after`, `changed_attributes`]:

> Review the raw JSON artifact: [scenario_artifact_1.json](./scenario_artifact_1.json)

2. [Event 2 — if applicable, e.g., permission change]:

> Review the raw JSON artifact: [scenario_artifact_2.json](./scenario_artifact_2.json)

**SOC Investigation (Identify -> Document):**

| Step            | Finding                                              |
| :-------------- | :--------------------------------------------------- |
| **Identify**    | [What happened? Rule, level, location, mode]         |
| **Validate**    | [True Positive / False Positive? How confirmed?]     |
| **Investigate** | [Endpoint, user, IP, process, file — with IDs]       |
| **Correlate**   | [Related events? Same IP / firedtimes progression?]  |
| **Scope**       | [Isolated or multi-host? Query used to confirm]      |
| **Respond**     | [See Section 4]                                      |
| **Document**    | [Screenshots + JSON exports in this folder]          |

---

## 3. Root Cause Analysis

[Why did this happen? Misconfiguration, exposed service, weak perms?]

1. **[Weakness 1]:** [What + why it matters without detection.]
2. **[Weakness 2]:** [If applicable, e.g., `644 -> 777` persistence prep.]

---

## 4. Remediation & Response

- **Immediate Action:** [Contain: verify content/hash, remove artifact, kill session, block IP via Active Response / firewall. Include validation screenshots if Active Response used.]
- **Long-term Fix:**
  - [Hardening, e.g., SSH keys, critical-path FIM with `realtime + report_changes + whodata`, `auditd` enabled.]
  - [Tuning, e.g., ignore expected users, pair FIM with SCA.]
