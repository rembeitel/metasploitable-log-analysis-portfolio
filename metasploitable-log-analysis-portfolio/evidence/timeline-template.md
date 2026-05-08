# Investigation Timeline Template

| Time | Source IP | Log Source | Event | Notes |
|---|---:|---|---|---|
| `<timestamp>` | `<KALI_IP>` | Apache access log | HTTP request to `<path>` | Status code: `<code>` |
| `<timestamp>` | `<KALI_IP>` | Apache access log | Additional web request | Look for repeated 404s or tool user-agent |
| `<timestamp>` | `<KALI_IP>` | Auth log | Failed SSH login | Attempted username: `<username>` |

## Summary

Write a short summary of what happened:

```text
The same source IP generated web requests against the Apache service and then attempted an SSH login. The events occurred close together in time, allowing the activity to be correlated across web and authentication logs.
```
