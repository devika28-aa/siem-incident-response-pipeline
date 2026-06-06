# Enterprise Incident Response (IR) Playbook & Framework
## 1. Purpose & Core Objectives
This document establishes the official Incident Response (IR) Framework for containing and eradicating multi-stage adversarial campaigns targeting enterprise infrastructure. The primary objective is to minimize operational downtime, protect sensitive transaction registries, and maintain absolute visibility over an attacker's digital footprint.

## 2. Standard IR Phase Matrix (NIST SP 800-61 r2)
Regardless of the attack vector, every security analyst must execute the following four fundamental lifecycle phases:
*   **Phase 1: Detection & Analysis:** Continuous ingestion of high-fidelity kernel telemetry (Sysmon) via Elastic Agent pipelines to isolate high-risk Indicators of Compromise (IoCs).
*   **Phase 2: Containment:** Immediate network-level segregation and credential token revocation to neutralize lateral threat movement.
*   **Phase 3: Eradication:** Elevated system purging of malicious process trees, unauthorized startup entries, and staging binaries.
*   **Phase 4: Post-Incident Activity:** Post-mortem analysis and Group Policy Object (GPO) hardening to ensure long-term architectural immunity.

---

## 🛠️ Specialized Tactical Playbooks
Because generic checklists fail against sophisticated threats, the IR lifecycle must be tailored specifically to the nature of each breach. 

Select the appropriate tactical playbook module below to execute attack-specific containment, eradication commands, and parsed KQL hunting queries:

### 📁 [Playbook Module 01: Post-Phishing Host Reconnaissance (T1033)](../01-post-phishing-host-reconnaissance/)
*   **Trigger:** Unauthorized execution of native administrative auditing commands (`whoami`, `net user`).
*   **Core Action:** Process tree freeze and proactive local credential revocation.

### 📁 [Playbook Module 02: Startup Registry Persistence Modification (T1547.001)](../02-registry-startup-persistence/)
*   **Trigger:** Tampering detected within system auto-run registry hives (Sysmon Event ID 13).
*   **Core Action:** Zero-reboot network containment and deep registry value purging (`reg delete`).

### 📁 [Playbook Module 03: Application Layer Protocol Data Exfiltration (T1105)](../03-application-layer-data-exfiltration/)
*   **Trigger:** Staging and exfiltration of data logs masquerading as standard web traffic (`curl`, `libcurl.dll`).
*   **Core Action:** Emergency egress firewall isolation and multi-tier financial fraud digital footprint mapping.
