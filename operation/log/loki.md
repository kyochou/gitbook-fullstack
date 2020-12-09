# Loki
## Promtail
### Config

```yaml
## promtail.yml

server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://10.170.0.3:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  pipeline_stages:
    - regex:
        expression: "\\s*time=\"(?P<time>\\S+)\"\\s*"
    # 让 Loki 使用程序中的时间. https://grafana.com/docs/loki/latest/clients/promtail/stages/timestamp/
    - timestamp:
        source: time
        format: RFC3339Nano

  static_configs:
    - targets:
      - localhost
      labels:
        job: center
        __path__: /var/log/supervisor/center-*.log
    - targets:
      - localhost
      labels:
        job: qznn
        __path__: /var/log/supervisor/qznn-*.log
```

## Refs
* [Loki Documentation](https://grafana.com/docs/loki/latest/)