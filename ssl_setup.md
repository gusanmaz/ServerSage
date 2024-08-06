# SSL Setup Guide

This guide covers setting up SSL certificates with Let's Encrypt and configuring HTTPS on your Nginx server.

## Table of Contents

1. [Installing Certbot](#installing-certbot)
2. [Obtaining SSL Certificates](#obtaining-ssl-certificates)
3. [Configuring Nginx for HTTPS](#configuring-nginx-for-https)
4. [Auto-renewal of Certificates](#auto-renewal-of-certificates)
5. [Testing SSL Configuration](#testing-ssl-configuration)

## Installing Certbot

1. Update your package list:
   ```bash
   sudo apt update
   ```

2. Install Certbot and the Nginx plugin:
   ```bash
   sudo apt install certbot python3-certbot-nginx
   ```

## Obtaining SSL Certificates

1. Run Certbot with the Nginx plugin:
   ```bash
   sudo certbot --nginx -d example.com -d www.example.com
   ```

2. Follow the prompts to provide your email address and agree to the terms of service.

3. Choose whether to redirect HTTP traffic to HTTPS when asked.

## Configuring Nginx for HTTPS

Certbot will automatically modify your Nginx configuration, but here's what it typically adds:

```nginx
server {
    listen 443 ssl http2;
    server_name example.com www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem;

    # ... rest of your server block ...
}

server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}
```

## Auto-renewal of Certificates

Certbot creates a systemd timer to automatically renew certificates. You can check its status:

```bash
sudo systemctl status certbot.timer
```

To test the renewal process:

```bash
sudo certbot renew --dry-run
```

## Testing SSL Configuration

Use the SSL Server Test by Qualys SSL Labs to check your SSL configuration:

https://www.ssllabs.com/ssltest/

To achieve an A+ rating, consider adding the following to your Nginx configuration:

```nginx
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
ssl_session_timeout 1d;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
```

Remember to reload Nginx after making changes:

```bash
sudo nginx -t && sudo systemctl reload nginx
```

With these steps, your website should now be securely served over HTTPS with an up-to-date SSL certificate from Let's Encrypt.