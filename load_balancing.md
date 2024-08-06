# Load Balancing Guide

This guide covers implementing load balancing for high-traffic scenarios on a VPS.

## Table of Contents

1. [Nginx as a Load Balancer](#nginx-as-a-load-balancer)
2. [HAProxy Load Balancing](#haproxy-load-balancing)
3. [Layer 4 vs Layer 7 Load Balancing](#layer-4-vs-layer-7-load-balancing)
4. [Load Balancing Algorithms](#load-balancing-algorithms)
5. [Health Checks](#health-checks)
6. [SSL Termination](#ssl-termination)

## Nginx as a Load Balancer

1. Install Nginx:
   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. Configure Nginx as a load balancer:
   ```nginx
   http {
       upstream backend {
           server 192.168.1.101;
           server 192.168.1.102;
           server 192.168.1.103;
       }

       server {
           listen 80;
           location / {
               proxy_pass http://backend;
           }
       }
   }
   ```

3. Restart Nginx:
   ```bash
   sudo systemctl restart nginx
   ```

## HAProxy Load Balancing

1. Install HAProxy:
   ```bash
   sudo apt update
   sudo apt install haproxy
   ```

2. Configure HAProxy:
   ```
   global
       log /dev/log local0
       log /dev/log local1 notice
       chroot /var/lib/haproxy
       stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
       stats timeout 30s
       user haproxy
       group haproxy
       daemon

   defaults
       log global
       mode http
       option httplog
       option dontlognull
       timeout connect 5000
       timeout client  50000
       timeout server  50000

   frontend http_front
      bind *:80
      default_backend http_back

   backend http_back
      balance roundrobin
      server server1 192.168.1.101:80 check
      server server2 192.168.1.102:80 check
      server server3 192.168.1.103:80 check
   ```

3. Restart HAProxy:
   ```bash
   sudo systemctl restart haproxy
   ```

## Layer 4 vs Layer 7 Load Balancing

- Layer 4 (Transport Layer): Based on IP address and port
- Layer 7 (Application Layer): Can route based on content of the request (e.g., URL)

## Load Balancing Algorithms

1. Round Robin: Requests are distributed evenly across servers
2. Least Connections: New requests go to the server with the fewest active connections
3. IP Hash: The client's IP address is used to determine which server receives the request

Example (Nginx):
```nginx
upstream backend {
        least_conn;
        server 192.168.1.101;
        server 192.168.1.102;
        server 192.168.1.103;
    }
}
```

## Health Checks

Health checks ensure that traffic is only sent to healthy servers.

1. Nginx health checks:
   ```nginx
   upstream backend {
       server 192.168.1.101:80 max_fails=3 fail_timeout=30s;
       server 192.168.1.102:80 max_fails=3 fail_timeout=30s;
       server 192.168.1.103:80 max_fails=3 fail_timeout=30s;
   }
   ```

2. HAProxy health checks:
   ```
   backend http_back
       balance roundrobin
       option httpchk GET /health
       server server1 192.168.1.101:80 check
       server server2 192.168.1.102:80 check
       server server3 192.168.1.103:80 check
   ```

## SSL Termination

SSL termination at the load balancer can offload SSL processing from backend servers.

1. Nginx SSL termination:
   ```nginx
   http {
       upstream backend {
           server 192.168.1.101;
           server 192.168.1.102;
           server 192.168.1.103;
       }

       server {
           listen 443 ssl;
           ssl_certificate /path/to/certificate.crt;
           ssl_certificate_key /path/to/certificate.key;

           location / {
               proxy_pass http://backend;
           }
       }
   }
   ```

2. HAProxy SSL termination:
   ```
   frontend https_front
       bind *:443 ssl crt /path/to/certificate.pem
       default_backend http_back

   backend http_back
       balance roundrobin
       server server1 192.168.1.101:80 check
       server server2 192.168.1.102:80 check
       server server3 192.168.1.103:80 check
   ```

## Session Persistence

Sometimes it's necessary to route requests from a client to the same server.

1. Nginx sticky sessions:
   ```nginx
   upstream backend {
       ip_hash;
       server 192.168.1.101;
       server 192.168.1.102;
       server 192.168.1.103;
   }
   ```

2. HAProxy sticky sessions:
   ```
   backend http_back
       balance roundrobin
       cookie SERVERID insert indirect nocache
       server server1 192.168.1.101:80 check cookie s1
       server server2 192.168.1.102:80 check cookie s2
       server server3 192.168.1.103:80 check cookie s3
   ```

Remember to monitor your load balancer's performance and adjust your configuration as needed to handle your traffic patterns effectively.