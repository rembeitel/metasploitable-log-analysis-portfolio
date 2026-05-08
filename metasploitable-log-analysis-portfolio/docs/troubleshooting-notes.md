# Troubleshooting Notes

## Metasploitable SSH Compatibility

Metasploitable may use older SSH algorithms. If a normal SSH connection fails with a negotiation error, this compatibility command can be used in a lab environment:

```bash
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 \
    -oHostKeyAlgorithms=+ssh-rsa \
    -oPubkeyAcceptedAlgorithms=+ssh-rsa \
    msfadmin@<MS_IP>
```

## Log Path Differences

Apache and authentication logs may exist in different locations depending on the Linux distribution.

Common Apache paths:

```bash
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/httpd/access_log
/var/log/httpd/error_log
```

Common authentication log paths:

```bash
/var/log/auth.log
/var/log/secure
```

Search commands used when the exact path was unknown:

```bash
sudo find /var/log -type f -iname "*access*log*" 2>/dev/null | head
sudo find /var/log -type f -iname "*error*log*" 2>/dev/null | head
sudo find /var/log -type f \( -iname "*auth*" -o -iname "*secure*" \) 2>/dev/null | head
```

## Not Every Request Creates an Error Log Entry

Apache access logs are usually the primary source for HTTP request evidence. The error log may not update for every request, especially if the request only results in a normal `404` response.

## Avoiding Unsafe Testing

The commands in this project were used only against a local authorized lab server. They should not be pointed at public systems or networks without explicit permission.
