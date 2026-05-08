# 01 — Live Log Observation

## Objective

The objective of this section was to observe how actions performed from a Kali client machine appeared as log evidence on a Metasploitable server.

## What I Did

1. Identified the Metasploitable server IP address.
2. Confirmed Kali could reach the server using `ping` and `curl`.
3. Opened SSH sessions into Metasploitable.
4. Located Apache access logs and Linux authentication logs.
5. Used `tail -f` to watch new log entries appear in real time.
6. Generated normal web traffic from a browser.
7. Generated command-line HTTP traffic with `curl`.
8. Generated one failed SSH login attempt to create an authentication log event.

## Commands Used

```bash
ip a
ping -c 2 <MS_IP>
curl -I http://<MS_IP>/
ssh msfadmin@<MS_IP>
sudo ls -l /var/log/apache2
sudo ls -l /var/log/auth.log
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/auth.log
curl http://<MS_IP>/
curl http://<MS_IP>/this_page_should_not_exist
ssh not_a_real_user@<MS_IP>
```

## What I Observed

The Apache access log updated when I visited the Metasploitable web server from Kali. Browser requests and `curl` requests both appeared in the access log, but they could be distinguished by user-agent values and requested paths.

The authentication log updated after I attempted to log in over SSH with an invalid username. The failed login entry included useful investigation fields such as timestamp, source IP address, attempted username, and failure result.

## Evidence Fields Identified

### Apache Access Log

| Field | Why It Matters |
|---|---|
| Source IP | Identifies the client making the request |
| Timestamp | Shows when the request happened |
| HTTP method | Shows the action, such as `GET` |
| Requested path | Shows what resource was requested |
| Status code | Shows whether the request succeeded, failed, or redirected |
| User agent | Helps distinguish browsers, tools, and scripts |

### Authentication Log

| Field | Why It Matters |
|---|---|
| Timestamp | Shows when the login attempt occurred |
| Source IP | Identifies where the login attempt came from |
| Username | Shows which account was attempted |
| Result | Shows whether the login succeeded, failed, or used an invalid user |

## Skills Demonstrated

- Linux command-line navigation
- Server log discovery
- Real-time log monitoring
- Basic HTTP evidence analysis
- Basic SSH authentication log review
- Understanding how actions become forensic evidence

## SOC Analyst Relevance

This activity connects directly to SOC work because analysts frequently need to understand what a system recorded, where the evidence is stored, and how to identify the important fields in raw logs before escalating or investigating further.
