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
