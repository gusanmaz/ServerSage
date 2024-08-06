# Nginx Configuration Guide

This guide covers the setup, optimization, and common configurations for Nginx on a VPS.

## Table of Contents

1. [Installation](#installation)
2. [Basic Configuration](#basic-configuration)
3. [Server Blocks (Virtual Hosts)](#server-blocks-virtual-hosts)
4. [Optimizing Nginx](#optimizing-nginx)
5. [Common Configurations](#common-configurations)

## Installation

1. Update your package list:
   ```bash
   sudo apt update
   ```

2. Install Nginx:
   ```bash
   sudo apt install nginx
   ```

3. Start Nginx and enable it to start on boot:
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

## Basic Configuration

The main Nginx configuration file is located at `/etc/nginx/nginx.conf`. Here's a basic structure:

```nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 768;
    # multi_accept on;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    gzip on;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

## Server Blocks (Virtual Hosts)

Server blocks allow you to host multiple domains on a single server. Create a new file in `/etc/nginx/sites-available/`:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/example.com;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable the server block:
```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

## Optimizing Nginx

1. Adjust worker processes:
   ```nginx
   worker_processes auto;
   ```

2. Increase worker connections:
   ```nginx
   events {
       worker_connections 1024;
   }
   ```

3. Enable Gzip compression:
   ```nginx
   gzip on;
   gzip_vary on;
   gzip_proxied any;
   gzip_comp_level 6;
   gzip_types text/plain text/css text/xml application/json application/javascript application/rss+xml application/atom+xml image/svg+xml;
   ```

4. Configure caching:
   ```nginx
   location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
       expires 30d;
   }
   ```

## Common Configurations

1. Redirect www to non-www:
   ```nginx
   server {
       server_name www.example.com;
       return 301 $scheme://example.com$request_uri;
   }
   ```

2. Enable PHP-FPM:
   ```nginx
   location ~ \.php$ {
       fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
       fastcgi_index index.php;
       include fastcgi_params;
   }
   ```

3. Set up a reverse proxy:
   ```nginx
   location / {
       proxy_pass http://localhost:8080;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
   }
   ```

Remember to test your configuration after making changes:
```bash
sudo nginx -t
```

And reload Nginx to apply changes:
```bash
sudo systemctl reload nginx
```