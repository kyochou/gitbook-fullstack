# Grafana

## Query

```json
# 同时查多个 job 中的关键字
{job=~"game0|game1"} |~ "warning|error"
{job=~"game0|game1|bjl"} |= "stock-10-1608608589-827-3164"
```