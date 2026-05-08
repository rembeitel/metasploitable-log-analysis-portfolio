# 03 — Manual Log Correlation

## Objective

The objective of this section was to manually correlate events across Apache access logs and Linux authentication logs using shared fields such as timestamp and source IP address.

## What I Did

1. Generated a burst of HTTP requests from Kali to Metasploitable.
2. Immediately attempted a failed SSH login from the same Kali client.
3. Reviewed a recent time window from the Apache access log.
4. Reviewed a recent time window from the Linux authentication log.
5. Identified the source IP address in both logs.
6. Used timestamps and source IPs to build a short activity timeline.

## Commands Used

```bash
for p in admin login phpmyadmin robots.txt sitemap.xml backup.zip .git/; do
  curl -s -o /dev/null http://<MS_IP>/$p
done

ssh not_a_real_user@<MS_IP>

sudo tail -n 60 /var/log/apache2/access.log
sudo tail -n 80 /var/log/auth.log
```

## What I Observed

The web requests appeared in the Apache access log, while the failed SSH login appeared in the authentication log. Although these logs recorded different services, they could still be connected because they shared key investigation fields.

The most important shared field was the source IP address. When the same source IP appeared in both the web access log and the authentication log within a close time window, it created a simple investigative story: the same client generated web activity and then attempted an SSH login.

## Example Timeline Format

| Time | Source IP | Log Source | Event Summary |
|---|---:|---|---|
| `<timestamp>` | `<KALI_IP>` | Apache access log | HTTP request to `/admin` or another tested path |
| `<timestamp>` | `<KALI_IP>` | Apache access log | HTTP request returned `404 Not Found` |
| `<timestamp>` | `<KALI_IP>` | Auth log | Failed SSH login for invalid user |

## Correlation Fields Used

| Field | How It Helped |
|---|---|
| Timestamp | Placed events in order |
| Source IP | Connected activity across different logs |
| Requested path | Explained what web resource was accessed |
| Status code | Showed success or failure of web requests |
| Username attempted | Added context to the SSH authentication event |
| Log source | Identified which service recorded the event |

## Skills Demonstrated

- Multi-log analysis
- Manual timeline building
- Event correlation using common fields
- Source IP investigation
- Web-to-authentication activity mapping
- Foundational SIEM investigation logic

## SOC Analyst Relevance

Manual correlation is an important analyst skill because SIEM alerts are built on the same idea: connecting related events across different sources. This exercise helped show how multiple low-level log entries can be combined into a clearer investigation narrative.
