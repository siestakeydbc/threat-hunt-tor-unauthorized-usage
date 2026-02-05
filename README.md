# Threat Hunt: Unauthorized TOR Browser Usage

## Executive Summary

This project documents a threat-hunting investigation focused on the **unauthorized installation and use of the TOR Browser** within a corporate Windows environment. The objective of the hunt was to identify indicators of policy violation, potential insider risk, and early-stage malicious behavior using **Microsoft Defender for Endpoint (MDE)** telemetry and **KQL-based advanced hunting queries**.

While TOR itself is not inherently malicious, its unsanctioned use inside an enterprise environment introduces elevated risk, including data exfiltration, anonymized command-and-control activity, and violations of regulatory or acceptable-use policies. This investigation demonstrates a structured, SOC-style approach to detecting, validating, and contextualizing TOR activity using endpoint, process, and network telemetry.

The project is designed to mirror **real-world Security Operations Center (SOC) workflows**, emphasizing hypothesis-driven hunting, evidence correlation, and risk-based assessment rather than signature-only detection.

---

## Skills Demonstrated

This investigation highlights practical, job-relevant skills aligned with modern SOC and Threat Hunting roles:

- **Threat Hunting Methodology:** Hypothesis-driven, proactive detection
- **KQL Proficiency:** Advanced hunting across multiple MDE data tables
- **Endpoint Telemetry Analysis:** File, process, and network correlation
- **Insider Risk & Policy Assessment:** Identifying non-malware high-risk behavior
- **Evidence Correlation:** Reducing false positives through multi-signal validation
- **SOC Reporting:** Clear articulation of findings, risk, and recommendations
- **Security Tooling:** Microsoft Defender for Endpoint (MDE), KQL, GitHub documentation

---

## Threat Hypothesis

Unauthorized use of anonymization tools such as the TOR Browser within an enterprise Windows environment may indicate:

- Policy violations or insider risk  
- Attempts to evade network monitoring and security controls  
- Early-stage malicious activity, including anonymized command-and-control (C2) communications  

**Hypothesis:**  
If the TOR Browser is installed and used without authorization, corresponding indicators should be observable across endpoint telemetry, process execution logs, and network connection data collected by Microsoft Defender for Endpoint.

---

## Scope of Investigation

### Environment & Telemetry

- **Operating System:** Windows 10 / Windows 11  
- **Telemetry Source:** Microsoft Defender for Endpoint (MDE)  
- **Hunting Methodology:** Proactive threat hunting using KQL  

### Data Sources Analyzed

- `DeviceFileEvents`  
- `DeviceProcessEvents`  
- `DeviceNetworkEvents`  

The investigation intentionally avoided assuming malware presence and instead evaluated TOR usage strictly through a **policy enforcement and risk assessment lens**, reflecting how a SOC would triage non-standard but high-risk user behavior.

---

## Detection Logic Overview

The hunt was structured into multiple focused detection stages, each represented by a dedicated KQL query:

1. **Installer & Download Artifacts**  
   Detection of TOR-related installers and downloads inconsistent with approved software baselines.

2. **Process Execution**  
   Identification of TOR-related binaries (`tor.exe`, `firefox.exe`) executed from non-standard or user-space directories.

3. **Network Activity**  
   Detection of outbound connections to ports and destinations commonly associated with TOR relay traffic.

4. **User Artifacts & Intent**  
   Analysis of file creation and access patterns suggestive of anonymized browsing behavior.

Individually, each signal provided limited context. Correlated together, they formed a clear narrative of unauthorized anonymization tool usage.

---

## Investigation Findings

The investigation identified multiple indicators consistent with unauthorized TOR Browser usage within the environment.

### Summary of Observations

- TOR Browser installation activity outside approved deployment mechanisms  
- Execution of TOR-related binaries from non-standard paths  
- Outbound network connections consistent with TOR relay behavior  
- User file artifacts aligned with anonymized browsing activity  

### Assessment

While no direct malware payloads were observed, the **combined presence of installation artifacts, process execution, network activity, and user behavior indicators** supports the hypothesis of unauthorized anonymization tool usage.

This activity presents elevated risk due to potential:

- Data exfiltration  
- Policy evasion  
- Unmonitored external communications  

---

## MITRE ATT&CK Mapping

The observed behaviors align with the following MITRE ATT&CK techniques. Mapping reflects **behavioral alignment and risk assessment**, not confirmed adversary activity.

| Tactic | Technique ID | Technique Name | Relevance |
|------|-------------|---------------|----------|
| Defense Evasion | T1070 | Indicator Removal on Host | Use of anonymization to obscure activity |
| Command and Control | T1090 | Proxy | TOR network used as an anonymizing proxy |
| Command and Control | T1573 | Encrypted Channel | TOR-encrypted communications |
| Discovery | T1083 | File and Directory Discovery | User file creation and access activity |
| Execution | T1059 | Command and Scripting Interpreter | TOR-related process execution |

---

## Recommendations

### Immediate Response Actions

- Identify affected endpoints and associated user accounts for policy validation  
- Conduct targeted user or management review to determine business justification  
- Preserve relevant endpoint telemetry for follow-up investigation or compliance review  

### Detection & Monitoring Enhancements

- Implement continuous monitoring for TOR-related binaries across managed endpoints  
- Expand network detection logic for anonymization and proxy-related traffic  
- Correlate process execution with network activity to improve detection confidence  

### Policy & Control Improvements

- Clarify acceptable-use policies regarding anonymization tools  
- Enforce application control mechanisms to restrict unauthorized privacy software  
- Communicate monitoring expectations to reduce insider-risk ambiguity  

### Threat Hunting & SOC Maturity

- Incorporate anonymization detection as a recurring proactive hunt  
- Use this investigation as a baseline playbook for VPN, proxy, and evasion tool detection  
- Document lessons learned to refine escalation criteria and response consistency  

---

## Risk Considerations

While TOR Browser is not inherently malicious, unauthorized use in a corporate environment increases exposure to:

- Data exfiltration risk  
- Policy violations  
- Unmonitored command-and-control communications  
- Regulatory or compliance concerns  

Maintaining visibility and enforcing clear controls is essential to balancing user privacy with enterprise security requirements.

---

## Tools & Technologies

- Microsoft Defender for Endpoint (MDE)  
- Kusto Query Language (KQL)  
- Windows Endpoint Telemetry  
- SOC Threat Hunting Methodologies  

---

## Disclaimer

This project was conducted for educational and professional development purposes and reflects a **defensive, policy-focused threat-hunting scenario**. No malicious activity was executed or encouraged as part of this work.