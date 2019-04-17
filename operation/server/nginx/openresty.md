# Openresty
## 架构
* Nginx
* Nginx 官方模块
* Openresty 工具
* Openresty 第三方模块
    其中 `ngx_http_lua_module` 和 `ngx_stream_lua_module` 为 Nginx 操作了 Lua 脚本的支持.
* Lua 语言模块

## 编译
1. 下载源码解压. 执行 `make` 命令.
2. 进入 `make` 生成的 `openresty-<VERSION>` 目录, 执行 `configure` 命令. 如: `./configure --with-openssl=/usr/local/opt/openssl --with-debug --with-http_realip_module`.
    * `configure` 命令生成的 nginx 源码的中间文件位于 `build/nginx-<VERSION>` 目录下.
    * `build/nginx-<VERSION>/objs/ngx_modules.c` 文件中保存了模块相关的信息.