# Tools

## 抓包
### Refs
* [关于手机App的Https抓包](https://blog.huoding.com/2019/05/31/741)


## Wireshark
### 过滤器
#### 捕获过滤器
用于减少抓取的报文体积. 使用 BPF 语法.
[BPF(Berkeley Packet  Filter)](https://biot.com/capstats/bpf.html), 在设备驱动级别提供抓包过滤接口. BPF 语法适用于很多抓包工具.
BPF 表达式由原语(primitives)和运算符组成.
原语由限定词(qualifiers)和限定条件组成, 如:
* Type: 类型, 如 `host www.baidu.com`;
    * host, port;
    * net: 设定子网. 如 `net 192.168.0.0/24` 等价于 `net 192.168.0.0 mask 255.255.255.0`;
    * portrange, 设定端口范围. 如 `portrange 6000-8000`;
* Dir: 网络出入方向, `src` 表示网络流入, `dst` 表示网络流出. 如 `dst port 80`;
* Proto: 协议, 如 `udp`;
* 其他: geteway, broadcast, multicast, less, greater 等;

运算符有:
* 与: `&&` 或者 `and`;
* 或: `||` 或者 `or`;
* 非: `!` 或者 `not`;

BPF 语法对数据的过滤可以精确到 `bit` 单位. 如:
* 捕获所有TCP中的RST报文: `tcp[13]&4 == 4`;
* 抓取HTTPGET报文: `port 80 and tcp[((tcp[12:1]&0xf0)>>2):4] = 0x47455420`;

#### 显示过滤器
对于已经抓取到的报文过滤显示.
任何在报文细节面板中解析出的字段名, 都可以作为过滤属性. 可在 [Display Filter Reference](https://www.wireshark.org/docs/dfref/) 中查看属性的对应关系.
过滤值比较符号:
* 等于: `eq`, `==`;
* 不等于: `ne`, `!=`;
* 大于: `gt`, `>`;
* 小于: `lt`, `<`;
* 大于等于: `ge`, `>=`;
* 小于等于: `le`, `<=`;
* 包含: `contains`. 如 `sip.To contains "a1762"`;
* 正则匹配: `matches`, `~`. 如 `host matches "baidu\.(com|cn)"`;
* 位与操作: `bitwise_and`, `&`. 如 `tcp.flags & 0x02`;

过滤值类型:
* 整型
* 布尔值
* Ethernet address
* IP 地址
* 字符串

多个表达式间的组合:
* 逻辑与: `and`, `&&`;
* 逻辑或: `or`, `||`;
* 逻辑异或: `xor`, `^^`;
* 逻辑非: `!`;
* 切片操作: `[]`;
* 集合操作: `in`;

集合使用 `{}` 表示. 其内可以使用 `..` 表示范围, 使用空格分隔多个元素. 如 `tcp.port in {443 4430..4434}`.
切片使用 `[]` 表示. 切片中的多个元素使用 `,` 分隔. 操作有:
* `[n:m]` 中 n 为起始偏移量, m 是切片长度. n 的默认值为 0, m 的默认值为切片长度;
* `[n-m]` 中 n 为起始偏移量, m 是截止偏移量;
* `[m]` 表示取偏移量 m 处的字节;

可用函数:
* `upper`
* `lower`
* `len`
* `count`
* `string`