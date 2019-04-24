# OpenResty
OpenResty 是什么? 被扩展的 Nginx, 可以直接在配置文件中执行 Lua 代码, 处理业务逻辑, 访问缓存和数据库等.
OpenResty 可以理解为集成了很多模块的定制加强版 Nginx.
OpenResty 应用开发过程, 主要就是与 OpenResty 添加的 Nginx 模块, 以及各种 Lua 的 Package 打交道的过程.

## 架构
* Nginx
* Nginx 官方模块
* Openresty 工具
* Openresty 第三方模块
    其中 `ngx_http_lua_module` 和 `ngx_stream_lua_module` 为 Nginx 操作了 Lua 脚本的支持.
* Lua 语言模块

## 编译
1. 下载源码解压. 执行 `make` 命令.
2. 进入 `make` 生成的 `openresty-<VERSION>` 目录, 执行 `configure` 命令. 如: `./configure --with-cc-opt="-I/usr/local/opt/openssl@1.1/include/" --with-ld-opt="-L/usr/local/opt/openssl@1.1/lib/" --with-debug --with-http_realip_module --with-http_auth_request_module -j4`.
    * `configure` 命令生成的 nginx 源码的中间文件位于 `build/nginx-<VERSION>` 目录下.
    * `build/nginx-<VERSION>/objs/ngx_modules.c` 文件中保存了模块相关的信息.

### Refs
* [Openresty Installation](https://openresty.org/en/installation.html)
* [MAC 重装各种的痛点](http://homeway.me/2015/07/10/rebuild-osx-environment/)
* 