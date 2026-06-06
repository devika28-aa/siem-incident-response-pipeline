# Module 01: Post-Phishing Host Reconnaissance (MITRE ATT&CK T1033)

## 1. Technical Attack Simulation
* **Adversary Objective:** Auditing local system context, user group memberships, and administrative privilege boundaries immediately following initial access via a phishing vector.
* **Execution Command:** ```powershell
    whoami
    ```

---

## 2. Incident Response Playbook

### 🛠️ Phase 1: Detection & Analysis
* **SIEM Threat Hunting Query (KQL):** ```text
    message : *whoami* or message : *net*
    ```
* **Telemetry Verification:** Look for a synchronized volume anomaly spike on the timeline histogram inside the `logs-*` view. 
* **Forensic Parsing:** Expand the target document log entry. Inspect and verify the presence of **Sysmon Event ID 1 (Process Creation)**. Ensure that the `winlog.event_data.Image` field explicitly maps to `C:\Windows\System32\whoami.exe` and matches the targeted user account context (`DESKTOP-MFC4RRM\Devika`).

### 🛡️ Phase 2: Containment
* **Session Suspension:** Immediately identify and freeze the parent process ID (PID) spawning the unauthorized command line shell to block further interactive commands.
* **Credential Revocation:** Force an immediate enterprise-wide log-out and password reset for the compromised local/domain account profile (`Devika`) to disrupt potential lateral movement.

### 🧹 Phase 3: Eradication
* **Process Termination:** Kill all rogue administrative terminal process trees using elevated privileges:
    ```powershell
    Stop-Process -Id <Target_PID> -Force
    ```
* **Session Audit:** Run a thorough forensic integrity check on the host memory space to confirm no residual unmapped interactive shell wrappers are active in the background.

### 📝 Phase 4: Lessons Learned & System Hardening
* **Application Execution Restrictions:** Implement AppLocker or Software Restriction Policies via Group Policy Objects (GPOs) to block built-in discovery binaries like `whoami.exe` from executing out of non-administrative user paths (such as `\Users\*\Downloads\`).
* **Telemetry Automation:** Configure an automated alert trigger inside the SIEM architecture to instantly flag standard account profiles executing rapid, successive system discovery commands.

---

## 🖼️ Forensic Artifact Evidence
![Host Reconnaissance Pipeline Capture](./host_recon_evidence.png)
