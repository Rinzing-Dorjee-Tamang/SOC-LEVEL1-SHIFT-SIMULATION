# SOC-ALERT-01: Multiple Failed SSH Logins

## 1. Alert Summary

| Field | Value |
|---|---|
| Alert ID | SOC-ALERT-01 |
| Alert Name | Multiple Failed SSH Logins from One External IP |
| Platform | Splunk Cloud |
| Data Source | Linux SSH authentication logs |
| Index | `main` |
| Sourcetype | `secure-2` |
| Hosts in Scope | mailsv, www1, www2, www3 |
| Detection Condition | 10 or more failed SSH logins from one source IP |
| Initial Severity | Verify the severity configured in Splunk |
| Status | Investigated and recommended for L2 escalation |

## 2. Detection Query

```spl
index=main sourcetype=secure-2 "Failed password"
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3}) port"
| stats count by src_ip
| where count >= 10
```

## 3. What the Query Does

1. Searches the main index for Linux SSH authentication logs containing failed password messages.
2. Uses `rex` to extract the source IP address from each event.
3. Uses `stats count by src_ip` to calculate the number of failed login attempts from each IP address.
4. Uses `where count >= 10` to identify IP addresses with at least 10 failed attempts.

This detection logic helps identify repeated SSH authentication failures that may indicate brute-force or credential-guessing activity.


## 4. Simulation Note

This project uses a static training dataset containing previously collected security logs.

The detection query was tested manually in Splunk to validate its search logic against the available events.

The results demonstrate how the detection could identify repeated SSH authentication failures in a live SOC environment. The simulation does not represent real-time monitoring of an active production network.

## 5. Investigation

The alert investigation identified suspicious authentication activity involving the source IP `194.8.74.23`.

The detailed investigation, evidence, Splunk queries and final assessment are documented here:

[Investigation 01 – Multiple Failed SSH Logins](../investigations/investigation_01.md)