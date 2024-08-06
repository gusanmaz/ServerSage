# Monitoring and Logging Guide

This guide covers setting up monitoring tools like Prometheus, Grafana, and the ELK stack on a VPS.

## Table of Contents

1. [Prometheus](#prometheus)
2. [Grafana](#grafana)
3. [ELK Stack](#elk-stack)
4. [Log Rotation](#log-rotation)

## Prometheus

Prometheus is an open-source systems monitoring and alerting toolkit.

### Installation

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz
tar xvf prometheus-2.30.3.linux-amd64.tar.gz
cd prometheus-2.30.3.linux-amd64/
```

### Configuration

Edit `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

### Running Prometheus

```bash
./prometheus --config.file=prometheus.yml
```

## Grafana

Grafana is an open-source platform for monitoring and observability.

### Installation

```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana
```

### Start Grafana

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Access Grafana at `http://your-server-ip:3000`

## ELK Stack

ELK Stack consists of Elasticsearch, Logstash, and Kibana.

### Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
```

### Logstash

```bash
sudo apt install logstash
```

### Kibana

```bash
sudo apt install kibana
```

### Configuration

1. Edit Elasticsearch configuration:
   ```bash
   sudo nano /etc/elasticsearch/elasticsearch.yml
   ```

2. Edit Logstash configuration:
   ```bash
   sudo nano /etc/logstash/logstash.yml
   ```

3. Edit Kibana configuration:
   ```bash
   sudo nano /etc/kibana/kibana.yml
   ```

### Start Services

```bash
sudo systemctl start elasticsearch
sudo systemctl start logstash
sudo systemctl start kibana
```

## Log Rotation

Use logrotate to manage log files:

1. Install logrotate:
   ```bash
   sudo apt install logrotate
   ```

2. Create a configuration file:
   ```bash
   sudo nano /etc/logrotate.d/myapp
   ```

3. Add configuration:
   ```
   /var/log/myapp/*.log {
       weekly
       rotate 4
       compress
       delaycompress
       missingok
       notifempty
       create 0640 www-data www-data
   }
   ```

Remember to regularly review your logs and adjust your monitoring setup as your system evolves.