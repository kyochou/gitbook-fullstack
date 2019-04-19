# Modules

## Request
### http_referer_module
目的: 拒绝非正常的网站访问我们站点的资源.

场景: 某网站通过 URL 引用了你的页面, 当用户在浏览器上点击 URL 时, HTTP 请求的头部会通过 referer 头部将该网站当前页面的 URL 带上, 告诉服务器本次请求是由这个页面发起的.

思路: 通过 referer 模块, 用 `invalid_referer` 变量根据配置判断 referer 头部是否合法.

指令:
* valid_referers: 指定合法的 referers 值.
* referer_hash_bucket_size.
* referer_hash_max_size.

### http_secure_link_module
目的: 实现请求资源的验证.
思路:
1. 应用服务器生成带有用户信息(IP 地址)和过期时间的链接, 返回给客户端. 
2. 客户端访问此链接, Nginx 对此链接的合法性进行判断.

指令:
* secure_link
* secure_link_expires