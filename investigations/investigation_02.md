# Investigation Report — ALERT-002: Possible Encoded PowerShell Execution

## 1. Alert Information

- **Alert ID:** ALERT-002
- **Alert Name:** Possible Encoded PowerShell Execution
- **Category:** Endpoint Security / Process Execution
- **Initial Severity:** Medium
- **Final Assessment:** Benign Activity
- **Classification:** False Positive for malicious activity
- **Affected Account:** ATTACKRANGE\Administrator
- **Hostname:** Windows Endpoint (uploaded Sysmon dataset)
- **Log Source:** Windows Sysmon
- **MITRE ATT&CK:** T1059.001 — PowerShell

## 2. Alert Description

The investigation focused on a PowerShell process launched with execution policy bypass and an encoded command.

The objective was to determine whether the command was associated with malicious execution or legitimate administrative activity.

## 3. Investigation Details

### 3.1 Process Identification

A PowerShell process creation event (Sysmon Event ID 1) was identified.

- Executable: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- Process ID: `5948`
- Parent Process: `C:\Windows\System32\cmd.exe`
- Parent Process ID: `4936`
- Timestamp: `2026-11-09 10:17:41`

### 3.2 Command-Line Analysis

The PowerShell command included:

- `-Exec bypass`
- `-enc`

These arguments indicate execution policy bypass and encoded command execution.

The Base64 command was extracted and decoded using UTF-16LE.

**Decoded command:**

`Write-Host "Hello World"`

The decoded command only prints text to the PowerShell console. No malicious functionality was identified in the decoded payload.

### 3.3 Parent Process Investigation

The parent process was identified as `cmd.exe`.

The parent command line was:

`"C:\Windows\system32\cmd.exe"`

The corresponding CMD process creation event could not be located in the uploaded dataset. Therefore, the original process responsible for launching CMD could not be established.

### 3.4 User and Execution Context

- User: `ATTACKRANGE\Administrator`
- Integrity Level: High
- Current Directory: `C:\Users\Administrator\`

The process executed with elevated privileges.

### 3.5 File Creation Investigation

A Sysmon Event ID 11 file creation event was identified for PowerShell PID 5948.

Created file:

`C:\Users\Administrator\AppData\Local\Temp\__PSScriptPolicyTest_1py4clft.gbo.ps1`

The filename is consistent with a PowerShell script execution-policy test artifact.

### 3.6 Network Investigation

Sysmon Event ID 3 was searched using the PowerShell process ID.

No matching network connection events were identified in the available dataset.

This does not establish that no network activity occurred outside the available telemetry.

### 3.7 Process Access Investigation

Sysmon Event ID 10 was searched for PowerShell PID 5948.

No matching process-access events were identified.

### 3.8 Executable Hashes

The Sysmon event recorded the following executable hashes:

- MD5: `097CE5761C89434367598B34FE32893B`
- SHA256: `BA4038FD20E474C047BE8AAD5BFACDB1BFC1DDBE12F803F473B7918D8D819436`

These hashes correspond to the PowerShell executable, not the decoded command or temporary script.

## 4. Evidence Summary

| Evidence | Finding |
|---|---|
| Encoded PowerShell | Identified |
| Execution policy bypass | Identified |
| Decoded payload | `Write-Host "Hello World"` |
| Parent process | cmd.exe |
| Parent creation event | Not available |
| User | ATTACKRANGE\Administrator |
| Integrity Level | High |
| Temporary file creation | Identified |
| Matching network activity | Not found |
| Matching process access | Not found |

## 5. Final Assessment

**Classification: Benign Activity / False Positive for malicious activity**

The investigation confirmed that PowerShell executed with an encoded command and execution policy bypass.

However, decoding revealed only a simple console output command. The associated temporary file was consistent with PowerShell execution-policy testing.

No matching network connection or process-access activity was identified in the available Sysmon dataset.

The available evidence does not establish malicious execution or system compromise.

## 6. Investigation Limitations

- The parent CMD process creation event was not available.
- The investigation was limited to the uploaded Sysmon dataset.
- No independent validation of the PowerShell executable hash was performed.
- Absence of matching network events does not prove that no network activity occurred.

## 7. Recommended Actions

- Retain the investigation evidence.
- Validate the executable hash against a trusted source if required.
- Review the original CMD process creation telemetry if additional logs become available.
- Escalate if additional evidence of suspicious activity is discovered.

## 8. Tools Used

- Splunk Cloud
- Windows Sysmon
- SPL (Search Processing Language)
- CyberChef

## 9. Lessons Learned

This investigation demonstrated that encoded PowerShell execution and execution policy bypass are suspicious indicators, but do not independently establish malicious activity.

Decoding commands, examining process relationships, and correlating endpoint telemetry are essential to making evidence-based SOC decisions.