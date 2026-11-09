# SOC Incident Report
Escalation assessment required

Suspicious Authentication & Multi-Service Activity
SOC L1 Investigation | Splunk SIEM

Incident details	Value
Incident date	06 November 2026
Affected host	mailsv
Primary source IP	194.8.74.23
Total matching events	313
Failed SSH attempts	132
Targeted usernames	5
Successful logins from primary IP	None observed
Log source	Linux secure.log
Initial classification	Suspicious activity


1. Executive summary
During routine security monitoring and log analysis in Splunk SIEM, suspicious authentication activity was identified against the Linux server mailsv.

The investigation identified 132 failed SSH authentication attempts originating from 194.8.74.23. The source IP attempted to authenticate using five different usernames: local, system, irc, games, and root.

Further analysis identified additional IP addresses generating failed password attempts against the same server. The primary source IP was also observed in events associated with other services, expanding the scope of the investigation beyond SSH authentication.

A review of the available authentication logs revealed no successful login attempts from the primary source IP.

The activity is consistent with possible automated credential guessing, username enumeration, and multi-service reconnaissance. However, the exact attack technique and whether the activity is coordinated remain unconfirmed.
2. Investigation evidence
A. Suspicious SSH authentication
Initial event identified:
Nov 06 06:16:56 mailsv1 sshd[1039]:
Failed password for root from 194.8.74.23 port 3768 ssh2


Key observations:
- SSH authentication failure.
- Root account targeted.
- External source IP identified.
- Password-based authentication attempted.
B. Multiple failed login attempts
Splunk search:
index=main "194.8.74.23"
| stats count by host, source, sourcetype


The initial search returned 313 matching events.

Further filtering identified 132 failed password events.
C. Multiple usernames targeted
The following usernames were identified:
Username	Observation
local	Targeted
system	Targeted
irc	Targeted
games	Targeted
root	Targeted

This behaviour may indicate automated account discovery or credential-guessing activity.
D. Successful authentication check
Authentication logs were searched for:
- Accepted password
- Accepted publickey
No successful authentication was identified from 194.8.74.23 in the investigated logs.

Finding: No evidence of successful authentication from the primary source IP.
E. Multiple source IP addresses
Additional IP addresses were identified generating failed password attempts against mailsv.

This indicates that the server may be receiving authentication attempts from multiple external sources.

The individual IP addresses, attempt counts and relationships have not yet been fully documented.
F. Multi-service activity
The primary source IP was also observed in events associated with other services.

This warrants further investigation to establish whether the source was performing service discovery, authentication attempts, or other suspicious activities.

The exact services and event details still need to be confirmed.
3. Threat assessment
Potential activity	Assessment
SSH brute-force attempts	Suspected
Password spraying	Possible, unconfirmed
Username enumeration	Possible
Multi-source authentication attacks	Observed
Multi-service reconnaissance	Requires verification
Successful account compromise	Not observed from primary IP

4. SOC L1 actions performed
The following activities were performed during initial triage:
- Identified suspicious SSH authentication events in Splunk.
- Extracted the source IP and destination hostname.
- Investigated repeated authentication failures.
- Identified the number of failed login attempts.
- Identified multiple targeted usernames.
- Distinguished failed authentic
- Investigated additional IP addresses targeting the server.
- Expanded the investigation to events associated with other services.
- Identified the need for external IP reputation checks through VirusTotal.
- Documented investigation findings and prepared recommendations.
- Recommended containment and remediation
The following actions should be considered by the responsible security or infrastructure team.
Recommendation	Purpose
Restrict SSH access	Limit access to trusted networks
Disable direct root SSH login	Reduce privileged account exposure
Enforce SSH key authentication	Reduce password-based attacks
Configure Fail2ban	Limit repeated authentication attempts
Review firewall rules	Restrict unnecessary service exposure
Monitor authentication logs	Detect repeated failures and successful logins
Review user accounts	Identify unnecessary or unused accounts

Any IP blocking should follow the organisation's incident response procedures and account for the possibility of shared or legitimate infrastructure.

6. Escalation to SOC L2
Recommended: Escalate for further investigation
Although no successful authentication was identified from the primary IP, escalation to SOC L2 is justified because the investigation has identified multiple suspicious indicators:
- 132 failed SSH login attempts from one source.
- Five different usernames targeted, including root.
- Additional source IP addresses generating failed authentication attempts.
- The primary IP appearing in events associated with other services.
- Potentially broader activity against the same server.