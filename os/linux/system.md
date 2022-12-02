# System
## Systemd
```shell
# 开机自启动
sudo systemctl enable ssh
# 关闭开机自启动
sudo systemctl disable ssh
# 启动服务
sudo systemctl start ssh
# 关闭服务
sudo systemctl stop ssh
# 查看状态
sudo systemctl status ssh

```

### 添加新服务
1. 将新服务的启动脚本如 `supervisord.service` 放入 `/usr/lib/systemd/system` 目录;
2. `sudo systemctl daemon-reload`
3. `sudo systemctl enable supervisord.service`
4. `sudo systemctl start supervisord.service`