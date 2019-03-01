# Secure

## 服务
需对外提供服务的程序, 均应该使用 shell 为 `/sbin/nologin` 的用户运行.

## sudo

### 参数
* -E: 在 sudo 命令中使用当前用户的环境变量
* -u <user>: 使用指定的 user 执行命令.
* -i: 切换至指定用户 shell (默认 root)执行命令.  