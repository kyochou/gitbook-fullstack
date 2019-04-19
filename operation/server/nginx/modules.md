# Modules

## Variable
### http_map_module
场景: 基于已有变量, 使用类似 `switch` 的语法创建新变量.

### http_split_clients_module
场景: 基于已有变量, 基于特定百分比算法生成指定的多个变量中的一个. 可用于 AB 测试等功能中.

### http_geo_module
场景: 基于 IP 地址创建新的变量.

### http_geoip_module
场景: 基于 IP 地址自动生成地理位置相关的新变量.

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

#### 验证链接的正确性
思路:
1. 应用服务器使用链接地址和密匙生成带有 hash 值的链接(`/prefix/hash/link`), 返回给客户端. 
2. 客户端访问此链接, Nginx 对此链接的合法性进行判断.

指令:
* secure_link_secret: 生成加密链接时使用的密匙.

变量:
* secure_link: 验证通过返回链接地址, 否则返回空.


#### 验证用户信息和过期时间
思路:
1. 应用服务器生成带有用户信息(如 IP 地址)和过期时间的链接, 返回给客户端. 
2. 客户端访问此链接, Nginx 对此链接的合法性进行判断.

指令:
* secure_link: 定义验证需要的参数值.
* secure_link_md5: 定义加密链接的生成规则.

变量:
* secure_link: 值为空表示验证不通过, 为 0 表示 URL 过期, 为 1 表示验证通过.
* secure_link_expires: 过期时间.