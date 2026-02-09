### Evidence: TOR-Related Network Connections

| Field | Value |
|------|------|
| Query | `03_tor_network_activity.kql` |
| Detection Type | Network Telemetry |
| Direction | Outbound |
| Protocol | TCP |
| Initiating Processes | `tor.exe`, `firefox.exe` |
| TOR-Related Ports | `9001`, `9030`, `443` |
| Action | Allowed |
| Device | WIN-CLIENT-01 |
| Remote IP (masked) | 185.220.101.xxx |
| Timestamp (UTC) | 2026-02-02 14:21:09 |
| Risk Classification | Policy Violation / Elevated Data Exfiltration Risk |

**Assessment:**  
While encrypted traffic alone is not malicious, correlation with TOR-specific processes and relay ports increases confidence in unauthorized TOR usage within the enterprise environment.