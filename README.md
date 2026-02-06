# Threat Hunt: Unauthorized TOR Browser Usage

## Executive Summary

This project documents a threat-hunting investigation focused on the **unauthorized installation and use of the TOR Browser** within a corporate Windows environment. The objective of the hunt was to identify indicators of policy violation, potential insider risk, and early-stage malicious behavior using **Microsoft Defender for Endpoint (MDE)** telemetry and **Kusto Query Language (KQL)** advanced hunting queries.

While TOR itself is not inherently malicious, its unsanctioned use inside an enterprise environment introduces elevated risk, including data exfiltration, anonymized command-and-control (C2) activity, and violations of regulatory or acceptable-use policies. This investigation demonstrates a structured, SOC-style approach to detecting, validating, and contextualizing TOR activity using endpoint, process, and network telemetry.

The project mirrors **real-world Security Operations Center (SOC) workflows**, emphasizing hypothesis-driven hunting, evidence correlation, and risk-based assessment rather than signature-only detection.

---

## Skills Demonstrated

This investigation highlights practical, job-relevant skills aligned with SOC Analyst and Threat Hunting roles:

- Threat hunting methodology using hypothesis-driven detection
- Advanced KQL across multiple Microsoft Defender for Endpoint tables
- Endpoint telemetry analysis (file, process, and network events)
- Insider risk and policy-based security assessment
- Multi-source evidence correlation to reduce false positives
- SOC-style reporting, documentation, and recommendations
- Security tooling: Microsoft Defender for Endpoint (MDE), KQL, GitHub workflows

---

## Threat Hypothesis

Unauthorized use of anonymization tools such as the TOR Browser within an enterprise Windows environment may indicate:

- Policy violations or insider risk
- Attempts to evade network monitoring and security controls
- Early-stage malicious activity, including anonymized command-and-control (C2) or data exfiltration

**Hypothesis**

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

This investigation intentionally avoided assuming malware presence and instead evaluated TOR usage through a **policy enforcement and risk assessment lens**, reflecting how a SOC would triage non-standard but high-risk user behavior.

---

## Detection Logic Overview

The hunt was structured into focused detection stages, each represented by a dedicated KQL query:

1. **Installer & Download Artifacts**  
   Detection of TOR-related installation files and downloads inconsistent with approved software baselines.

2. **Process Execution**  
   Identification of TOR-related binaries (`tor.exe`, `firefox.exe`) executed from non-standard or user-space directories.

3. **Network Activity**  
   Detection of outbound connections consistent with TOR relay behavior.

4. **User Artifacts & Intent**  
   Analysis of file creation and access patterns suggestive of anonymized browsing behavior.

Individually, each signal provided limited context. Correlated together, they formed a coherent narrative of unauthorized anonymization tool usage.

---

## Investigation Findings

The investigation identified multiple indicators consistent with unauthorized TOR Browser usage within the environment.

### Summary of Observations

- TOR Browser installation activity outside approved deployment mechanisms
- Execution of TOR-related binaries from non-standard paths
- Outbound network connections consistent with TOR relay traffic
- User file artifacts aligned with anonymized browsing behavior

### Assessment

While no direct malware payloads were observed, the **combined presence of installation artifacts, process execution, network activity, and user behavior indicators** supports the hypothesis of unauthorized anonymization tool usage.

This activity presents elevated risk due to potential:

- Data exfiltration
- Policy evasion
- Unmonitored external communications

---

## MITRE ATT&CK Mapping

The observed behaviors align with the following MITRE ATT&CK techniques. Mapping reflects **behavioral alignment and risk assessment**, not confirmed adversary attribution.

| Tactic | Technique ID | Technique Name | Relevance |
|------|-------------|---------------|----------|
| Defense Evasion | T1070 | Indicator Removal on Host | Anonymization to obscure activity |
| Command and Control | T1090 | Proxy | TOR network used as anonymizing proxy |
| Command and Control | T1573 | Encrypted Channel | TOR-encrypted communications |
| Discovery | T1083 | File and Directory Discovery | User file creation and access activity |
| Execution | T1059 | Command and Scripting Interpreter | TOR-related process execution |

---

## Recommendations

### Immediate Response Actions

- Identify affected endpoints and associated user accounts
- Validate activity against acceptable-use policies
- Preserve endpoint telemetry for follow-up investigation or compliance review

### Detection & Monitoring Enhancements

- Implement continuous monitoring for TOR-related binaries
- Expand network detection for anonymization and proxy traffic
- Correlate process execution with network activity to improve detection confidence

### Policy & Control Improvements

- Clarify acceptable-use policies regarding anonymization tools
- Enforce application control for unauthorized privacy software
- Communicate monitoring expectations to reduce insider-risk ambiguity

### Threat Hunting & SOC Maturity

- Incorporate anonymization tool detection into recurring threat hunts
- Use this investigation as a baseline playbook for VPN, proxy, or evasion tool abuse
- Document lessons learned to refine escalation and response consistency

---

## Risk Considerations

While TOR Browser is not inherently malicious, unauthorized use in a corporate environment increases exposure to:

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

---

## Appendix A: How to Run These Queries (MDE)

This investigation uses **Microsoft Defender for Endpoint Advanced Hunting**.

### Steps

1. Log in to **Microsoft 365 Defender**
2. Navigate to **Hunting → Advanced Hunting**
3. Paste KQL queries from the `/queries/` directory
4. Execute queries in the following order:
   - File artifacts
   - Process execution
   - Network activity
   - User artifact correlation
5. Validate results by hostname, username, and timestamp proximity
6. Correlate findings across tables to reduce false positives

**Note:** In SOC environments, confidence comes from **correlation**, not a single alert.

---

## Appendix B: SOC Interview Walkthrough (Desk Reference)

### “Walk me through this project.”

> I started with a hypothesis that unauthorized TOR usage could represent policy violations or insider risk. Using Microsoft Defender for Endpoint advanced hunting, I looked for weak signals across file, process, and network telemetry. Individually, these signals were low confidence, but when correlated, they formed a clear narrative of unauthorized anonymization tool usage. I evaluated the activity through a policy and risk lens rather than assuming malware, which reflects how a SOC actually triages ambiguous behavior.

### “Why is TOR a concern if it isn’t malware?”

> TOR bypasses traditional monitoring controls and enables encrypted, anonymized communication. In an enterprise environment, that creates blind spots for data exfiltration and policy enforcement even if no malware is present.

### “How would you escalate this?”

> I would validate business justification with management, preserve telemetry, and escalate based on policy impact and risk tolerance rather than technical severity alone.

### “What would you improve next?”

> I would automate correlation between endpoint execution and network behavior and incorporate anonymization detection into recurring threat hunts.
