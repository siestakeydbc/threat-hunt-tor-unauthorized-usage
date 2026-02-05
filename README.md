# Threat Hunt: Unauthorized TOR Browser Usage

## Executive Summary

This project documents a threat-hunting investigation focused on the **unauthorized installation and use of the TOR Browser** within a corporate Windows environment. The objective of the hunt was to identify indicators of policy violation, potential insider risk, and early-stage malicious behavior using **Microsoft Defender for Endpoint (MDE)** telemetry and **KQL-based advanced hunting queries**.

While TOR itself is not inherently malicious, its unsanctioned use inside an enterprise environment introduces significant risk, including data exfiltration, anonymized command-and-control activity, and violations of regulatory or acceptable-use policies. This investigation demonstrates a structured, SOC-style approach to detecting, validating, and contextualizing TOR activity using endpoint, process, and network telemetry.

The project is designed to mirror **real-world Security Operations Center (SOC) workflows**, emphasizing hypothesis-driven hunting, evidence correlation, and risk-based assessment rather than signature-only detection.

---

## Threat Hypothesis

Unauthorized use of anonymization tools such as the TOR Browser within an enterprise Windows environment may indicate:

- Policy violations or insider risk  
- Attempts to evade network monitoring and security controls  
- Early-stage malicious activity, including data exfiltration or anonymized command-and-control (C2) communications  

**Hypothesis:**  
If the TOR Browser is installed and used without authorization, corresponding indicators should be observable across endpoint telemetry, process execution logs, and network connection data collected by Microsoft Defender for Endpoint.

---

## Scope of Investigation

**Environment & Telemetry**

- **Operating System:** Windows 10 / Windows 11  
- **Telemetry Source:** Microsoft Defender for Endpoint (MDE)  
- **Hunting Methodology:** Proactive threat hunting using KQL  

**Data Sources Analyzed**

- `DeviceFileEvents`  
- `DeviceProcessEvents`  
- `DeviceNetworkEvents`  

The investigation intentionally avoided assuming malware presence and instead evaluated TOR usage strictly through a **policy enforcement and risk assessment lens**, reflecting how a SOC would triage non-standard but high-risk user behavior.

---

## Detection Logic Overview

The hunt was structured into multiple focused detection stages, each represented by a dedicated KQL query:

1. **Installer & Download Artifacts**  
   Identified TOR-related installation files and download activity inconsistent with approved software baselines.

2. **Process Execution**  
   Detected execution of TOR-related binaries (`tor.exe`, `firefox.exe`) from non-standard or user-space directories.

3. **Network Activity**  
   Identified outbound connections to ports and destinations commonly associated with TOR relay traffic.

4. **User Artifacts & Intent**  
   Examined file creation and access patterns suggestive of anonymized browsing behavior and TOR-related user activity.

Each query independently surfaced weak signals; collectively, they formed a coherent narrative of unauthorized anonymization tool usage.

---

## Investigation Findings

The investigation identified multiple indicators consistent with unauthorized TOR Browser usage within the environment.

### Summary of Observations

- Endpoint telemetry confirmed TOR Browser installation activity outside approved software deployment mechanisms.  
- Process execution logs showed TOR-related binaries launched from non-standard paths.  
- Network telemetry revealed outbound connections consistent with TOR relay behavior.  
- User file artifacts aligned with anonymized browsing activity and TOR usage contexts.

### Assessment

While no direct malware payloads were observed, the **combined presence of installation artifacts, process execution, network activity, and user behavior indicators** supports the hypothesis of unauthorized anonymization tool usage.

This activity presents elevated risk due to the potential for:

- Data exfiltration  
- Policy evasion  
- Unmonitored external communications  

---

## Recommendations

Based on the findings of this investigation, the following actions are recommended to reduce risk, improve visibility, and prevent unauthorized anonymization tool usage.

### Immediate Response Actions

- Identify affected endpoints and associated user accounts for validation against acceptable-use policies.  
- Conduct targeted user or management review to determine business justification, if any.  
- Preserve relevant endpoint telemetry and logs for potential follow-up investigation or compliance review.

### Detection & Monitoring Enhancements

- Implement continuous monitoring for TOR-related binaries across managed endpoints.  
- Expand network detection logic to flag connections associated with anonymization services.  
- Correlate endpoint process execution with network activity to reduce false positives and improve detection confidence.

### Policy & Control Improvements

- Review and clarify acceptable-use policies regarding anonymization tools and unapproved software.  
- Enforce application control mechanisms to restrict execution of unauthorized privacy tools.  
- Communicate monitoring expectations clearly to reduce insider-risk ambiguity.

### Threat Hunting & SOC Maturity

- Incorporate anonymization tool detection as a recurring proactive threat-hunting scenario.  
- Use this investigation as a baseline playbook for detecting VPN abuse, proxy tools, or evasion frameworks.  
- Document lessons learned to refine escalation criteria and response consistency.

---

## Risk Considerations

While TOR Browser is not inherently malicious, unauthorized usage in a corporate environment increases exposure to:

- Data exfiltration risk  
- Policy violations  
- Unmonitored command-and-control communications  
- Regulatory or compliance concerns  

Maintaining visibility and enforcing clear controls is essential to balancing user privacy considerations with enterprise security requirements.

---

## Tools & Technologies

- Microsoft Defender for Endpoint (MDE)  
- Kusto Query Language (KQL)  
- Windows Endpoint Telemetry  
- SOC Threat Hunting Methodologies

---

## Disclaimer

This project was conducted for educational and professional development purposes and reflects a defensive, policy-focused threat-hunting scenario. No malicious activity was executed or encouraged as part of this work.