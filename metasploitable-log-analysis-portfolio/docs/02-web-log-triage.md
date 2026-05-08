# 02 — Web Server Log Triage

## Objective

The objective of this section was to review Apache web access logs and separate normal web browsing from noisier request patterns.

## What I Did

1. Generated normal browser traffic to the Metasploitable web server.
2. Generated command-line web requests using `curl`.
3. Requested several paths that were likely to return `404 Not Found` responses.
4. Generated a short burst of web requests to simulate noisy behavior.
5. Reviewed the most recent Apache access log entries.
6. Counted status codes with `grep` and `wc`.
7. Compared browser user agents against `curl` user agents.
8. Checked the Apache error log for additional context.

## Commands Used

```bash
curl http://<MS_IP>/
curl http://<MS_IP>/not_real
curl http://<MS_IP>/admin
curl http://<MS_IP>/this_page_should_not_exist

for p in admin login phpmyadmin robots.txt sitemap.xml backup.zip .git/; do
  curl -s -o /dev/null http://<MS_IP>/$p
done

sudo tail -n 30 /var/log/apache2/access.log
sudo grep " 200 " /var/log/apache2/access.log | wc -l
sudo grep " 404 " /var/log/apache2/access.log | wc -l
sudo grep -i "curl" /var/log/apache2/access.log | tail -n 5
sudo grep -i "firefox" /var/log/apache2/access.log | tail -n 5
sudo tail -n 30 /var/log/apache2/error.log
```

## What I Observed

Normal browser traffic generally appeared as standard requests to the web root or site resources. These requests were often associated with browser-style user agents.

Command-line traffic generated with `curl` appeared differently. The user-agent field often made the request source clear, and intentional requests to paths such as `/not_real`, `/admin`, `/phpmyadmin`, `/backup.zip`, and `/.git/` produced entries that stood out from normal browsing.

Repeated `404` responses were especially useful for triage because they may indicate probing, reconnaissance, or automated scanning behavior when seen in large numbers or short time windows.

## Indicators Reviewed

| Indicator | Defensive Value |
|---|---|
| High number of `404` responses | May indicate scanning or probing |
| Requests to `/admin` or `/login` | May indicate authentication surface discovery |
| Requests to `/phpmyadmin` | Common target for opportunistic scanning |
| Requests to `/backup.zip` | May indicate attempts to discover exposed backups |
| Requests to `/.git/` | May indicate attempts to access exposed source repositories |
| `curl` user agent | May indicate scripted or command-line activity |

## Skills Demonstrated

- Apache access log review
- Status code analysis
- User-agent comparison
- Basic command-line filtering
- Recognition of noisy web behavior
- Understanding of common reconnaissance paths

## SOC Analyst Relevance

This activity is relevant to SOC work because web logs often contain early signs of reconnaissance. Even without a SIEM, simple command-line tools can help identify suspicious patterns such as repeated errors, unusual paths, and non-browser user agents.
