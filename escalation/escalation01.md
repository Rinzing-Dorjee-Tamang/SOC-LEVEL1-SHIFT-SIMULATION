### Escalation Request – SOC L1 to SOC L2

**Subject:** Suspicious Multi-Source SSH Authentication and Multi-Service Activity – mailsv

**Affected Asset:** mailsv

**Primary Source IP:** 194.8.74.23

**Incident Summary:**

During Splunk SIEM monitoring, suspicious authentication activity was identified against the Linux server `mailsv`.

A total of 132 failed SSH authentication attempts were identified from source IP `194.8.74.23`, targeting five usernames: `local`, `system`, `irc`, `games`, and `root`.

Further investigation identified additional source IP addresses generating failed password attempts against the same server. The primary source IP was also observed in events associated with other services, suggesting that the activity may extend beyond SSH authentication.

A review of the available authentication logs identified no successful login attempts from the primary source IP.

**Actions Performed by SOC L1:**

- Investigated suspicious SSH authentication logs in Splunk.
- Identified repeated failed login attempts and targeted usernames.
- Checked for successful authentication events.
- Identified additional source IPs generating authentication failures.
- Expanded the investigation to include activity involving other services.
- Prepared indicators and findings for further threat-intelligence investigation.

**Reason for Escalation:**

The combination of repeated authentication failures, multiple targeted usernames, activity from additional source IPs, and events involving other services warrants deeper investigation.

**Requested SOC L2 Actions:**

1. Conduct deeper correlation of authentication and service logs.
2. Investigate the source IPs and their reputation using threat-intelligence sources.
3. Determine whether the activity represents coordinated scanning, brute-force attempts, or password spraying.
4. Review successful authentication events across all identified source IPs.
5. Investigate possible post-authentication activity, privilege escalation, or lateral movement.
6. Assess whether containment measures are necessary.

**SOC L1 Assessment:**

Suspicious authentication and potential multi-service reconnaissance activity. No successful authentication has been observed from the primary source IP in the investigated logs.

**Escalation Status:** Recommended for SOC L2 investigation.

SOC L1 final verdict
Escalate to L2

Classification: Suspicious authentication activity with potential multi-service reconnaissance.

Compromise status: Not confirmed.

Severity: Requires further assessment by SOC L2 based on asset exposure, event scope and business impact.

Disposition: Escalate for deeper investigation and continue monitoring.