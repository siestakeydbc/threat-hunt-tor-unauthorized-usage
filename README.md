# Threat Hunt: Unauthorized TOR Browser Usage

## Executive Summary

This project documents a threat-hunting investigation focused on the **unauthorized installation and use of the TOR Browser** within a corporate Windows environment. The objective of the hunt was to identify indicators of policy violation, potential insider risk, and early-stage malicious behavior using **Microsoft Defender for Endpoint (MDE)** telemetry and **KQL-based advanced hunting queries**.

While TOR itself is not inherently malicious, its unsanctioned use inside an enterprise environment introduces significant risk, including data exfiltration, anonymized command-and-control activity, and violations of regulatory or acceptable-use policies. This investigation demonstrates a structured SOC-style approach to detecting, validating, and contextualizing TOR activity using endpoint, process, and network telemetry.

The project is designed to mirror **real-world SOC operations**, emphasizing:
- Threat hypothesis development  
- Evidence-based detection  
- Log correlation across multiple data sources  
- Clear articulation of risk and findings  

This investigation was conducted as part of hands-on security operations training and is suitable for **SOC Analyst**, **Threat Hunter**, and **MDR** role portfolios.

## Threat Hypothesis & Scope

### Threat Hypothesis

Unauthorized use of anonymization tools such as the TOR Browser within an enterprise Windows environment may indicate:
- Policy violations or insider risk
- Attempts to evade network monitoring controls
- Early-stage malicious activity, including data exfiltration or anonymized command-and-control (C2) communications

The hypothesis for this hunt was that **if TOR Browser was installed and used without authorization**, corresponding indicators would be observable across endpoint telemetry, process execution logs, and network connection data.

---

### Scope of Investigation

The investigation focused on the following scope:

- **Operating System:** Windows 10 / Windows 11  
- **Telemetry Source:** Microsoft Defender for Endpoint (MDE)  
- **Hunting Methodology:** Proactive threat hunting using KQL  
- **Data Sources:**
  - DeviceFileEvents
  - DeviceProcessEvents
  - DeviceNetworkEvents

The hunt intentionally excluded assumptions of malware presence and instead evaluated TOR usage strictly through a **policy enforcement and risk assessment lens**, mirroring how a SOC would triage and escalate non-standard but high-risk user behavior.
## Investigation Findings

The investigation identified multiple indicators consistent with unauthorized TOR Browser usage within the environment.

### Summary of Observations

- Endpoint telemetry confirmed TOR Browser installation activity inconsistent with approved software baselines.
- Process execution logs showed TOR-related binaries (`tor.exe`, `firefox.exe`) launched outside standard application paths.
- Network telemetry revealed outbound connections to ports commonly associated with TOR relay traffic.
- File creation and access artifacts suggested user activity aligned with anonymized browsing behavior, including keywords commonly associated with TOR usage contexts.

### Assessment

While no direct malware payloads were observed, the combination of installation artifacts, process execution, network activity, and user file indicators supports the hypothesis of unauthorized anonymization tool usage. This behavior presents elevated risk due to the potential for data exfiltration, policy evasion, and unmonitored external communications.