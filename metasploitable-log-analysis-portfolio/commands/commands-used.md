# Commands Used

This file documents the main commands used during the project and why they were used.

## Network Discovery and Connectivity

```bash
ip a
```

Used on Metasploitable to identify the server IP address.

```bash
ping -c 2 <MS_IP>
```

Used from Kali to confirm network reachability to Metasploitable.

```bash
curl -I http://<MS_IP>/
```

Used from Kali to confirm the Apache web service responded over HTTP.

## SSH Access

```bash
ssh msfadmin@<MS_IP>
```

Used to connect from Kali to Metasploitable.

```bash
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 \
    -oHostKeyAlgorithms=+ssh-rsa \
    -oPubkeyAcceptedAlgorithms=+ssh-rsa \
    msfadmin@<MS_IP>
```

Used if the older Metasploitable SSH service required legacy algorithm support.

## Log Discovery

```bash
sudo ls -l /var/log/apache2
sudo ls -l /var/log/auth.log
sudo ls -l /var/log/secure
```

Used to locate Apache and authentication logs.

```bash
sudo find /var/log -type f -iname "*access*log*" 2>/dev/null | head
sudo find /var/log -type f -iname "*error*log*" 2>/dev/null | head
sudo find /var/log -type f \( -iname "*auth*" -o -iname "*secure*" \) 2>/dev/null | head
```

Used when common log paths were not immediately available.

## Live Log Monitoring

```bash
sudo tail -f /var/log/apache2/access.log
sudo tail -f /var/log/auth.log
```

Used to watch web and authentication logs update in real time.

## HTTP Traffic Generation

```bash
curl http://<MS_IP>/
curl http://<MS_IP>/not_real
curl http://<MS_IP>/admin
curl http://<MS_IP>/this_page_should_not_exist
```

Used to generate normal and intentionally noisy web requests.

```bash
for p in admin login phpmyadmin robots.txt sitemap.xml backup.zip .git/; do
  curl -s -o /dev/null http://<MS_IP>/$p
done
```

Used to generate a short burst of web requests that would be easy to find in Apache logs.

## Authentication Event Generation

```bash
ssh not_a_real_user@<MS_IP>
```

Used to create one failed SSH login event in the authentication log.

## Log Review and Triage

```bash
sudo tail -n 30 /var/log/apache2/access.log
sudo tail -n 60 /var/log/apache2/access.log
sudo tail -n 80 /var/log/auth.log
```

Used to review recent log windows.

```bash
sudo grep " 200 " /var/log/apache2/access.log | wc -l
sudo grep " 404 " /var/log/apache2/access.log | wc -l
```

Used to count successful and not-found HTTP responses.

```bash
sudo grep -i "curl" /var/log/apache2/access.log | tail -n 5
sudo grep -i "firefox" /var/log/apache2/access.log | tail -n 5
sudo grep -i "mozilla" /var/log/apache2/access.log | tail -n 5
```

Used to compare command-line and browser-based user agents.
