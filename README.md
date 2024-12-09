# Grafana Monitoring

Leer:
https://core.telegram.org/bots
https://stackoverflow.com/questions/76323053/alertmanager-send-telegram-messages

## Grafana

Grafana es un software que permite la visualización y el formato de datos métricos. Permite crear cuadros de mando y gráficos a partir de múltiples fuentes.

# OS stats

### Instalación Node exporter

Para monitorear un sistema linux hay que instalar node_exporter. Node exporter se encargará de recolectar métricas en vivo y las deja disponibles para que prometheus realice el scrapping.

Descargar de Node exporter https://prometheus.io/docs/guides/node-exporter/

```bash
$ tar -xvf node_exporter-*.tar.gz
$ sudo mv node_exporter-*/node_exporter /usr/local/bin/
$ sudo useradd -rs /bin/false node_exporter
$ sudo vim /etc/systemd/system/node_exporter.service
```

```bash
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl start node_exporter
$ sudo systemctl enable node_exporter
```

Accede a las métricas en http://<ip_del_servidor>:9100/metrics

### Integración con Prometheus

Edita el archivo de configuración de Prometheus (prometheus.yml) y agregar el job para Node Exporter.

```yml
scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets: ["<ip_del_servidor>:9100"]
```
