## SOC Incident Investigation Report – SSH Brute-Force Activity
# Suspicious activity
No successful login observed

# Investigation platform: Splunk SIEM
Target system: mailsv
Source IP: 194.8.74.23
Incident date: November 6, 2026
Log source: tutorialdata.zip:./mailsv/secure.log
1. Executive summary
During security log analysis in Splunk, I identified suspicious SSH authentication activity originating from IP address 194.8.74.23, targeting the server mailsv.

The investigation identified 132 failed SSH login attempts targeting five different usernames: local, system, irc, games, and root.

Further investigation of authentication logs revealed no successful login attempts from the identified IP address.

The activity is consistent with automated SSH credential-guessing behaviour, potentially involving brute-force attempts or password spraying.
2. Evidence collected
Indicator	Investigation finding
Source IP	194.8.74.23
Destination host	mailsv
Protocol	SSH (SSH2)
Failed attempts	132
Usernames targeted	5
Targeted root account	Yes
Successful authentication	None observed
Log source	Linux secure.log

# Targeted usernames:
- local
- system
- irc
- games
- root
3. Investigation methodology
Step 1: Identifying the suspicious event
Initial log:
Nov 06 06:16:56 mailsv1 sshd[1039]:
Failed password for root from 194.8.74.23 port 3768 ssh2


This indicated a failed SSH authentication attempt against the root account.
Step 2: Identifying repeated attempts
Splunk query:
index=main "194.8.74.23"
| stats count by host, source, sourcetype


The investigation identified 313 matching events initially. Further filtering identified 132 failed login attempts.
Step 3: Identifying targeted accounts
index=main "194.8.74.23" "Failed password"
| rex "Failed password for (invalid user )?(?<username>\S+)"
| stats count by username
| sort - count


Five usernames were identified as targets.
Step 4: Checking successful authentication
index=main "194.8.74.23"
("Accepted password" OR "Accepted publickey")
| table _time host _raw


No successful authentication events were found in the investigated logs.
Step 5: IP reputation investigation
Tool: VirusTotal

The source IP 194.8.74.23 should be checked against VirusTotal to identify:
- Malicious or suspicious detections.
- Threat-intelligence associations.
- Historical reports and observed activity.
The actual VirusTotal detection result was not provided, so its reputation remains unverified in this report.
4. Final incident classification
Final assessment
Suspicious

Likely activity: Automated SSH brute-force / credential-guessing attempt.

Compromise status: No successful authentication observed.

Confidence: Moderate. The repeated failures and multiple targeted accounts support the hypothesis, but the exact attack technique is not established.

Recommended priority: Investigate and apply appropriate SSH hardening.

5. Recommendations
1. Restrict SSH access to trusted IP addresses wherever possible.
2. Disable direct root SSH login.
3. Use SSH key-based authentication instead of password authentication.
4. Implement Fail2ban or equivalent rate-limiting protection.
5. Monitor repeated authentication failures using Splunk alerts.
6. Review other source IPs targeting the same server.
7. Preserve relevant authentication logs for further investigation.
VirusTotal should be used as supporting threat intelligence, not as the sole basis for classifying an IP as safe or malicious. Its IP reports provide contextual information such as network ownership, passive DNS and historical relationships. 
VirusTotal+1

6. Final conclusion
The investigation identified suspicious SSH authentication activity originating from 194.8.74.23, targeting the Linux server mailsv.

A total of 132 failed login attempts were identified across five usernames: local, system, irc, games, and root.

Analysis of the available authentication logs revealed no successful login attempts from the identified source IP. The activity is consistent with automated SSH credential-guessing behaviour, potentially involving brute-force attempts or password spraying.

The source IP was identified for further threat-intelligence investigation using VirusTotal. Its reputation assessment should be documented separately based on the actual lookup results.

No evidence of successful authentication or account compromise was identified during this investigation.

Final Verdict: Suspicious SSH authentication activity – attempted unauthorized access, with no confirmed compromise.

Recommended Response: Continue monitoring, investigate related source IPs, strengthen SSH authentication controls, and retain relevant logs for further analysis.




Report limitation: This conclusion is based on the events and searches shared during the investigation. It does not establish that no compromise occurred through another IP, service, or authentication method.

You now have a complete preliminary SOC investigation report suitable for your learning portfolio or SOC simulation documentation.##