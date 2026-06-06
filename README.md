# Case Study: End-to-End SIEM Detection & Digital Footprint Tracing of a Multi-Stage Host Compromise

## 1. Executive Summary
This case study documents the detection, engineering pipeline analysis, and digital forensic investigation of a simulated multi-stage adversary campaign targeting an enterprise Windows 11 endpoint (`DESKTOP-MFC4RRM`). The campaign spans post-compromise host reconnaissance, registry-based startup persistence, and application-layer data exfiltration. All telemetry was centralized using a local Elastic SIEM pipeline fed by kernel-level Sysmon auditing.

## 2. Investigative Environment Architecture
The defensive infrastructure engineered to capture this incident consists of a three-tier architecture:
* **Sensor Layer:** Microsoft System Monitor (Sysmon) v15.0 utilizing a low-level kernel driver to monitor process creation, registry modifications, and network connections.
* **Transport Layer:** Elastic Agent (v8.12.2) deployed locally to capture standard Windows Event logs and Sysmon operational channels.
* **Analytics Layer:** Elasticsearch cluster running via a local container environment, visualized natively via engineered Kibana Query Language (KQL) custom dashboards.

---

## 3. Timeline of Investigation & Forensic Artifacts

### Phase 1: Post-Phishing Host Reconnaissance (MITRE ATT&CK T1033)
* **Incident Discovery:** The analyst observed a synchronized telemetry spike on the custom Host Reconnaissance dashboard.
* **Forensic Evidence:** The attacker executed the native administrative utility `whoami.exe` under the compromised user context `DESKTOP-MFC4RRM\Devika`. 
* **SIEM Capture:** Sysmon intercepted the process execution at the kernel level (**Event ID 1: Process Creation**), capturing the exact timestamp, parent process mappings, and terminal execution strings.

### Phase 2: Startup Persistence Mechanism (MITRE ATT&CK T1547.001)
* **Incident Discovery:** To ensure volatile access survived standard user reboots, the adversary attempted to establish permanent residency.
* **Forensic Evidence:** The adversary called the Windows Registry Console tool (`reg.exe`) to inject a unauthorized key path string.
* **SIEM Capture:** The pipeline successfully bypassed traditional operating system logging blind spots, capturing **Sysmon Event ID 13 (RegistryValueName Set)**. The telemetry exposed the injection of a startup key value named `PhishingBackdoor` configured to execute a hidden payload path (`C:\Windows\System32\calc.exe`).

### Phase 3: Application Layer Data Exfiltration (MITRE ATT&CK T1105)
* **Incident Discovery:** The threat actor attempted to package and exfiltrate sensitive enterprise transaction logs.
* **Forensic Evidence:** The adversary utilized the native application utility `curl.exe` to execute an outbound web request, appending an exfiltration tracking query string: `?exfil=Financial_Fraud_Logs_Dump`.
* **SIEM Capture:** The advanced Elastic data stream successfully achieved field-level parsing. As verified in the expanded telemetry view, the SIEM captured **Sysmon Event ID 11 (File Created)**, tracking the system actively loading the underlying networking dynamic link library (`libcurl.dll`) inside a high-risk directory profile (`\atomics\T1105\bin\`), mapping explicitly to the MITRE framework.

---

## 4. Scalability: Digital Forensics & Financial Fraud Tracing
The pipeline engineered in this case study proves complete operational scalability. The exact same parsing architecture used to isolate the `libcurl.dll` network footprint can natively ingest Web Application Firewalls (WAF) and core financial transaction databases. By correlating local process tracking anomalies with national fraud reporting portals, incident response teams can map a comprehensive digital footprint—tracing an attacker from the initial local endpoint phishing compromise directly to the destination of fraudulent financial wire transfers.
