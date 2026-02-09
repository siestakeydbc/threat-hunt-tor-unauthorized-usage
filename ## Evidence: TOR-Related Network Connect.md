## Evidence: TOR-Related Network Connections (Sanitized)

| Field | Value |
|------|------|
| Detection Type | Network Telemetry |
| Data Source | Microsoft Defender for Endpoint (MDE) |
| Query Used | `03_tor_network_activity.kql` |
| Direction | Outbound |
| Protocol | TCP |
| Initiating Processes | `tor.exe`, `firefox.exe` |
| TOR-Related Ports | `9001`, `9030`, `443` |
| Action | Allowed |
| Device Name | WIN-CLIENT-01 |
| Remote IP (Masked) | 185.220.101.xxx |
| Timestamp (UTC) | 2026-02-02 14:21:09 |
| Risk Classification | Policy Violation / Elevated Data Exfiltration Risk |

### Analyst Assessment

Encrypted network traffic alone is not inherently malicious. However, correlation between
TOR-specific processes (`tor.exe`, `firefox.exe`) and known TOR relay ports significantly
increases confidence in **unauthorized TOR usage** within the enterprise environment.

This activity represents elevated organizational risk due to:
- Potential policy violations
- Reduced network visibility
- Increased likelihood of unmonitored external communications