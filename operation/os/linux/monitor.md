# Monitor
## Grafana
Grafana 与 Prometheus 是一对多的关系. 即可以在一个 Grafana 应用中添加多个 Prometheus 数据源. 每个 Prometheus 数据源均可配置不同的 metrics.

## Prometheus
时序数据库(Time Series Database)就是用来存储 "随着时间变化而变化的数据" 的数据库.
[Prometheus](https://prometheus.io/) 就是用 Go 编写的时序数据库.

1. 使用 Ansible 安装相关软件:
    * [cloudalchemy/ansible-prometheus](https://github.com/cloudalchemy/ansible-prometheus)
    * [cloudalchemy/ansible-grafana](https://github.com/cloudalchemy/ansible-grafana)
    * [cloudalchemy/ansible-node-exporter](https://github.com/cloudalchemy/ansible-node-exporter)
    * [cloudalchemy/ansible-process_exporter](https://github.com/cloudalchemy/ansible-process_exporter)

1. 在 `/etc/prometheus/prometheus.yml` 文件中添加相应的 Exporter 配置:

    ```yaml
    # /etc/prometheus/prometheus.yml
    scrape_configs:
      - job_name: node-exporter
        metrics_path: /metrics
        static_configs:
        - targets: ['127.0.0.1:9100']
      - job_name: process-exporter
        metrics_path: /metrics
        static_configs:
        - targets: ['127.0.0.1:9256']
    ```

1. 访问 `http://<IP>:9090/targets` 查看是否配置正确.
2. 打开安装的 [Grafana](http://<IP>:3000), 将相关 Exporter 的 DashBoard 导入.
    1. 在 [Dashboards](https://grafana.com/grafana/dashboards) 中找到相关 Exporter 的 Dashboard. 记下其 ID.
    2. 在安装的 Grafana 页面上选择 `+ -> Import`, 输入 ID.
        * 11074: [Node Exporter for Prometheus Dashboard](https://grafana.com/grafana/dashboards/11074)
        * 4202: [Named processes by host](https://grafana.com/grafana/dashboards/4202)
        
            ```json
            ## 自定义 Panel
            # 进程的 CPU 使用时间. 可以认为单个 CPU 被完全占用的值为一秒. 用输出的值除以单位秒即可做为进程的 CPU 使用率.
            # Legend {{groupname}}
            sum without (mode)(
        rate(namedprocess_namegroup_cpu_seconds_total{groupname=~"$processes",instance=~"$host"}[$interval])
)
        ```

### Refs
* [Prometheus 集成 Node Exporter](https://juejin.im/post/5d54bc80f265da03a6531063)
* [研究监控系统之prometheus](https://jeremyxu2010.github.io/2018/08/%E7%A0%94%E7%A9%B6%E7%9B%91%E6%8E%A7%E7%B3%BB%E7%BB%9F%E4%B9%8Bprometheus/)
* [prometheus 入门与思考](http://tech.dianwoda.com/2018/08/15/prometheus-ru-men-yu-si-kao/)
* [使用 Prometheus 监控服务器性能](https://cjting.me/2017/03/12/use-prometheus-to-monitor-server/)

## Tools
* [netdata/netdata](https://github.com/netdata/netdata): Real-time performance monitoring.