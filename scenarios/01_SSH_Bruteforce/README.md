# Scenario 01: SSH Brute Force Attack

## Objective

Demonstrating the detection of an automated SSH dictionary attack against a Linux endpoint using Wazuh.

## Incident Overview

| Attribute                  | Detail              |
| :------------------------- | :------------------ |
| **MITRE ATT&CK Tactic**    | Credential Access   |
| **MITRE ATT&CK Technique** | T1110 - Brute Force |
| **Severity Level**         | 10 - High           |
| **Primary Rule ID**        | 40111               |
| **Target OS**              | Debian 12 VM        |

---

## 1. Attack Execution (Red Team)

An automated dictionary attack was launched from the Kali Linux attacker machine targeting the local user `soclab` on the Debian 12 agent.

**Command / Tool Used:**

```bash
hydra -l soclab -P /usr/share/wordlists/fasttrack.txt ssh://192.168.122.25

```

---

## 2. Detection & Evidence (Blue Team)

The Wazuh agent successfully intercepted the malicious traffic via the `journald` logs on the target endpoint. The system detected sequential Pluggable Authentication Modules (PAM) failures and aggressive connection terminations by the SSH daemon due to exceeded authentication limits.

**Visual Evidence:**

![Before Attack](./assets/Events_Before_SSH_Attack.png)
![After Attack](./assets/Events_After_SSH_Attack.png)

**Log Artifacts:**
The raw SIEM alert data containing the precise timestamp (`2026-09-13T04:56:31.845Z`), source IP (`192.168.122.228`), and triggered rules has been exported and documented.

> Review the raw JSON artifact: [scenario1_ssh_brute_force.json](/scenario1_ssh_brute_force.json)

---

## 3. Root Cause Analysis

The SSH service on the target endpoint was exposed to the local network with password-based authentication enabled. This configuration, combined with the lack of immediate rate-limiting at the host level, allowed an external actor to rapidly submit automated credential guesses without the connection being dropped.

---

## 4. Remediation & Response

- **Immediate Action:** Utilize Wazuh Active Response to dynamically add the attacking IP (`192.168.122.228`) to the firewall drop list after 5 failed authentication attempts.
- **Long-term Fix:** Modify the `/etc/ssh/sshd_config` file on the Debian endpoint to disable password authentication (`PasswordAuthentication no`) and enforce public key cryptography (e.g., `ed25519` keys).
