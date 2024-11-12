Configuración Prometheus Node, y Grafana

useradd -m -s /bin/false prometheus

mkdir /etc/prometheus
mkdir /var/lib/prometheus
chown -R prometheus:prometheus /var/lib/prometheus/

https://prometheus.io/download/

tar -zxpvf prometheus-2.42.0.linux-amd64.tar.gz
cd prometheus-2.42.0.linux-amd64/
cp promtool /usr/local/bin
cp prometheus /usr/local/bin

sudo nano /etc/prometheus/prometheus.yml
chown -R prometheus:prometheus /etc/prometheus/prometheus.yml


# Configuración Global.
global:
  scrape_interval: 15s 
  evaluation_interval: 15s
  scrape_timeout: 15s  
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090'] 

sudo firewall-cmd --add-port=9090/tcp --permanent
sudo firewall-cmd --reload

sudo nano /etc/systemd/system/prometheus.service

Description=Prometheus Time Series Collection and Processing Server
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --storage.tsdb.retention.time=30d \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target



sud systemctl enable prometheus
sudo systemctl status prometheus


useradd -m -s /bin/false node_exporter

tar -zxpvf node_exporter-1.5.0.linux-amd64.tar.gz
cd node_exporter-1.5.0.linux-amd64
cp node_exporter /usr/local/bin/

chown -R node_exporter:node_exporter /usr/local/bin/node_exportercd
nano /etc/systemd/system/node_exporter.service

systemctl status node_exporter

systemctl status grafana-server

systemctl status prometheus

