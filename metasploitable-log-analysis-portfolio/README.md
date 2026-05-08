# Metasploitable Log Analysis Portfolio Project

This repository documents a hands-on blue team log analysis project completed in a Kali Linux and Metasploitable lab environment. The goal was not to build a vulnerable system or create a training lab from scratch. The goal was to perform the exercises, observe real server-side log evidence, and explain what was learned from web access logs, authentication logs, noisy HTTP activity, and manual event correlation.

## Project Summary

I completed a three-part log analysis workflow using Metasploitable as the server and Kali Linux as the client. During the project, I generated controlled web and SSH activity, reviewed the resulting Apache and Linux authentication logs, identified important log fields, separated normal activity from noisy requests, and manually correlated events across multiple log sources.

The project demonstrates beginner SOC analyst skills such as:

- Locating Linux and Apache log files
- Watching logs update in real time with `tail -f`
- Generating controlled HTTP requests from a browser and `curl`
- Identifying source IP addresses, timestamps, HTTP methods, requested paths, status codes, and user agents
- Recognizing noisy web behavior such as repeated 404 responses and tool-like user agents
- Reviewing failed SSH authentication attempts in Linux auth logs
- Correlating Apache access logs and authentication logs using timestamp and source IP
- Building a short investigation timeline from multiple evidence sources

## Lab Environment Used

| Component | Purpose |
|---|---|
| Kali Linux | Client machine used to generate browser, curl, and SSH activity |
| Metasploitable | Vulnerable Linux server used to observe Apache and authentication logs |
| Apache access log | Evidence source for HTTP requests |
| Apache error log | Secondary source for web server errors |
| Linux auth log | Evidence source for SSH authentication attempts |
| Bash commands | Used for log viewing, filtering, counting, and traffic generation |

Common log paths checked during the project:

```bash
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/auth.log
/var/log/secure
```

## Repository Structure

```text
metasploitable-log-analysis-portfolio/
├── README.md
├── docs/
│   ├── 01-live-log-observation.md
│   ├── 02-web-log-triage.md
│   ├── 03-manual-log-correlation.md
│   ├── skills-demonstrated.md
│   └── troubleshooting-notes.md
├── evidence/
│   ├── evidence-notes-template.md
│   └── timeline-template.md
├── commands/
│   └── commands-used.md
├── sample-data/
│   ├── sample-access.log
│   └── sample-auth.log
└── GITHUB_SETUP.md
```

## What I Accomplished

### 1. Live Log Observation

I connected to the Metasploitable server, located the Apache access log and Linux authentication log, and watched both logs update live. I generated browser traffic, command-line HTTP requests, and a failed SSH login attempt from Kali. This showed how normal user activity becomes evidence in server logs.

### 2. Web Server Log Triage

I reviewed Apache access logs to distinguish normal web browsing from noisier command-line requests. I compared successful `200` responses with `404 Not Found` responses, reviewed requested paths, and used user-agent strings to identify browser traffic versus `curl` traffic.

### 3. Manual Log Correlation

I generated web requests and a failed SSH login attempt close together in time, then reviewed both Apache and authentication logs. I used shared fields such as timestamp and source IP address to build a simple timeline showing how activity across different services can be connected during an investigation.

## Example Commands Used

```bash
ip a
ping -c 2 <MS_IP>
curl -I http://<MS_IP>/
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/auth.log
sudo grep " 404 " /var/log/apache2/access.log | wc -l
sudo grep -i "curl" /var/log/apache2/access.log | tail -n 5
ssh not_a_real_user@<MS_IP>
```

More commands are documented in [`commands/commands-used.md`](commands/commands-used.md).

## Key Takeaways

- Server-side logs provide direct evidence of client activity.
- Apache access logs can reveal source IPs, HTTP methods, requested resources, status codes, and user agents.
- Repeated requests to paths like `/admin`, `/phpmyadmin`, `/backup.zip`, or `/.git/` can indicate reconnaissance or automated scanning behavior.
- Failed SSH attempts are visible in authentication logs and can be correlated with other activity from the same IP.
- Manual correlation is a foundational SOC skill before moving into SIEM tools.

## Portfolio Summary

**Metasploitable Log Analysis Project**  
Completed a hands-on blue team log analysis project using Kali Linux and Metasploitable. Observed live Apache and authentication logs, generated controlled HTTP and SSH events, triaged normal vs noisy web traffic, identified key log fields, and manually correlated events across multiple log sources to build an investigation timeline.

## Scope and Ethics

All activity documented in this repository was performed in an authorized local lab environment. The techniques and commands are intended for defensive cybersecurity learning, log analysis practice, and portfolio demonstration only.
