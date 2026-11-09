# SOC Alert Investigation – Failed SSH Login Attempt

## 1. Alert Details

| Field | Value |
|---|---|
| Date | 06 November 2026 |
| Time | 06:16:56 AM |
| Host | mailsv1 |
| Process | sshd |
| Process ID | 1039 |
| Event Type | Failed Password Authentication |
| Target Username | root |
| Source IP | 194.8.74.23 |
| Source Port | 3768 |
| Protocol | SSH2 |
| Index | main |
| Sourcetype | secure-2 |

## 2. Original Log Event

```log
Thu Nov 06 2026 06:16:56 mailsv1 sshd[1039]:
Failed password for root from 194.8.74.23 port 3768 ssh2
```

## 3. Initial Observation

The log indicates that an SSH authentication attempt was made against the `root` account on the server `mailsv1`.

The authentication attempt originated from IP address `194.8.74.23` and failed due to an incorrect password.

## 4. Initial Assessment

- Suspicious SSH authentication attempt.
- Privileged root account targeted.
- External source IP identified for further investigation.
- No conclusion about compromise can be drawn from this event alone.
